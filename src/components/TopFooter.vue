<template>
  <div class="image-background">
    <div class="image-container">
      <img src="../assets/images/shutterstock_790582078.jpg" />
      <!-- <img src="anastasia-dulgier-g8AZXNVkbq0-unsplash.jpg" /> -->
    </div>
    <div class="back-right"></div>
    <div class="header-background">
      <div class="header-wrapper">
        <div class="header-block">
          <div class="text-bold">
            <p>Місто<br />для всіх</p>
          </div>
          <div class="text-under">
            <p>Зробимо Запоріжжя доступним!</p>
          </div>
          <!-- <a href="#nav-section" class="scroll-button">Переглянути проєкт</a> -->
        </div>
      </div>
    </div>
    <div
      class="logo-footer"
      id="main-page"
    >
      <div class="image-logo mincif">
        <img src="../assets/images/MIN.png" />
      </div>
      <div class="image-logo zap">
        <img src="../assets/images/zap_logo.svg" />
      </div>
      <div class="image-logo usaid">
        <img src="../assets/images/usaid-logo-transparent.png" />
      </div>
      <div class="image-logo ukaid">
        <img src="../assets/images/uk-aid-logo-transparent.png" />
      </div>
      <div class="image-logo eur">
        <img src="../assets/images/eurasia-foundation-logo-transparent.png" />
      </div>
      <div class="image-logo texty">
        <img src="../assets/images/texty.png" />
      </div>
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style lang="scss">
.image-background {
  display: grid;
  // grid-template-rows: auto;
  grid-template-rows: auto 87px;
  // grid-template-columns: 1fr 1fr;
  grid-template-columns: 50% 50%;
  position: relative;
  max-width: 100vw;
  height: 100vh;
  // height: 669px;
  margin-bottom: 2em;
  .image-container {
    grid-column: 2;
    grid-row: 1 / span 2;
    height: 100%;
    width: 100%;
    position: absolute;
    z-index: 1;
    img {
      object-fit: cover;
      object-position: 0 0;
      // object-position: 80% 0;
      height: 100%;
      width: 100%;
    }
  }
  .back-right {
    grid-column: 2;
    grid-row: 2;
    // position: absolute;
    z-index: 1;
    height: 100%;
    width: 100%;
    background-color: rgb(231, 238, 243);
    opacity: 0.6;
  }
  .header-background {
    grid-row: 1;
    grid-column: 1 / span 2;
    // grid-column: 1;
    // background-color: #E7EEF3;
    background-color: #dbebeb;
    // background-color: #15CF74;
    // margin-bottom: 20px;
    position: relative;
    z-index: 0;
    .header-wrapper {
      //   max-width: 1680px;
      //   width: 864px;
      margin: 0px auto 0px auto;
      height: 100%;
      display: flex;
      align-items: center;
    }
    .header-block {
      display: block;
      // padding: 11% 0px 11% 0px;
      .text-bold {
        // margin-bottom: 1em;
        margin-bottom: 26px;
        p {
          font-family: e-UkraineHead;
          font-size: 3.2em;
          line-height: 1.2;
        }
      }
      .text-under p {
        line-height: 1.4;
        // margin-bottom: 2.5em;
        margin-bottom: 72px;
      }
      //   a.scroll-button {
      //     padding: 14px 44px;
      //     background: transparent;
      //     border-radius: 50px;
      //     font-size: 16px;
      //     line-height: 24px;
      //     text-align: center;
      //     border: 2px solid black;
      //     box-sizing: border-box;
      //     text-indent: 0px;
      //     width: fit-content;
      //     cursor: pointer;
      //     text-decoration: none;
      //     &:hover {
      //       color: white;
      //       background-color: black;
      //     }
      //   }
    }
  }

  .logo-footer {
    grid-row: 2;
    grid-column: 1 / span 2;
    display: flex;
    flex-direction: row;
    margin: 10px auto 10px auto;
    height: 67px;
    justify-content: space-between;
    align-items: center;
    $logo_baseline: 67px;
    .image-logo {
      position: relative;
      z-index: 2;
      height: calc(#{$logo_baseline} * 0.6);
      display: flex;
      justify-content: center;
      img {
        height: 100%;
        object-fit: contain;
      }
      &.mincif {
        height: calc(#{$logo_baseline} * 0.8);
      }
      &.zap {
        height: calc(#{$logo_baseline} * 0.65);
      }
      &.usaid {
        height: calc(#{$logo_baseline} * 0.5);
      }
      &.ukaid {
        height: calc(#{$logo_baseline} * 0.6);
      }
      &.eur {
        height: calc(#{$logo_baseline} * 0.5);
      }
      &.texty {
        height: calc(#{$logo_baseline} * 0.6);
      }
    }
  }
}
@media (max-width: 1280px) {
  .image-background {
    .image-container {
      img {
        object-fit: cover;
        object-position: 35% 0;
        height: 100%;
        width: 100%;
      }
    }
    .logo-footer {
      height: 6vw;
      $logo_baseline: 6vw;
      .image-logo {
        img {
          height: 100%;
          object-fit: contain;
        }
        &.mincif {
          height: calc(#{$logo_baseline} * 0.8);
        }
        &.ukaid,
        &.texty {
          height: calc(#{$logo_baseline} * 0.6);
        }
        &.eur,
        &.usaid {
          height: calc(#{$logo_baseline} * 0.5);
        }
      }
    }
  }
}
@media (max-width: 992px) {
  .image-background {
    grid-template-rows: auto 65px;
  }
}
@media (max-width: 767px) {
  .image-background {
    margin-bottom: 0.3em;
    .image-container {
      img {
        display: none;
      }
    }
    .back-right {
      display: none;
    }
    .logo-footer {
      width: 90%;
      height: 200px;
      flex-wrap: wrap;
      justify-content: space-around;
      // margin-bottom: 3em;
      $logo_baseline: 60px;
      .image-logo {
        img {
          height: 100%;
          object-fit: contain;
        }
        &.mincif {
          height: calc(#{$logo_baseline} * 0.8);
        }
        &.zap,
        &.ukaid,
        &.texty {
          height: calc(#{$logo_baseline} * 0.6);
        }
        &.eur,
        &.usaid {
          height: calc(#{$logo_baseline} * 0.5);
        }
        &.zap,
        &.mincif {
          flex-basis: 40%;
        }
        &.usaid {
          flex-basis: 33.3%;
        }
        &.eur {
          flex-basis: 33.3%;
        }
        &.ukaid {
          flex-basis: 33.3%;
        }
      }
    }
  }
}
@media (max-width: 767px) {
  .image-background {
    grid-template-rows: auto 220px;
  }
}
@media (max-width: 480px) {
  .image-background {
    .header-background {
      .header-wrapper {
        max-width: none;
        width: 90%;
        .header-block {
          padding: 15% 0px 15% 0px;
          width: fit-content;
          margin: 0 auto;
        }
      }
    }
  }
}
@media (max-width: 350px) {
  .image-background {
    .logo-footer {
      height: 260px;
    }
  }
}
</style>