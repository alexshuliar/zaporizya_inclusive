<template>
  <div id="app">
    <div class="container">
      <top-footer></top-footer>
      <div class="about-project-wrapper">
        <div class="about-project">
          <div class="header-container">
            <p class="header-text">Про проєкт</p>
          </div>
          <div class="content-container">
            <p class="normal-text">
              Місто для всіх - це веб-сервіс, який покликаний зробити Запоріжжя
              зручнішим для маломобільних верств населення. Проєкт має на меті
              забезпечити прозорість інформації щодо наявних у місті засобів
              доступності та робіт, що проводились для їх облаштування.
            </p>
            <!-- <p class="normal-text">
              Проект буде корисний жителям міста, що входять до маломобільних груп населення та особам, що їх супроводжують, адже дозволить краще планувати своє пересування містом за допомогою мапи сервісу, на якій позначено, якими об’єктами доступності обладнана конкретна будівля. За допомогою об’єднаних даних щодо дозволів, тендерів та позатендерних закупівель, які можна знайти на сторінці власника кожного об’єкта, громадськість матиме додатковий інструмент контролю за тим, наскільки ефективно витрачаються кошти щодо облаштування засобів доступності в місті. Щодо об’єктів міської інфраструктури сервіс надасть можливість ознайомитись з інформацією щодо стану засобів доступності, що була встановлена в ході перевірок комунальних об’єктів. Це дозволить жителям міста оцінити відповідність заявленого стану будівель реальній ситуації в місті та краще комунікувати із місцевою владою щодо необхідності проведення додаткових перевірок та нових ремонтних робіт на об’єктах.
            </p> -->
            <p class="normal-text">
              Проєкт використовує <a
                href="https://data.gov.ua/dataset/bd9a85a7-610d-40ed-962b-bb1cf509d796"
                class="h-link"
              >дані Запорізької міської ради про доступність будівель для осіб з інвалідністю та інших маломобільних груп населення</a>, <a
                href="https://prozorro.gov.ua/openprocurement"
                class="h-link"
              >інформацію щодо закупівель системи Prozorro</a>, <a
                href="https://spending.gov.ua/new/"
                class="h-link"
              >дані Єдиного веб-порталу використання публічних коштів</a> та <a
                href="https://data.gov.ua/dataset/0c352c5f-786b-4407-9c3d-49cddd63452e"
                class="h-link"
              >дані про декларативні документи з Реєстру будівельної діяльності</a>. Дані сервісу оновлюються щоквартально.
            </p>
          </div>

        </div>
      </div>
      <div class="section-block">
        <p class="block-header">Для кого цей проєкт?</p>
        <p class="block-text">
          Проект буде корисний жителям міста, що входять до маломобільних груп населення та особам, що їх супроводжують, адже дозволить краще планувати своє пересування містом за допомогою мапи сервісу, на якій позначено, якими об’єктами доступності обладнана конкретна будівля. За допомогою об’єднаних даних щодо дозволів, тендерів та позатендерних закупівель, які можна знайти на сторінці власника кожного об’єкта, громадськість матиме додатковий інструмент контролю за тим, наскільки ефективно витрачаються кошти щодо облаштування засобів доступності в місті. Щодо об’єктів міської інфраструктури сервіс надасть можливість ознайомитись з інформацією щодо стану засобів доступності, що була встановлена в ході перевірок комунальних об’єктів. Це дозволить жителям міста оцінити відповідність заявленого стану будівель реальній ситуації в місті та краще комунікувати із місцевою владою щодо необхідності проведення додаткових перевірок та нових ремонтних робіт на об’єктах.
        </p>
      </div>
      <div class="section-wrapper">
        <div class="section-block">
          <p class="block-header">Допоможи зробити місто доступнішим</p>
          <p class="block-text">
            Що можете зробити ви для того, щоб зробити сервіс кращим, а місто зручнішим для пересування? Сервіс дозволяє користувачам додавати власні об’єкти на мапі. Таким чином сервіс буде містити найбільш повну і актуально інформацію щодо доступності в місті, що дозволить маломобільним групам населення краще планувати своє пересування містом. Інформацію про те, як додати об’єкт на мапі можна знайти, клікнувши <a
              @click="scrollToHow"
              class="h-link"
            >тут</a>.
          </p>
        </div>
      </div>
      <div class="section-block">
        <p class="block-header">Доступна навігація</p>
        <p class="block-text">
          На мапі нижче відображені комунальні об’єкти міста, що містяться в
          наборі даних з доступності Запорізької міської ради. Різними кольорами
          відмічені типи організацій, що знаходяться на об’єкті. За допомогою
          галочки ви можете фільтрувати об’єкти на мапі, залишивши галочку
          лише для того засобу доступності, який хочете побачити на карті. При
          наведенні на маркер з’явиться спливаюче віконце із назвою об’єкту.
          Ознайомитись з більш детальною інформацією про об’єкт та засоби
          доступності на ньому можна, клікнувши на це віконце.
        </p>
      </div>
      <div
        id="how-to-area"
        class="how-to-section is-closed"
        v-on:click="switchHowTo"
      >
        <div class="how-to-border">
          <div class="how-to-bar">
            <div class="pointer-icon">
              <!-- &#9757; -->
              <img src="./assets/images/pointer-icon.png" />
            </div>
            <p class="how-to-title">
              Не знайшли об'єкт на карті? Додайте його власноруч!
            </p>
            <!-- <span></span> -->
          </div>
          <div class="how-to-description">
            <p>
              Сервіс також дозволяє додавати власні об’єкти на карту. Наприклад,
              ви хотіли б додати інформацію про магазин в місті, в якому
              обладнаний пандус, щоб інші жителі також могли це побачити. Для
              цього вам необхідно зайти на цю сторінку зі свого персонального
              комп'ютера або ноутбука, затиснути клавішу Ctrl (для Windows) або
              &#8984; (для Mac) та, не відпускаючи її, клікнути на карті, щоб
              створити точку. Після кліку з’явиться форма, в якій необхідно
              заповнити дані щодо цього об’єкту. Після цього дані відправляться
              на модерацію, і об’єкт буде доданий після його перевірки.
            </p>
          </div>
        </div>
      </div>
      <!-- </div> -->
      <map-view
        :jsonFile="jsonFile"
        :sectionNames="sectionNames"
        v-on:sendScrollObject="getScrollObject"
      ></map-view>
      <!-- <div class="section-two"> -->
      <div class="section-block">
        <p class="block-header">Якими засобами обладнані місцеві будівлі?</p>
        <p class="block-text">
          Нижче доступна розгорнута інформація по кожному з об’єктів міста.
          Виберіть тип організації та клікніть по назві об’єкту або на знак + у
          правій частині екрану. Після цього відкриється картка об’єкту, в якій
          можна побачити деталі щодо засобів доступності, які були встановлені в
          ході перевірки об’єкту, та обсяг фінансування, виділений на відповідні
          роботи. Клікніть по назві організації, яка розміщена внизу кожної
          картки, щоб перейти до інформації про дозволи та тендери на
          будівельні/ремонтні роботи.
        </p>
      </div>
      <!-- </div> -->
      <buildings-list
        :jsonFile="jsonFile"
        :sectionNames="sectionNames"
        :scrollTo="scrollTo"
        v-on:sendEntityCode="getEntityCode"
      ></buildings-list>
      <entity-panel
        :currentEntity="currentEntity"
        :currentEntityName="currentEntityName"
        v-on:changeEntityStatus="currentEntity = undefined"
      ></entity-panel>
      <div class="call-to-action-container">
        <div class="call-to-action">
          <div class="block-background">
            <p>
              Дані нашого проєкту можуть допомогти вам оцінити стан доступності конкретної будівлі. Щодо об’єктів, які відносяться до інфраструктури міста, ви можете побачити, які роботи проводились на об’єкті і коли востаннє проводилась перевірка.
            </p>
            <div class="title-container">
              <div class="pointer-icon">
                <img src="./assets/images/pointer-icon.png" />
              </div>
              <p class="title-small">
                Що робити, якщо ви помітили невідповідність заявлених умов реальному стану засобів доступності на цих об’єктах?
              </p>
            </div>
            <p>
              Наприклад, ви помітили пандус, стан якого не дозволяє його використання за призначенням і бачите, що остання перевірка проводилась декілька років назад. В такому разі, якщо мова йде про будівлю, що знаходиться в комунальній власності, ви можете звернутися до <a
                href="https://zp.gov.ua/uk/page/council_email"
                class="h-link"
              >Запорізької міської ради</a>. Щодо будівель, які перебувають у приватній власності, ви можете звернутися до:
            <ul>
              <li><a
                  href="https://www.kmu.gov.ua/profile/tetyana-barantsova"
                  class="h-link"
                >Урядового уповноваженого з прав осіб з інвалідністю</a></li>
              <li><a
                  href="https://www.president.gov.ua/appeals/appeals-info"
                  class="h-link"
                >Уповноваженого Президента України з прав осіб з інвалідністю</a></li>
              <li><a
                  href="https://dabi.gov.ua/contacts/"
                  class="h-link"
                >Державної архітектурно-будівельної інспекції</a>, якщо мова йде про новобудову</li>
            </ul>
            </p>
          </div>
        </div>
      </div>
      <footer class="grid">
        <div class="footer-content content">
          <div class="created-by-texty">
            <p>Створено Текстами</p>
            <img src="./assets/images/white_logo.texty.svg" />
          </div>
          <div class="socio-wrapper">
            <p>Поширити:</p>
            <div class="socio-icons-box">
              <a
                href=""
                class="icon-wrapper"
              >
                <img
                  src="./assets/images/share-icon-border.png"
                  class="icon-box"
                />
                <img
                  src="./assets/images/fb.png"
                  class="icon-box"
                  id="fb"
                /></a><a
                href=""
                class="icon-wrapper"
              >
                <img
                  src="./assets/images/share-icon-border.png"
                  class="icon-box"
                />
                <img
                  src="./assets/images/twitter.png"
                  class="icon-box"
                  id="tw"
                /></a>
            </div>
          </div>
        </div>
      </footer>
    </div>
  </div>
</template>

<script>
import TopFooter from "@/components/TopFooter";
import BuildingsList from "@/components/BuildingsList";
import MapView from "@/components/Map";
import EntityPanel from "@/components/EntityPanel";

export default {
  name: "App",
  components: {
    TopFooter,
    BuildingsList,
    MapView,
    EntityPanel,
  },
  data() {
    return {
      jsonFile: [],
      sectionNames: [
        "Адмінбудівлі",
        "Соціальна сфера",
        "Житлові будинки",
        "Школи",
        "Клінічні заклади",
        "Культурні заклади",
        "Інфраструктура",
      ],
      scrollTo: undefined,
      currentEntity: undefined,
      currentEntityName: undefined,
    };
  },
  methods: {
    getScrollObject(idx) {
      this.scrollTo = idx;
    },
    getEntityCode(entity) {
      this.currentEntity = entity.edrpou;
      this.currentEntityName = entity.name;
    },
    switchHowTo(event) {
      event.currentTarget.classList.toggle("is-closed");
      event.currentTarget.classList.toggle("is-open");
    },
    scrollToHow() {
      document.documentElement.scrollTop =
        document.querySelector("#how-to-area").offsetTop - 20;

      setTimeout(() => {
        document.querySelector("#how-to-area").click(), 3000;
      }, 1000);
    },
  },
  mounted() {
    this.jsonFile = require("@/assets/data/zp_objects_final_v3.json");
    // this.jsonFile.forEach((record) => {
    //   this.columnsUpperCase.forEach((colName) => {
    //     if (record[colName]) {
    //       record[colName] = record[colName].toUpperCase();
    //     }
    //   });
    // });
  },
};
</script>

<style lang="scss">
@font-face {
  font-family: e-UkraineLight;
  src: url(./assets/fonts/e-Ukraine-Light.woff);
}

@font-face {
  font-family: e-UkraineRegular;
  src: url(./assets/fonts/e-Ukraine-Regular.woff);
}

@font-face {
  font-family: e-UkraineHead;
  src: url(./assets/fonts/e-UkraineHead-NAME.otf);
}
* {
  margin: 0;
  padding: 0;
}

html {
  scroll-behavior: smooth;
}

.container {
  font-family: e-UkraineLight;
  .header-wrapper,
  .logo-footer,
  .about-project,
  .big-stats,
  .how-to-section,
  .section-block,
  // .section-one,
  // .section-two,
  .buildings-list,
  .map-container, 
  .call-to-action-container {
    max-width: 1680px;
    @media (min-width: 1680px) {
      width: 1408px;
    }
    @media (min-width: 1280px) and (max-width: 1679px) {
      width: 1120px;
    }
    @media (min-width: 992px) and (max-width: 1279px) {
      width: 864px;
    }
    @media (max-width: 991px) and (min-width: 480px) {
      width: 95%;
    }
    @media (max-width: 479px) {
      // width: 100%;
      width: 90%;
    }
  }
  .section-wrapper {
    background-color: #e5eef2;
    padding: 2.5em 0;
    margin-bottom: 2.5em;
    .section-block {
      .block-text {
        margin-bottom: 0;
      }
    }
  }
  .section-block {
    margin: 0px auto 0px auto;
  }
  @media (max-width: 768px) {
    .section-block {
      br {
        display: none;
      }
      .block-text {
        max-width: 100%;
      }
    }
  }
  p.normal-text {
    line-height: 1.5em;
    margin-bottom: 1em;
  }
  p.block-header {
    font-size: 1.7em;
    margin-bottom: 1em;
    display: block;
  }
  p.block-text {
    line-height: 1.5em;
    margin-bottom: 2.5em;
    max-width: 70%;
  }
  a.h-link {
    text-decoration: none;
    border-bottom: 1px solid black;
    color: black;
    cursor: pointer;
    &:hover {
      color: #616365;
      border-color: #616365;
    }
  }
  .pointer-icon {
    padding-left: 16px;
    // display: inline-block;
    margin-right: 8px;
    img {
      height: 28px;
      width: 28px;
      // margin-bottom: -2px;
    }
  }
  .about-project {
    margin: 120px auto 2.5em;
    padding-bottom: 2.5em;
    display: grid;
    grid-template-areas: "project-header project-text" ". next-button";
    grid-template-columns: 0.4fr 0.6fr;
    column-gap: 30px;
    .header-container {
      display: flex;
      justify-content: center;
      grid-area: project-header;
      .header-text {
        font-family: e-UkraineRegular;
        font-size: 2.9em;
        height: fit-content;
      }
    }
    .header-text-small {
      font-family: e-UkraineRegular;
      font-size: 2em;
      margin: 1em 0 0.7em 0;
    }
    .normal-text,
    li {
      line-height: 1.5em;
      margin-bottom: 1.5em;
    }
    .content-container {
      grid-area: project-text;
    }
    .read-next-button {
      grid-area: next-button;
      display: inline-block;
      width: fit-content;
      align-items: center;
      color: white;
      background-color: black;
      padding: 16px 32px;
      border-radius: 40px;
      height: 30px;
      border: 2px solid black;
      cursor: pointer;
      text-decoration: none;
      height: fit-content;
      // @include button-on-hover;
    }
  }
  @media (max-width: 768px) {
    .about-project {
      grid-template-columns: 0.5fr 0.5fr;
    }
  }
  @media (max-width: 480px) {
    .about-project {
      display: block;
      max-width: none;
      width: 90%;
      .header-container {
        display: block;
        .header-text {
          margin-bottom: 1em;
        }
      }
    }
  }
  .big-stats {
    display: flex;
    flex-direction: row;
    margin: 0px auto 120px auto;
    .stats-cell {
      // background-color: white;
      background-color: #f1f1f1;
      padding: 1em 2% 1em 2%;
      margin-right: 2%;
      .stats-number {
        font-size: 6em;
      }
      .stats-label {
        font-size: 1em;
      }
      &:nth-child(1) {
        flex-basis: 20%;
      }
      &:nth-child(2) {
        flex-basis: 25%;
      }
      &:last-child {
        margin-right: 0;
        span {
          margin-left: 2%;
          font-size: 6vw;
          font-size: 0.76em;
        }
      }
    }
  }
  @media (max-width: 768px) {
    .big-stats {
      .stats-cell {
        margin-bottom: 0.7em;
        &:last-child {
          span {
            margin-left: 3%;
          }
        }
        .stats-number {
          font-size: 9vmin;
        }
        .stats-label {
          font-size: 1em;
        }
      }
    }
  }
  @media (max-width: 480px) {
    .big-stats {
      flex-direction: column;
      width: 90%;
      text-align: center;
      .stats-cell {
        margin-bottom: 0.7em;
        margin-right: 0;
        &:last-child {
          span {
            font-size: 12vmin;
            margin-left: 3%;
          }
        }
        .stats-number {
          font-size: 12vmin;
        }
        .stats-label {
          font-size: 1em;
        }
      }
    }
  }
  .how-to-section {
    margin: 0px auto 36px auto;
    display: flex;
    flex-direction: column;
    position: relative;
    // width: 67%;
    transition: background-color 0.5s ease-in-out;
    .how-to-border {
      width: 67%;
      border: double 2px transparent;
      background-image: linear-gradient(white, white),
        radial-gradient(
          circle at top left,
          #c3aab2 0.66%,
          #99eecc 57.2%,
          #80c0c8 75.87%,
          #4b8bfa 98.72%
        );
      background-origin: border-box;
      background-clip: padding-box, border-box;
      .pointer-icon {
        font-size: 28px;
      }
    }
    .how-to-bar {
      display: flex;
      flex-direction: row;
      align-items: center;
      position: relative;
      // .pointer-icon {
      //   padding-left: 16px;
      //   display: inline-block;
      //   margin-right: 8px;
      //   img {
      //     height: 28px;
      //     width: 28px;
      //     // margin-bottom: -2px;
      //   }
      // }
      &:before,
      &:after {
        position: absolute;
        top: 22px;
        right: 16px;
        width: 2px;
        height: 20px;
        content: "";
        background-color: #000;
        transition: 0.3s ease-in-out;
      }
      &:after {
        transform: rotate(90deg);
      }
      p.how-to-title {
        font-size: 1.2em;
        padding: 20px 0;
        transition: 0.5s;
      }
    }
    cursor: pointer;
    .how-to-description {
      max-height: 0;
      overflow: hidden;
      transition: max-height 0.2s ease-out;
      padding: 0 16px 0px 16px;
      p {
        line-height: 1.5em;
        font-size: 13px;
        width: 95%;
      }
    }
    &.is-open {
      span {
        background: transparent;
      }
      .how-to-description {
        max-height: 1000px;
        padding: 0 16px 16px 16px;
      }
      .how-to-bar {
        &:before {
          transform: rotate(90deg);
        }
      }
    }
    &.is-closed {
      &:hover {
        background-color: inherit;
        color: black;
        p.how-to-title {
          transform: none;
        }
        span {
          background: transparent;
        }
        &:before {
          background-color: black;
          transform: none;
        }
        &:after {
          background-color: black;
          transform: rotate(90deg);
        }
      }
    }
  }
  @media (max-width: 480px) {
    .how-to-section {
      .how-to-border {
        width: 100%;
        .how-to-title {
          width: 75%;
        }
        .how-to-description {
          // width: 95%;
          padding: 0 8px 0px 8px;
        }
      }
      &.is-open {
        .how-to-description {
          padding: 0 8px 16px 8px;
        }
      }
    }
  }
  .call-to-action-container {
    margin: 64px auto 64px auto;
    .call-to-action {
      width: 67%;
      border: double 2px transparent;
      background-image: linear-gradient(white, white),
        radial-gradient(
          circle at top left,
          #c3aab2 0.66%,
          #99eecc 57.2%,
          #80c0c8 75.87%,
          #4b8bfa 98.72%
        );
      background-origin: border-box;
      background-clip: padding-box, border-box;
      .block-background {
        padding: 16px 32px;
        background-color: #fff6e3;
      }
      p {
        line-height: 1.5em;
        // font-size: 13px;
        margin-bottom: 16px;
        &:last-child {
          margin-bottom: 0;
        }
        &.title-small {
          // font-size: 16px;
        }
      }
      .title-container {
        display: flex;
        margin-bottom: 16px;
        .pointer-icon {
          padding-left: 0;
          // display: inline-block;
          // margin-right: 8px;
          // img {
          //   height: 28px;
          //   width: 28px;
          //   // margin-bottom: -2px;
          // }
        }
      }
      ul {
        margin-top: 8px;
        padding-left: 16px;
        li {
          margin-bottom: 4px;
        }
      }
    }
  }
  @media (max-width: 480px) {
    .call-to-action-container {
      .call-to-action {
        width: 100%;
      }
    }
  }
  footer.grid {
    margin-top: 64px;
    display: grid;
    grid-template-columns: [start] 1fr [content-start text-start] 5fr [middle] 5fr [content-end] 1fr [end];
    font-style: normal;
    font-size: 16px;
    line-height: 24px;
    letter-spacing: -0.02em;
    background-color: black;
    .footer-content {
      padding: 3em 0;
      display: flex;
      justify-content: space-between;
      grid-column: content-start/content-end;
      .created-by-texty,
      .socio-wrapper {
        p {
          margin: 10px 0;
          color: #ece9e5;
        }
        img {
          width: 180px;
        }
      }
      .socio-icons-box {
        display: flex;
        .icon-wrapper {
          box-sizing: border-box;
          margin-right: 12px;
          position: relative;
          width: 36px;
          height: 36px;
          text-decoration: none;
          .icon-box {
            position: absolute;
            left: 0;
            right: 0;
            top: 0;
            bottom: 0;
            width: 36px;
            height: 36px;
            box-sizing: border-box;
          }
          #fb {
            position: absolute;
            left: 36.11%;
            right: 36.11%;
            top: 25%;
            bottom: 22.22%;
            width: 10px;
            height: 19px;
            box-sizing: border-box;
          }
          #tw {
            position: absolute;
            left: 25%;
            right: 25%;
            top: 27.78%;
            bottom: 30.56%;
            width: 18px;
            height: 15px;
            box-sizing: border-box;
          }
        }
      }
    }
  }
  @media (max-width: 320px) {
    footer.grid {
      font-size: 14px;
      .footer-content {
        .created-by-texty {
          img {
            width: 148px;
          }
        }
        .socio-icons-box {
          .icon-wrapper {
            // width: 24px;
            // height: 24px;
          }
        }
      }
    }
  }
}
</style>
