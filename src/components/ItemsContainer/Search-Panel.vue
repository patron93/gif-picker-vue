<template>
  <div class="container-fluid">

    <transition
      name="custom-transition"
      enter-active-class="animated fadeIn">

      <div class="search-panel" v-show="state">
        <div class="row">

          <div class="col-6">
            <p class="search-text">showing by:</p>
            <p class="search-tag">#{{searchingTag}}</p>
          </div>

          <div class="col-6">
            <div class="close" @click="close">
              <i class="fa fa-times" aria-hidden="true"></i>
            </div>
          </div>

        </div>
      </div>
    </transition>
  </div>

</template>


<script>
  export default {
    name: 'SearchPanel',
    props: ['searchingTag', 'state'],
    methods: {
      close() {
        this.$emit('close');
      },
    },
  };

</script>


<style lang="scss">

  @import '../../assets/scss/vars';

  .search-panel {
    width: 100%;
    background-color: $headerColor;
    margin: 20px auto;
    box-shadow: 2px 2px 2px 0 rgba(0, 0, 0, 0.3);
    line-height: 1.2em;
    cursor: default;
    text-align: left;
    &.animated {
      animation-duration: .3s;
      animation-fill-mode: both;
    }

    .search-text {
      position: relative;
      left: 20px;
      display: inline-block;
      text-transform: uppercase;
      font-weight: 500;
      font-size: .8em;
      color: $textColor;

    }
    .search-tag {
      position: relative;
      left: 20px;
      display: inline-block;
      color: $accent;
      font-weight: 500;
      text-transform: uppercase;

    }

    .close {

      line-height: 51px;
      display: block;
      position: relative;
      right: 20px;
      text-align: right;
      font-size: 1.8em;
      color: $accent;
      transition: all ease .3s;
      cursor: pointer;
      &:hover {
        color: #22bbff;
      }
    }
  }

</style>
