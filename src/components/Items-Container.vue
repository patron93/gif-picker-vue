<template>
  <div class="items-container">
    <div class="container-fluid">
      <div class="row align-items-end items-container js-items-container">

        <search-panel
          :searchingTag="searchingTag"
          :state="searching"
          @close="close"
        />

        <gif-item
          v-for="item in items"
          :key="item.src"
          :src="item.src"
          :tags="item.keys"
          :showing-plus="showingPlus"
          @search="searchItems"
          @copy="copySrc"
          @remove-item="removeItem"
        />

        <transition name="done" @after-enter="animationCopy">
          <div class="done" v-show="animationShow"></div>
        </transition>

        <p class="select"></p>

      </div>
    </div>
  </div>
</template>


<script>
  import GifItem from './ItemsContainer/Gif-Item';
  import SearchPanel from './ItemsContainer/Search-Panel';


  export default {
    name: 'ItemsContainer',
    props: ['collections'],
    components: {
      GifItem, SearchPanel,
    },
    data() {
      return {
        allItems: this.collections,
        items: [],
        count: 0,

        searching: false,
        searchingTag: '',

        animationShow: false,
        showingPlus: true,
        FAVORITES: 'favorites',
      };
    },

    methods: {
      load() {
        const amountLoadItems = 24;
        const amountItem = this.allItems.length;
        const rest = amountItem - this.count;

        if (this.count === amountItem) {
          return;
        }
        if (rest >= amountLoadItems) {
          for (let i = this.count; i < amountLoadItems + this.count; i += 1) {
            this.items.push(this.allItems[i]);
          }
          this.count += amountLoadItems;
        } else {
          for (let i = this.count; i < rest + this.count; i += 1) {
            this.items.push(this.allItems[i]);
          }
          this.count += rest;
        }
      },
      lazy() {
        const pageHeight = document.body.scrollHeight;
        const positionScroll = window.scrollY + document.documentElement.clientHeight;
        const diffHeight = pageHeight - positionScroll;

        if (diffHeight <= 0) {
          this.load();
        }
      },
      removeAllItems() {
        this.items = [];
        this.count = 0;
      },
      searchItems(tag) {
        const result = [];
        let tagsArr;
        const collection = this.collections;

        for (let i = 0; i < collection.length; i += 1) {
          tagsArr = collection[i].keys;

          if (tagsArr.indexOf(tag) > -1) {
            result.push(collection[i]);
          }
        }
        this.removeAllItems();
        this.searching = true;
        this.searchingTag = tag;
        this.allItems = result;
        this.load();
        this.showingPlus = true;
      },
      close() {
        this.removeAllItems();
        this.searching = false;
        this.allItems = this.collections;
        this.load();
        this.showingPlus = true;
      },
      copySrc(src) {
        const el = document.querySelector('.select');
        const range = document.createRange();

        el.textContent = src;

        window.getSelection().removeAllRanges();

        range.selectNode(el);
        window.getSelection().addRange(range);
        document.execCommand('copy');
        window.getSelection().removeAllRanges();

        this.animationShow = !this.animationShow;
      },
      animationCopy() {
        this.animationShow = !this.animationShow;
      },
      showFavorites() {
        const favorites = JSON.parse(localStorage.getItem(this.FAVORITES)) || [];
        this.removeAllItems();
        this.searching = true;
        this.searchingTag = this.FAVORITES;
        this.allItems = favorites;
        this.load();
        this.showingPlus = true;
      },
      showCollection(name, collection) {
        this.removeAllItems();
        this.searching = true;
        this.searchingTag = name;
        this.allItems = collection;
        this.load();
        this.showingPlus = false;
      },
      removeItem(key) {
        for (let i = 0; i < this.items.length; i += 1) {
          if (key === this.items[i].src) {
            this.items.splice(i, 1);
          }
        }
      },
    },

    created() {
      this.load();
      window.addEventListener('scroll', this.lazy);
    },
    mounted() {
      this.$root.$on('search', (tag) => {
        this.searchItems(tag);
      });
      this.$root.$on(this.FAVORITES, () => {
        this.showFavorites();
      });

      this.$root.$on('show-collection', (name, collection) => {
        this.showCollection(name, collection);
      });
    },
  };

</script>


<style lang="scss">
  @import '../assets/scss/vars.scss';

  .done {
    position: relative;
    &:before {
      content: '';
      width: 100%;
      height: 15px;
      left: 0;
      top: 0;
      position: fixed;
      background-color: green;
      z-index: 200;
      margin-top: 50px;
    }
    &:after {
      content: '';
      position: fixed;
      height: 15px;
      background-color: green;
      width: 100%;
      bottom: 0;
      left: 0;
      z-index: 200;
    }
  }

  .done-enter-active {
    transition: ease-in opacity .1s;
  }

  .done-leave-active {
    transition: ease-in opacity .5s;
  }

  .done-enter, .done-leave-to {
    opacity: 0;
  }

  .done-enter-to, .done-leave {
    opacity: 1;
  }

</style>
