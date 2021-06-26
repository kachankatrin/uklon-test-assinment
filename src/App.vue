<template>
  <v-app id="app">
    <double-comparison />
  </v-app>
</template>

<script>
import DoubleComparison from "./components/DoubleComparison.vue";

export default {
  name: 'App',

  components: {
    DoubleComparison
  },

  data: () => ({
  }),
};
</script>

<style lang="scss">
  #app {
    font-family: "Poppins, sans-serif";
    font-weight: 500;
    font-size: 14px;
    ul {
      li {
        list-style: none;
      }
    }
    .v-input--selection-controls {
      &__ripple {
        border-radius: 3px;
      }
      &__input {
        &:hover {
          .v-input--selection-controls__ripple {
            &:before {
              background: #80e8d9;
              transform: scale(1.2) translateX(8%);
            }
          }
        }
      }
    }
    .v-icon {
      &.v-icon {
        opacity: 0;
      }
    }
    .v-text-field--outlined fieldset {
      border-bottom-right-radius: 0;
      border-top-right-radius: 0;
      border-right: 0
    }
    .item-container {
      .v-input--selection-controls__input, .v-label{
          position: absolute !important;
          top: 0;
          width: 100%;
          height: 100%;
          padding: 2px;
          color: inherit;
      }
      .v-label {
          margin: 0;
          left: 50% !important;
          transform: translateX(-25%);
      }
    }
    .v-item--active {
      background-color: #80e8d9;
      border: none;
      color: #FFFFFF;
    }
  }
</style>