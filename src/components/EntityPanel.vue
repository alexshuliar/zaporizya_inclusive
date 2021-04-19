<template>
  <div
    tabindex="-1"
    class="entity-panel"
    v-if="currentEntity"
    v-on:click.self="$emit('changeEntityStatus')"
  >
    <div class="entity-card">
      <div class="close-card-wrapper">
        <div class="close-card" v-on:click="$emit('changeEntityStatus')"></div>
      </div>
      <div class="entity-content">
        <div class="data-fields">
          <div class="field full-name">
            <!-- <span class="title">Повна назва:</span> -->
            <span class="content">{{ currentEntityName }}</span>
          </div>
          <div class="field edrpou">
            <span class="title">Код ЄДРПОУ:</span>
            <span class="content">{{ currentEntity }}</span>
          </div>
          <div class="ring-link">
            <a
              :href="'https://ring.org.ua/edr/uk/company/' + currentEntity"
              target="_blank"
              >Переглянути дані щодо юридичної особи на порталі RING</a
            >
          </div>
        </div>
        <div class="views-headers">
          <div
            class="view-name"
            v-on:click="currentView = $event.target.innerHTML.trim()"
          >
            <p
              :class="
                'view-title ' + (currentView === 'Дозволи' ? 'is-selected' : '')
              "
            >
              Дозволи
            </p>
          </div>
          <div
            class="view-name"
            v-on:click="currentView = $event.target.innerHTML.trim()"
          >
            <p
              :class="
                'view-title ' + (currentView === 'Тендери' ? 'is-selected' : '')
              "
            >
              Тендери
            </p>
          </div>
          <div
            class="view-name"
            v-on:click="currentView = $event.target.innerHTML.trim()"
          >
            <p
              :class="
                'view-title ' +
                (currentView === 'Позатендерні закупівлі' ? 'is-selected' : '')
              "
            >
              Позатендерні закупівлі
            </p>
          </div>
        </div>
        <div class="table-wrapper" v-if="currentView === 'Дозволи'">
          <div class="external-link-info" v-if="currentRegistry.length">
            Клікніть на будь-якому рядку таблиці, щоб відкрити сторінку цього
            документу на
            <a href="https://e-construction.gov.ua/"
              >Порталі державної електронної системи у сфері будівництва</a
            >
          </div>
          <table-view
            :jsonDataset="currentRegistry"
            v-if="currentRegistry.length"
          ></table-view>
          <div class="no-data-found" v-else>
            <p>
              На жаль, по цій організації відсутні дані про {{ currentView }} :(
            </p>
          </div>
        </div>
        <div class="table-wrapper" v-if="currentView === 'Тендери'">
          <div class="external-link-info" v-if="currentTenders.length">
            Клікніть на будь-якому рядку таблиці, щоб відкрити сторінку тендеру
            на
            <a href="https://prozorro.gov.ua/">Prozorro</a>
          </div>
          <table-view
            :jsonDataset="currentTenders"
            v-if="currentTenders.length"
          ></table-view>
          <div class="no-data-found" v-else>
            <p>
              На жаль, по цій організації відсутні дані про {{ currentView }} :(
            </p>
          </div>
        </div>
        <div
          class="table-wrapper"
          v-if="currentView === 'Позатендерні закупівлі'"
        >
          <div class="external-link-info" v-if="currentSpends.length">
            Клікніть на будь-якому рядку таблиці, щоб відкрити сторінку
            закупівлі на
            <a href="https://spending.gov.ua/"
              >Єдиному веб-порталі використання публічних коштів</a
            >
          </div>
          <table-view
            :jsonDataset="currentSpends"
            v-if="currentSpends.length"
          ></table-view>
          <div class="no-data-found" v-else>
            <p>
              На жаль, по цій організації відсутні дані про {{ currentView }} :(
            </p>
          </div>
        </div>
        <!-- <div class="no-data-found" v-else>
          <p>
            На жаль, по цій організації відсутні дані про
            {{ currentView }} :( &#128577;
          </p>
        </div> -->
      </div>
    </div>
  </div>
</template>


<script>
import TableView from "@/components/Grid";

export default {
  components: { TableView },
  props: ["currentEntity", "currentEntityName"],
  data() {
    return {
      closePanel: this.currentEntity ? true : false,
      jsonRegistry: [],
      jsonTenders: [],
      jsonSpends: [],
      currentRegistry: undefined,
      currentTenders: undefined,
      currentSpends: undefined,
      currentView: "Дозволи",
    };
  },
  watch: {
    currentEntity() {
      this.currentRegistry = this.jsonRegistry.filter(
        (item) => item["obj_edrpou_code"] === this.currentEntity
      );
      this.currentTenders = this.jsonTenders.filter(
        (item) => item["obj_edrpou_code"] === this.currentEntity
      );
      this.currentSpends = this.jsonSpends.filter(
        (item) => item["edrpou"] === this.currentEntity
      );
    },
  },
  mounted() {
    this.jsonRegistry = require("@/assets/data/all_registry_v2.json");
    this.jsonTenders = require("@/assets/data/r_tenders_data_v2.json");
    this.jsonSpends = require("@/assets/data/r_spending_data_v2.json");
  },
};
</script>

<style lang="scss">
.entity-panel {
  //   display: none;
  position: fixed;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  background-color: black;
  background: rgba(0, 0, 0, 0.6);
  z-index: 1100;
  //   &.active {
  //     display: block;
  //   }
  .entity-card {
    position: relative;
    // background-color: #234463;
    // background-color: #e5eef3;
    background-color: white;
    top: 4%;
    // left: 6%;
    left: 6vw;
    // height: 580px;
    // width: 512px;
    // width: 1156px;
    width: 85vw;
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
    .entity-content {
      // display: flex;
      padding: 32px 16px;
      .title {
        width: min-content;
        // width: 120px;
        font-size: 12px;
        // color: #457bbf;
        color: black;
        // color: #999999;
        margin-right: 8px;
      }
      .content {
        font-size: 16px;
        // color: white;
        color: black;
        // color: #999999;
      }
      .data-fields {
        // margin-right: 16px;
        margin-bottom: 16px;
        // padding: 32px 16px;
        .field {
          display: flex;
          flex-direction: row;
          align-items: baseline;
          margin: 0 0 8px 0;

          &.short-name,
          &.edrpou,
          &.reg-date {
            .title {
              width: fit-content;
            }
          }

          // .title {
          //   width: 100px;
          //   font-size: 12px;
          //   color: #457bbf;
          //   margin-right: 8px;
          // }
          // .content {
          //   font-size: 14px;
          //   color: white;
          // }
        }
        a {
          font-size: 12px;
          // color: #ffda3c;
          color: black;
        }
        hr {
          margin: 16px 0;
        }
      }
      .views-headers {
        display: flex;
        // margin-bottom: 32px;
        margin: 32px 0 32px 0;
        .view-name {
          margin-right: 24px;
          cursor: pointer;
          .view-title {
            padding: 0px 0 6px 0;
            &.is-selected {
              border-bottom: 2px solid black;
            }
          }
        }
      }
      .table-wrapper {
        .external-link-info {
          width: 60%;
          font-size: 12px;
          margin-bottom: 8px;
        }
      }
    }
  }
}
@media (max-width: 480px) {
  .entity-panel {
    position: fixed;
    overflow: scroll;
    box-sizing: border-box;
    padding: 4vw;
    .entity-card {
      left: 0;
      width: 100%;
      .entity-content {
        .data-fields {
          margin-bottom: 48px;
        }
        .views-headers {
          font-size: 14px;
        }
        @media (max-width: 320px) {
          .views-headers {
            .view-name {
              margin-right: 16px;
            }
          }
        }
        .table-wrapper {
          .external-link-info {
            width: 90%;
          }
        }
      }
    }
  }
}
</style>