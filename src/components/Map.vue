<template>
  <div class="map-container">
    <div
      id="map"
      v-on:click.meta="pointMaker"
      v-on:keydown.91="pointMode('on')"
      v-on:keyup.91="pointMode('off')"
      v-on:keydown.17="pointMode('on')"
      v-on:keyup.17="pointMode('off')"
    ></div>
    <div class="map-bar">
      <div class="search-street-bar">
        <input
          type="text"
          placeholder="Назва вулиці або організації"
          list="name"
          v-model="selectedObject"
        />
        <datalist id="name">
          <option
            v-for="(optionName, idx) in streetsDatalist"
            v-bind:key="idx"
          >
            {{ optionName }}
          </option>
        </datalist>
        <div class="search-icon"></div>
      </div>
      <div class="building-types-marks">
        <p class="sub-bar-title">Тип організації:</p>
        <div class="section-markers">
          <div
            class="color-marker"
            v-for="name of sectionNames"
            v-bind:key="name"
          >
            <div
              class="m-circle"
              :style="{ backgroundColor: colors[name] }"
            ></div>
            <p class="m-title">
              {{ name }}
            </p>
          </div>
        </div>
      </div>
      <div class="facilities-filters">
        <p class="sub-bar-title">Засоби доступності:</p>
        <div class="facilities-buttons">
          <div
            class="f-filter pandus active"
            v-on:click="filterMap"
          >
            <div class="search-mark"></div>
            <p class="f-title">Пандус</p>
          </div>
          <div
            class="f-filter toilet active"
            v-on:click="filterMap"
          >
            <div class="search-mark"></div>
            <p class="f-title">Туалет</p>
          </div>
          <div
            class="f-filter parking active"
            v-on:click="filterMap"
          >
            <div class="search-mark"></div>
            <p class="f-title">Парковка</p>
          </div>
          <div
            class="f-filter other active"
            v-on:click="filterMap"
          >
            <div class="search-mark"></div>
            <p class="f-title">Інше</p>
          </div>
        </div>
      </div>
      <!-- <div class="add-object-info">
        * Текст про те, що кожен може додати об’єкт на карту. Для цього
        перейдіть в режим редагування
      </div> -->
    </div>
    <form-submit
      :coordsApproved="coordsApproved"
      v-on:changePointStatus="clearPoint"
    ></form-submit>
  </div>
</template>

<script>
import L from "leaflet";
import "leaflet/dist/leaflet.css";
import FormSubmit from "@/components/FormSubmit";

import icon from "leaflet/dist/images/marker-icon.png";
import iconShadow from "leaflet/dist/images/marker-shadow.png";

export default {
  // components: { L },
  components: { FormSubmit },
  props: ["jsonFile", "sectionNames"],

  data() {
    return {
      filterStatus: [
        { name: "pandus", status: true },
        { name: "toilet", status: true },
        { name: "parking", status: true },
        { name: "other", status: true },
      ],
      colors: {
        Адмінбудівлі: "#000000",
        "Соціальна сфера": "#AC468A",
        "Житлові будинки": "#4762AB",
        Школи: "#878787",
        "Клінічні заклади": "#939CCF",
        "Культурні заклади": "#C98BBC",
        Інфраструктура: "#C98BBC",
      },
      map: undefined,
      markers: undefined,
      selectedObject: undefined,
      buildingClicked: undefined,
      buildingPopup: undefined,
      coordsClicked: undefined,
      coordsApproved: undefined,
    };
  },
  methods: {
    filterMap(event) {
      event.target.parentNode.classList.toggle("active");

      let markSelected = event.target.parentNode.classList[1];

      this.filterStatus = this.filterStatus.map((filterObj) => {
        if (filterObj["name"] === markSelected) {
          filterObj["status"] = !filterObj["status"];

          return filterObj;
        } else {
          return filterObj;
        }
      });
    },
    pointMaker() {
      setTimeout(() => {
        this.coordsApproved = this.coordsClicked;
      }, 100);
    },
    // pointMaker(event) {
    //   console.log(event);
    //   // setTimeout(() => console.log(this.coordsClicked), 200);

    //   let url =
    //     "https://script.google.com/macros/s/AKfycbwZxJ-ynVAhz6KrT5_n4R6TTaJiae6QLke_YhZn/exec";
    //   fetch(url, {
    //     method: "POST", // или 'PUT'
    //     mode: "no-cors",
    //     cache: "no-cache",
    //     redirect: "follow",
    //     body: JSON.stringify({ lat: 43, lng: 45 }), // данные могут быть 'строкой' или {объектом}!
    //     headers: {
    //       "Content-Type": "application/json",
    //     },
    //   });
    // },
    clearPoint() {
      this.coordsApproved = undefined;
    },
    pointMode(mode) {
      let markersList = Object.values(this.markers._layers);
      if (mode === "on") {
        this.$el.querySelector("#map").style.cursor = "crosshair";
        this.$el.querySelector("#map").style.opacity = 0.6;

        let activePopup = this.$el.querySelector(".leaflet-popup");
        if (activePopup) {
          activePopup.remove();
        }

        markersList.forEach((marker) => {
          marker.on("mouseover", function (m) {
            // m.target._path.style.cursor = "crosshair";
            m.target.closePopup();
          });
        });
      } else {
        this.$el.querySelector("#map").style.cursor = "default";
        this.$el.querySelector("#map").style.opacity = 1;

        markersList.forEach((marker) => {
          marker.on("mouseover", function (m) {
            // m.target._path.style.cursor = "pointer";
            m.target.openPopup();
          });
        });
      }
    },
  },
  computed: {
    activeFilters() {
      return this.filterStatus
        .filter((filterObj) => filterObj["status"])
        .map((filterObj) => filterObj["name"]);
    },
    jsonPoints() {
      if (this.selectedObject) {
        return this.jsonFile.filter(
          (record) =>
            record["building_name"] + " " + record["coord_address"] ===
            this.selectedObject
        );
      } else {
        let jsonAggr = [];

        this.jsonFile.forEach((record) => {
          this.activeFilters.forEach((filterName) => {
            if (record[filterName] === true) {
              jsonAggr.push(record);
            }
          });
        });

        return jsonAggr;
      }
    },
    streetsDatalist() {
      return this.jsonFile.map(
        (record) => record["building_name"] + " " + record["coord_address"]
      );
    },
  },
  watch: {
    jsonFile() {
      let vueContext = this;
      let DefaultIcon = L.icon({
        iconUrl: icon,
        shadowUrl: iconShadow,
      });
      L.Marker.prototype.options.icon = DefaultIcon;
      let zoom = 11.5;

      let map = L.map("map", {
        zoomSnap: 0.1,
      }).setView([47.84, 35.13], zoom);
      L.tileLayer(
        "https://stamen-tiles-{s}.a.ssl.fastly.net/toner-lite/{z}/{x}/{y}{r}.png",
        {
          attribution:
            '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> | Map tiles by <a href="http://stamen.com">Stamen Design</a>, under <a href="http://creativecommons.org/licenses/by/3.0">CC BY 3.0</a>.',
        }
      ).addTo(map);

      map.on("mouseover", function (e) {
        e.target._container.focus();
      });

      map.on("click", function (e) {
        vueContext.coordsClicked = e.latlng;
      });

      this.map = map;
    },
    jsonPoints() {
      if (this.markers) {
        this.map.removeLayer(this.markers);
      }

      let vueContext = this;
      let markersGroup = L.layerGroup();

      this.jsonPoints.forEach((d) => {
        if (d.Latitude && d.Longitude) {
          let marker = L.circleMarker([d.Latitude, d.Longitude], {
            // color: "#234463",
            color: this.colors[d.building_type],
            fillOpacity: 1,
            radius: 3,
          }).bindPopup(function () {
            vueContext.buildingClicked = d.bid;
            vueContext.buildingPopup = this._contentNode;

            return (
              d.building_name +
              "<br>" +
              (d.building_address
                ? d.building_address.split(",").slice(1) + "<br>"
                : "") +
              "<p class='card-link'>Клікніть, щоб переглянути картку об'єкту</p>"
            );
          });

          // .addTo(this.map);

          marker.on("mouseover", function (m) {
            m.target.openPopup();
          });

          markersGroup.addLayer(marker);
        }
      });

      this.markers = markersGroup;

      markersGroup.addTo(this.map);
    },
    buildingClicked() {
      let vueContext = this;
      // console.log(this.buildingPopup.querySelector(".card-link"));

      vueContext.buildingPopup
        // .querySelector("p")
        .addEventListener("click", () => {
          // console.log("Success on " + vueContext.buildingClicked);

          this.$emit("sendScrollObject", this.buildingClicked);
          // vueContext.$el.parentNode
          //   .querySelector("#b" + vueContext.buildingClicked)
          //   .scrollIntoView();
        });
    },
  },
};
</script>


<style lang="scss">
.map-container {
  display: grid;
  grid-template-columns: 60% 40%;
  margin: 0px auto 0px auto;
  margin-bottom: 80px;
  #map {
    // width: 500px;
    height: 500px;
    .leaflet-popup-tip,
    .leaflet-popup-content-wrapper {
      // background: #e0e0e0;
    }
  }
  .leaflet-container {
    background: #e5ebe6;
    outline: 0;
  }
  .leaflet-popup {
    cursor: pointer;
    .card-link {
      font-size: 11px;
      text-decoration: underline;
      // cursor: pointer;
    }
  }

  .map-bar {
    background-color: #e5eef3;
    padding: 24px 24px;
    .search-street-bar {
      width: 75%;
      margin-bottom: 40px;
      display: flex;
      position: relative;
      input {
        background-color: #e5eef3;
        width: 100%;
        padding: 8px 24px 8px 24px;
        font-size: 16px;
        text-overflow: ellipsis;
        border: 0.5px solid black;
        box-sizing: border-box;
        border-radius: 100px;
        outline: none;
        &::-webkit-calendar-picker-indicator {
          display: none;
        }
      }
      .search-icon {
        background-image: url(../assets/images/search-icon.svg);
        width: 24px;
        height: 24px;
        position: absolute;
        right: 20px;
        top: 50%;
        margin-top: -13px;
      }
    }
    .building-types-marks {
      font-size: 12px;
      margin-bottom: 40px;
      .section-markers {
        display: flex;
        flex-wrap: wrap;
        .color-marker {
          margin: 0 16px 8px 0;
          display: flex;
          align-items: center;
          .m-circle {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            margin-right: 8px;
            // background-color: #234463;
          }
        }
      }
    }
    .sub-bar-title {
      margin-bottom: 8px;
    }
    .facilities-filters {
      font-size: 12px;
      margin-bottom: 120px;
      .facilities-buttons {
        .f-filter {
          margin-bottom: 8px;
          display: flex;
          align-items: center;
          position: relative;
          cursor: pointer;
          .search-mark {
            margin-right: 8px;
            position: relative;
            box-sizing: border-box;
            &:before {
              content: "";
              background: 0 0;
              border: 2px solid #000;
              height: 12px;
              width: 12px;
              transition: background 0.4s;
              display: block;
            }
            &:after {
              content: "";
              background: url(../assets/images/check-black2.svg) center
                no-repeat;
              position: absolute;
              display: block;
              transition: opacity 0.3s;
              height: 16px;
              width: 16px;
              top: 0;
              left: 1%;
              opacity: 0;
            }
          }
          &.active {
            .search-mark:after {
              opacity: 1;
            }
          }
        }
      }
    }
  }
}
@media (max-width: 480px) {
  .map-container {
    display: block;
    .map-bar {
      padding: 24px 16px;
      .search-street-bar {
        width: 100%;
      }
      .building-types-marks {
        .section-markers {
          .color-marker {
            margin: 0 8px 8px 0;
          }
        }
      }
      .facilities-filters {
        margin-bottom: 0;
        .facilities-buttons {
          display: flex;
          flex-wrap: wrap;
          .f-filter {
            margin-right: 16px;
          }
        }
      }
    }
  }
}
</style>