<template>
  <header class="header">
    <nav class="header__nav">
      <transition name="fade" appear>
        <div v-if="showBackButton" class="header__nav__back-button">
          <m-icon-button @click="cancelSwap">
            <svg class="header__nav__back-button__icon" viewBox="0 0 24 24">
              <path d="M19 12H6M12 5l-7 7 7 7"></path>
              >
            </svg>
          </m-icon-button>
        </div>
      </transition>
      <img class="header__nav__logo" src="@/assets/switch-logo.svg" />
      <div class="header__nav__testnet-badge">Testnet</div>
    </nav>
    <transition name="fly-in" mode="out-in">
      <nav v-if="showSelectDestBar" class="header__select-bar">
        <span class="header__select-bar__title"
          >Select an asset to receive</span
        >
        <m-button
          class="header__select-bar__cancel-button"
          outlined
          dense
          @click="cancelSwap"
          >Cancel</m-button
        >
      </nav>
    </transition>
  </header>
</template>

<script>
export default {
  computed: {
    showSelectDestBar() {
      return (
        this.$store.state.route.name === 'home' &&
        this.$store.state.route.selectedSourceUplink
      )
    },
    showBackButton() {
      return (
        this.$store.state.route.name === 'swap' &&
        !this.$store.state.route.isStreaming
      )
    }
  },
  methods: {
    cancelSwap() {
      const route = this.$store.state.route
      if (route.name !== 'swap' || !route.isStreaming) {
        this.$store.commit('NAVIGATE_TO', {
          name: 'home',
          selectedSourceUplink: null
        })
      }
    }
  }
}
</script>

<style lang="scss" scoped>
// TODO Find a better solution
$mdc-typography-font-family: 'Rubik';
$mdc-theme-primary: white;
@import 'material-components-vue/dist/button/styles';

.header {
  display: flex;
  justify-content: center;

  &__nav {
    width: $content-max-width;
    height: 72px;
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;

    // The Material icon button requires a wrapper to get the Vue transition to function properly
    &__back-button {
      position: absolute;
      left: 0;

      &.fade-enter-active {
        // The old route takes 300ms to fade out, so don't start fading in until the new route begins fading in
        transition-delay: 300ms;
      }

      &__icon {
        fill: none;
        stroke: $primary;
        stroke-width: 2;
        stroke-linecap: round;
        stroke-linejoin: round;
      }
    }

    &__logo {
      height: 30px;
      position: absolute;
      user-select: none;
      -webkit-user-drag: none;
    }

    &__testnet-badge {
      position: relative;
      padding: 4px 7px;
      left: 130px;
      border: 2px solid $secondary;
      border-radius: 6px;
      text-transform: uppercase;
      letter-spacing: 1.4px;
      font-size: 10pt;
      font-weight: 700;
      color: $secondary;
    }
  }

  &__select-bar {
    width: 100%;
    height: 60px;
    display: flex;
    align-items: center;
    justify-content: center;
    background: $primary;
    color: white;
    position: fixed;
    top: 0;
    transform: translateZ(0);

    &.fly-in-enter-active {
      transition: transform 150ms $easing-decelerate;
    }

    &.fly-in-leave-active {
      transition: transform 150ms $easing-accelerate;
    }

    &.fly-in-enter,
    &.fly-in-leave-to {
      transition: transform 150ms $easing-accelerate;
      transform: translateY(-72px);
    }

    &__title {
      @extend %h6;
      margin: 0;
      color: $text-white-high-emphasis;
      font-weight: 400;
    }

    &__cancel-button {
      margin-left: 20px;
    }
  }
}
</style>
