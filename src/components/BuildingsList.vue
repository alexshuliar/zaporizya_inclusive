<template>
  <div class="buildings-list">
    <div class="section-headers">
      <div
        class="s-name"
        v-for="name of sectionNames"
        v-bind:key="name"
        v-on:click="chooseSection"
      >
        <p
          class="s-title"
          :class="'s-title ' + (name === currentSection ? 'is-selected' : '')"
        >
          {{ name }}
        </p>
      </div>
    </div>
    <div
      class="b-card"
      v-for="record of jsonFiltered"
      v-bind:key="record.object_name_and_address"
      :id="'b' + record.bid"
    >
      <div
        class="b-header"
        v-on:click="toogleFilter"
      >
        <p class="b-title">
          {{ record["building_name"] }}
        </p>
      </div>
      <div class="b-body">
        <div class="data-field address">
          <div class="field-header">Адреса:</div>
          <div class="field-content">{{ record.coord_address }}</div>
        </div>
        <div class="data-field facilities-active">
          <div class="field-header">Наявні засоби доступності:</div>
          <div class="field-content">
            {{ record.facilities ? record.facilities : "Дані відсутні" }}
          </div>
        </div>
        <div class="data-field facilities-need">
          <div class="field-header">Засоби, що потрібно встановити:</div>
          <div class="field-content">
            {{ record.works_need == '-' ? "Не вказані" : record.works_need }}
          </div>
        </div>
        <div class="data-field finances">
          <div class="field-header">Обсяг фінансування, грн:</div>
          <div :class="'field-content' + (record.funds_real == '-' ? ' no-finance': '' ) ">{{ record.funds_real == '-' ? "Дані щодо фінансування по даному об'єкту відсутні" : record.funds_real}}
            <span class="hover-icon"></span>
            <span class="hover-text">Ви можете спробувати знайти відповідні ремонтні роботи у закупівлях організації клікнувши на картку нижче</span>
          </div>
        </div>
        <div class="data-field date-check">
          <div class="field-header">Дата перевірки:</div>
          <div class="field-content">
            {{
              record.date_inspected ? record.date_inspected : "Дані відсутні"
            }}
          </div>
        </div>
        <div
          class="data-field org-name"
          v-on:click="getEntityCode"
        >
          <div class="field-header">Організація:</div>
          <div
            class="field-content"
            v-if="record.NAME"
          >
            <div
              class="text-content"
              v-on:mouseover="(infoMessage = true), (currentHover = record.bid)"
              v-on:mouseleave="infoMessage = false"
            >
              {{ record.NAME }}
            </div>
            <div id="doc-icon">
              <img
                src="../assets/images/ic_document.png"
                alt=""
              />
            </div>
            <div :class="
                'info-message' +
                (infoMessage && currentHover == record.bid ? ' open' : '')
              ">
              <p>Натисніть на назву організації, щоб переглянути її картку</p>
            </div>
          </div>
          <div
            class="empty-org"
            v-else
          >
            <div class="text-content">Дані відсутні</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["jsonFile", "sectionNames", "scrollTo"],
  data() {
    return {
      currentSection: "Адмінбудівлі",
      infoMessage: false,
      currentHover: undefined,
    };
  },
  mounted() {
    // this.$el.querySelector(".s-title").classList.add("is-selected");
  },
  methods: {
    chooseSection(event) {
      event.target.parentNode.parentNode
        .querySelectorAll(".s-title")
        .forEach((title) => {
          title.classList.remove("is-selected");
        });
      event.target.classList.toggle("is-selected");
      this.currentSection = event.target.innerHTML;
    },
    toogleFilter(event) {
      let blockEl = event.currentTarget.parentNode;

      blockEl.classList.toggle("is-open");
    },
    getEntityCode(event) {
      let bid;
      if (event.target.parentNode.id == "doc-icon") {
        bid = parseInt(
          event.target.parentNode.parentNode.parentNode.parentNode.parentNode.id.slice(
            1
          )
        );
      } else {
        bid = parseInt(
          event.target.parentNode.parentNode.parentNode.parentNode.id.slice(1)
        );
      }

      let entity = this.jsonFile.filter((record) => record["bid"] === bid)[0];

      let entityName = entity["NAME"];
      let edrpou = entity["EDRPOU"];

      this.$emit("sendEntityCode", { edrpou: edrpou, name: entityName });
    },
  },
  computed: {
    jsonFiltered() {
      return this.jsonFile.filter(
        (record) => record["building_type"] === this.currentSection.trim()
      );
    },
  },
  watch: {
    scrollTo() {
      if (this.scrollTo !== undefined) {
        let sectionSelected = this.jsonFile.filter(
          (record) => record["bid"] === this.scrollTo
        )[0]["building_type"];

        this.currentSection = sectionSelected;

        let vueContext = this;

        setTimeout(function () {
          vueContext.$el
            .querySelector("#b" + vueContext.scrollTo)
            .scrollIntoView({ block: "center", behavior: "smooth" });

          vueContext.$el
            .querySelector("#b" + vueContext.scrollTo)
            .classList.toggle("is-open");
        }, 200);
      }
    },
  },
};
</script>

<style lang="scss">
.buildings-list {
  margin: 0px auto 0px auto;
  .section-headers {
    display: flex;
    margin-bottom: 32px;
    .s-name {
      margin-right: 24px;
      cursor: pointer;
      .s-title {
        padding: 0px 0 6px 0;
        &.is-selected {
          border-bottom: 2px solid black;
        }
      }
    }
  }
  @media (max-width: 768px) {
    .section-headers {
      font-size: 14px;
    }
  }
  @media (max-width: 480px) {
    .section-headers {
      flex-wrap: wrap;
      font-size: 14px;
      .s-name {
        margin-bottom: 8px;
      }
    }
  }
  .b-card {
    // height: min-content;
    font-size: 12px;
    // margin: 0 0 0 8px;
    padding: 16px 0 16px 0;
    border-bottom: 1px solid black;
    transition: background-color 0.5s ease-in-out;
    &:nth-child(2) {
      border-top: 1px solid black;
    }
    &.is-open {
      // .search-options,
      // .search-selector {
      //   max-height: 1000px;
      //   padding: 16px 0 0 0;
      // }
      .b-header {
        &:before {
          transform: rotate(90deg);
        }
      }
      .b-body {
        // display: block;
        transition: all 0.5s ease-in-out;
        opacity: 1;
        max-height: 1000px;
        overflow: hidden;
        padding-top: 16px;
        .data-field {
          margin-bottom: 8px;
        }
      }
    }
    .b-header {
      display: flex;
      flex-direction: row;
      position: relative;
      cursor: pointer;
      // margin-bottom: 16px;
      &:before,
      &:after {
        position: absolute;
        top: 0;
        right: 8px;
        width: 1px;
        height: 16px;
        content: "";
        background-color: #000;
        transition: 0.3s ease-in-out;
      }
      &:after {
        transform: rotate(90deg);
      }
      .b-title {
        transition: 0.5s;
        margin: 0;
      }
      @media (max-width: 480px) {
        .b-title {
          width: 85%;
        }
      }
    }
    .b-body {
      // display: none;
      opacity: 0;
      max-height: 0;
      height: auto;
      padding-top: 0;
      transition: max-height 0.5s cubic-bezier(0, 1, 0, 1), padding 0.6s ease,
        opacity 0.5s cubic-bezier(0, 1, 0, 1);
      overflow: hidden;
      .data-field {
        display: flex;
        margin-bottom: 8px;
        align-items: flex-start;
        position: relative;
        .field-header {
          margin-right: 16px;
        }
        .field-content,
        .empty-org {
          font-size: 16px;
          max-width: 82%;
          margin-top: -5.5px;
          line-height: 1.5em;
        }
        .field-content.no-finance {
          .hover-icon {
            content: "";
            background: url(../assets/images/ic_hint.png) no-repeat;
            background-size: 24px 24px;
            display: inline-block;
            width: 24px;
            height: 24px;
            margin: -6px 8px;
            // cursor: pointer;
          }
          .hover-text {
            transition: opacity 0.5s ease;
            background-color: #fff8e3;
            position: absolute;
            // width: 256px;
            width: 300px;
            right: 3%;
            top: -88px;
            padding: 24px 36px;
            cursor: pointer;
            border-radius: 50px;
            z-index: 1;
            opacity: 0;
          }
          .hover-icon:hover + .hover-text {
            opacity: 1;
          }
        }
        @media (max-width: 1280px) {
          .field-content.no-finance {
            .hover-icon,
            .hover-text {
              display: none;
            }
          }
        }
        &.org-name {
          margin-top: 24px;
          cursor: pointer;
          .field-content {
            display: flex;
            .text-content {
              text-decoration: underline;
              margin-right: 24px;
              // flex: 0 0 auto;
            }
            #doc-icon {
              margin-top: -14px;
              margin-right: 16px;
              padding: 8px;
              &:hover + .info-message {
                opacity: 1;
              }
            }
            .info-message {
              // cursor: default;
              pointer-events: none;
              position: absolute;
              top: -80px;
              right: 0px;
              opacity: 0;
              // margin-top: -56px;
              transition: opacity 0.5s ease;
              padding: 16px 24px;
              border-radius: 50px;
              // padding: 16px;
              background-color: #fff8e3;
              &.open {
                opacity: 1;
              }
            }
          }
        }
      }
      @media (max-width: 480px) {
        .data-field {
          flex-direction: column;
          margin-bottom: 16px;
          &:first-child {
            margin-top: 8px;
          }
          &.org-name {
            .field-content {
              .text-content {
                flex: 0 0 90%;
              }
              #doc-icon {
                margin-top: 0;
              }
              .info-message {
                display: none;
              }
            }
          }
          .field-header {
            margin-bottom: 16px;
          }
        }
      }
    }
  }
}
</style>