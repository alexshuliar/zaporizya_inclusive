<template>
  <div
    class="submit-panel"
    v-if="coordsApproved"
    v-on:click.self="$emit('changePointStatus'), clearInputs()"
  >
    <div class="submit-card">
      <div class="close-card-wrapper">
        <div
          class="close-card"
          v-on:click="$emit('changePointStatus'), clearInputs()"
        ></div>
      </div>
      <div class="form-container">
        <div class="form-field">
          <label for="name">Назва будівлі</label>
          <input type="text" name="name" id="name" v-model="userBuildingName" />
        </div>
        <div class="form-field">
          <label for="lat" class="lat">Координати точки</label>
          <input
            type="text"
            name="latlng"
            id="lat"
            readonly="readonly"
            :value="coordsApproved.lat"
          />
          <label for="lng" class="lng"></label>
          <input
            type="text"
            name="latlng"
            id="lng"
            readonly="readonly"
            :value="coordsApproved.lng"
          />
        </div>

        <div class="submit-button">
          <!-- <input type="submit" value="Додати точку" /> -->
          <button type="button" v-on:click="submitPoint">Продовжити</button>
        </div>
        <div class="message-success">
          <p v-show="userFilledObject">
            Дані відправлено. Після перевірки, точка з'явиться на мапі. Дякуємо
            за те, що допомагаєте місту статим кращим!
          </p>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  props: ["coordsApproved"],
  data() {
    return {
      userBuildingName: "",
      userFilledObject: undefined,
    };
  },
  methods: {
    submitPoint() {
      if (this.userBuildingName) {
        this.userFilledObject = {
          name: this.userBuildingName,
          lat: this.coordsApproved.lat,
          lng: this.coordsApproved.lng,
        };

        this.sendToSheet();
      }
    },
    clearInputs() {
      this.userFilledObject = undefined;
      this.userBuildingName = undefined;
    },
    sendToSheet() {
      // setTimeout(() => console.log(this.coordsClicked), 200);

      let url =
        "https://script.google.com/macros/s/AKfycbwZxJ-ynVAhz6KrT5_n4R6TTaJiae6QLke_YhZn/exec";
      fetch(url, {
        method: "POST", // или 'PUT'
        mode: "no-cors",
        cache: "no-cache",
        redirect: "follow",
        body: JSON.stringify({
          name: this.userFilledObject.name,
          lat: this.userFilledObject.lat,
          lng: this.userFilledObject.lng,
          checked: false,
        }), // данные могут быть 'строкой' или {объектом}!
        headers: {
          "Content-Type": "application/json",
        },
      });
    },
  },
};
</script>


<style lang="scss">
.submit-panel {
  position: fixed;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  background-color: black;
  background: rgba(0, 0, 0, 0.6);
  z-index: 1100;
  .submit-card {
    position: relative;
    background-color: white;
    top: 15%;
    left: 30%;
    // height: 580px;
    // width: 512px;
    width: 512px;
    .close-card-wrapper {
      position: relative;
      .close-card {
        position: absolute;
        top: 0;
        right: 0;
        height: 32px;
        width: 32px;
        padding: 3px;
        cursor: pointer;
        &:before,
        &:after {
          content: "";
          position: absolute;
          top: 8px;
          right: 16px;
          color: white;
          height: 14px;
          // border: solid 1px rgb(160, 160, 255);
          border: solid 1px black;
          transform: rotate(45deg);
        }
        &:after {
          transform: rotate(135deg);
        }
      }
    }
    .form-container {
      // display: flex;
      padding: 64px 64px 32px 64px;
      .form-field {
        margin-bottom: 32px;
        // margin-right: 32px;
        label {
          display: block;
          margin-bottom: 8px;
          &.lng {
            display: none;
          }
        }

        input {
          // height: 32px;
          // height: 1.1875em;
          border: none;
          border-bottom: 2px solid black;
          display: block;
          padding: 6px 0 7px;
          width: 328px;
          // &#name {
          //   width: 256px;
          // }
          &#lat,
          &#lng {
            // width: 256px;
            cursor: auto;
            color: grey;
          }
          &#lat {
            margin-bottom: 8px;
          }
        }
        textarea:focus,
        input:focus {
          outline: none;
        }
      }

      .submit-button {
        margin-bottom: 32px;
        button {
          text-align: center;
          box-sizing: border-box;
          background-color: black;
          color: white;
          border-radius: 40px;
          min-width: 112px;
          font-size: 16px;
          min-height: 40px;
          padding: 16px 68px;
          outline: none;
          cursor: pointer;
          border: 0;
        }
      }
      .message-success {
        height: 48px;
        font-size: 12px;
      }
    }
  }
}
</style>