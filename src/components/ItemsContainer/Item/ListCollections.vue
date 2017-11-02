<template>

  <transition name="custom"
              enter-active-class="animated fadeIn"
              leave-active-class="animated fadeOut">

  <ul class="item-collections-ul" v-show="ListCollectionsShowing">
    <li class="item-collections-li"
        v-for="collection in collections()"
        :class="[stateList[collection.name] ? 'green' : '']"
        @click="addInCollection">{{collection.name}}
    </li>

  </ul>

  </transition>
</template>


<script>
  export default {
    name: 'ListCollections',
    props: ['ListCollectionsShowing'],
    data() {
      return {
        stateList: [],
      };
    },
    methods: {
      collections() {
        return JSON.parse(localStorage.getItem('collections'));
      },
      addInCollection(event) {
        const name = event.target.textContent.trim();

        if (!this.stateList[name]) {
          const collections = this.collections();
          const item = event.target.closest('.thumb');
          const itemObj = {
            src: '',
            keys: [],
          };
          const src = item.getElementsByTagName('img')[0].getAttribute('src');
          const tags = item.getElementsByClassName('item-tag');

          for (let i = 0; i < tags.length; i += 1) {
            itemObj.keys.push(tags[i].textContent.slice(1));
          }
          itemObj.src = src;

          for (let i = 0; i < collections.length; i += 1) {
            if (name === collections[i].name) {
              collections[i].items.push(itemObj);
            }
          }
          this.stateList[name] = true;
          event.target.classList.add('green');
          localStorage.setItem('collections', JSON.stringify(collections));
        }
      },
    },
    mounted() {
      const collections = this.collections() || [];
      const srcImage = this.$parent.$el.getElementsByTagName('img')[0].getAttribute('src');

      for (let i = 0; i < collections.length; i += 1) {
        this.stateList[collections[i].name] = false;
        for (let j = 0; j < collections[i].items.length; j += 1) {
          if (srcImage === collections[i].items[j].src) {
            this.stateList[collections[i].name] = true;
          }
        }
      }
    },
  };

</script>


<style lang="scss">

  @import "../../../assets/scss/vars.scss";

  .item-collections-ul {
    min-width: 80px;
    max-width: 150px;
    display: block;
    position: absolute;
    list-style-type: none;
    color: $textColor;
    padding: 0;
    margin: 0 3px;
    top: 45px;
    line-height: 1.5em;
    z-index: 50;
    font-size: .9em;
    background-color: rgb(239, 239, 239);
    border-radius: 5px;
    text-align: center;
    transition: all ease .3s;
    border: solid 1px rgba(0, 0, 0, 0.43);
    box-shadow: 0 0 2px 1px rgba(0, 0, 0, 0.3);
    &.animated {
      animation-duration: .1s;
      animation-fill-mode: both;
    }
    .green {
      background-color: #00d600 !important;
      color: $headerColor;
      &:hover {
        color: #fff;
        background-color: #00c600;
      }
      &:first-child {
        border-top-left-radius: 5px;
        border-top-right-radius: 5px;
      }
      &:last-child {
        border-bottom-left-radius: 5px;
        border-bottom-right-radius: 5px;
      }
    }
    .item-collections-li {
      display: block;
      padding: 3px 5px;
      transition: all ease .3s;
      border-bottom: 1px solid rgba(103, 103, 103, 0.35);
      &:hover {
        &:first-child {
          border-top-left-radius: 5px;
          border-top-right-radius: 5px;
        }
        &:last-child {
          border-bottom-left-radius: 5px;
          border-bottom-right-radius: 5px;
        }
        background-color: rgb(196, 196, 196);
      }
    }
  }
  @media only screen and (max-width : 480px){
    .item-collections-ul {
      font-size: .7em;
    }
  }
</style>
