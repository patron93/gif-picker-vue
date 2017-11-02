<template>

  <transition
    name="custom-transition"
    enter-active-class="animated zoomIn"
    leave-active-class="animated zoomOut">

    <div class="col-xl-2 col-lg-3 col-md-4 col-sm-4 col-6 item">
      <div class="thumb" @mouseleave="listCollectionsShowing = false">
        <img :src="src" alt="" @click="copy">
        <list-tags :tags="tags" @search-items="searchItems"
        />
        <star/>
        <plus :showing-plus="showingPlus" @open-list="openList" @remove-in-collection="removeInCollection"/>
        <list-collections :list-collections-showing="listCollectionsShowing"/>
      </div>
    </div>

  </transition>

</template>


<script>
  import ListTags from './Item/ListTags';
  import Star from './Item/StarIcon';
  import Plus from './Item/PlusIcon';
  import ListCollections from './Item/ListCollections';

  export default {
    name: 'GifItem',
    components: {
      ListTags, Star, Plus, ListCollections,
    },
    props: ['src', 'tags', 'showingPlus'],
    data() {
      return {
        listCollectionsShowing: false,
      };
    },
    methods: {
      openList() {
        this.listCollectionsShowing = !this.listCollectionsShowing;
      },
      searchItems(tag) {
        this.$emit('search', tag);
      },
      copy(event) {
        const el = event.target;
        const src = el.getAttribute('src');
        const lengthOldSuffix = 2;
        const newSuffix = 'orig';
        const newSrc = src.slice(0, src.length - lengthOldSuffix) + newSuffix;

        this.$emit('copy', newSrc);
      },
      removeInCollection() {
        const name = 'collections';
        const collections = JSON.parse(localStorage.getItem(name));
        const nameList = document.querySelector('.search-tag').textContent.trim().slice(1);

        for (let i = 0; i < collections.length; i += 1) {
          if (nameList === collections[i].name) {
            for (let j = 0; j < collections[i].items.length; j += 1) {
              if (this.src === collections[i].items[j].src) {
                collections[i].items.splice(j, 1);
                break;
              }
            }
          }
        }
        localStorage.setItem(name, JSON.stringify(collections));
        this.$emit('remove-item', this.src);
      },
    },
  };

</script>


<style lang="scss">

  @import "../../assets/scss/vars.scss";

  .item.animated {
    animation-duration: .4s;
    animation-fill-mode: both;
  }

  .thumb {
    position: relative;
    min-height: 80px;
    height: auto;
    line-height: 0;
    border: 2px solid rgba(255, 255, 255, 0);
    box-shadow: 2px 2px 2px rgba(0, 0, 0, 0.2);
    background-color: #fff;
    cursor: pointer;
    transition: all ease .4s;
    margin: 15px 0 25px 0;
    &:after {
      content: '';
      position: absolute;
      height: 40px;
      z-index: 10;
      width: 100%;
      background: linear-gradient(to bottom, #000, rgba(0, 0, 0, 0.0));
      top: 0;
      left: 0;
      opacity: 0;
      transition: all .8s ease;
    }
    .fa-star {
      opacity: 1;
      color: $colorStar;
    }
    &:hover {
      box-shadow: 0 0 6px 2px rgba(33, 148, 231, 0.61);
      border: 2px solid $accentHover;
      .fa-minus-square {
        opacity: 1;
      }
      .close-item {
        opacity: 1;
      }
      .item-tags {
        background-color: rgba(0, 0, 0, 0.69);
      }
      &:after {
        opacity: 1;
      }
      .fa-star-o {
        opacity: 1;
      }
      .fa-plus-square {
        opacity: 1;
      }
    }
    img {
      width: 100%;
      position: relative;
    }
  }

  @media only screen and (max-width: 992px) {
    .thumb:after {
      opacity: 1;
      background: linear-gradient(to bottom, rgba(0, 0, 0, 0.6), rgba(0, 0, 0, 0.0));
    }
  }
  @media only screen and (max-width : 480px){
    .col-6 {
      padding-left: 10px;
      padding-right: 10px;
    }
    .thumb {
      min-height: 60px;
      margin: 10px 0 15px 0;
    }
  }

</style>
