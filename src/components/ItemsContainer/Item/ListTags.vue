<template>
  <ul class="item-tags">
    <li class="item-tag" v-for="tag in tags" @click="search">#{{tag}}</li>
  </ul>

</template>

<script>
  export default {
    name: 'ListTags',
    props: ['tags'],
    methods: {
      search(event) {
        const str = event.target.textContent.slice(1);
        this.$emit('search-items', str);
      },
    },
  };

</script>


<style lang="scss">

  @import "../../../assets/scss/vars.scss";

  .item-tags {
    display: block;
    position: absolute;
    list-style-type: none;
    color: #fff;
    padding: 4px;
    margin: 0;
    bottom: 0;
    line-height: 1.1em;
    z-index: 10;
    font-size: .7em;
    background-color: rgba(0, 0, 0, 0.24);
    .item-tag {
      text-align: left;
      display: block;
      padding-left: 3px;
      padding-right: 8px;
      transition: all ease .3s;
      &:hover {
        color: $accent;
      }
    }
  }
  @media only screen and (max-width : 480px){
    .item-tags {
      font-size: .6em;
      .item-tag {
        opacity: .8;
      }
    }
  }

</style>
