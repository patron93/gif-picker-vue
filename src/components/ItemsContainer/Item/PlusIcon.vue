<template>
  <div>
    <i class="fa fa-plus-square" aria-hidden="true" @click="openList" v-if="showingPlus"></i>
    <i class="fa fa-minus-square" aria-hidden="true" @click="removeInCollection" v-else="!showingPlus"></i>
  </div>
</template>

<script>
  export default {
    name: 'Plus',
    props: ['showingPlus'],
    methods: {
      openList() {
        const collections = JSON.parse(localStorage.getItem('collections')) || [];

        if (collections.length === 0) {
          this.$root.$emit('showPopup');
        } else {
          this.$emit('open-list');
        }
      },
      removeInCollection() {
        this.$emit('remove-in-collection');
      },
    },
  };
</script>

<style lang="scss">

  @import "../../../assets/scss/vars.scss";

  .fa-plus-square, .fa-minus-square {
    opacity: 0;
    position: absolute;
    color: rgba(255, 255, 255, 0.75);
    font-size: 1.6em;
    top: 8px;
    left: 8px;
    z-index: 150;
    transition: all .4s ease;
    &:hover {
      color: $accent;
    }
  }
  .fa-minus-square:hover {
    color: red;
  }
  @media only screen and (max-width : 992px){
    .fa-plus-square, .fa-minus-square {
      opacity: .5;
    }
  }
  @media only screen and (max-width : 320px){
    .fa-plus-square, .fa-minus-square {
      font-size: 1.4em;
    }
  }

</style>
