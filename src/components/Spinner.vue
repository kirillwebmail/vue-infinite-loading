<template>
  <lottie
    :width="36"
    :height="36"
    :options="lottieOptions"
    v-on:animCreated="handleAnimation"/>
</template>

<script>
import lottie from 'vue-lottie/src/lottie.vue';
import config from '../config';
import * as animationData from '../lottie/LoadingLoopL.json';

const SPINNERS = {
  LOTTIE: {
    render(createElement) {
      return createElement('span', {
        attrs: {
          class: 'loading-lottie',
        },
      }, Array.apply(Array, Array(8)).map(() => createElement('span', {
        attrs: {
          class: 'lottie-item',
        },
      })));
    },
  },
};

export default {
  name: 'Spinner',
  components: {
    lottie,
  },
  computed: {
    spinnerView() {
      return (
        SPINNERS[(this.$attrs.spinner || '').toUpperCase()]
        || this.spinnerInConfig // fallback to spinner of config
      );
    },
    spinnerInConfig() {
      let result;

      if (config.slots.spinner && typeof config.slots.spinner === 'string') {
        // as spinner slot config a pure text spinner
        result = {
          render() {
            return this._v(config.slots.spinner); // eslint-disable-line no-underscore-dangle
          },
        };
      } else if (typeof config.slots.spinner === 'object') {
        // as spinner slot config a Vue component
        result = config.slots.spinner;
      } else {
        // fallback to spinner property config
        /* istanbul ignore next */
        result = SPINNERS.LOTTIE;
      }

      return result;
    },
  },
  data() {
    return {
      anim: null, // for saving the reference to the animation
      lottieOptions: { animationData: animationData.default },
    };
  },
  methods: {
    handleAnimation(anim) {
      this.anim = anim;
    },
  },
};
</script>

<style lang="less" scoped>
@import '../styles/spinner';
</style>
