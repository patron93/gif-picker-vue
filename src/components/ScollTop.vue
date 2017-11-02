<template>
  <div class="btn-scroll-top" v-show="showing" @click="scrollTop(400)">
    <i class="fa fa-chevron-up" aria-hidden="true"></i>
  </div>
</template>

<script>
  export default {
    name: 'ScrollTop',
    data() {
      return {
        showing: false,
      };
    },
    methods: {
      scrollTop(duration) {
        const STEP = 15;
        const scrollStep = -window.scrollY / (duration / STEP);
        const scrollInterval = setInterval(() => {
          if (window.scrollY !== 0) {
            window.scrollBy(0, scrollStep);
          } else clearInterval(scrollInterval);
        }, STEP);
      },
    },
    mounted() {
      window.addEventListener('scroll', () => {
        const DISTANCE_OF_SHOWING_BUTTONS = 200;
        if (window.scrollY >= DISTANCE_OF_SHOWING_BUTTONS) {
          this.showing = true;
        } else {
          this.showing = false;
        }
      });
    },
  };
</script>

<style lang="scss">

  @import "../assets/scss/vars.scss";

  .btn-scroll-top {
    position: fixed;
    bottom: 25px;
    right: 30px;
    height: 40px;
    width: 80px;
    font-size: 1.5em;
    border: 1px solid #fff;
    border-radius: 5px;
    background-color: $accent;
    color: #fff;
    line-height: 1.5em;
    transition: all ease .3s;
    box-shadow: 1px 1px 2px 1px rgba(0, 0, 0, 0.59);
    cursor: pointer;
    z-index: 200;
    -webkit-transform: translateZ(0);
    &:hover {
      background-color: $accentHover;
    }
  }
  @media only screen and (max-width : 480px){
    .btn-scroll-top {
      bottom: 0;
      right: 0;
      height: 40px;
      width: 100%;
      border-radius: 0;
    }
  }
</style>
