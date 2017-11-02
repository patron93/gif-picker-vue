<template>

  <aside class="menu">
    <div class="menu-icon" @click="toggleMenu">
      <i class="fa fa-bars" aria-hidden="true"></i>
    </div>

    <transition name="custom"
                enter-active-class="animated slideInLeft"
                leave-active-class="animated slideOutLeft">
      <div class="menu-wrap" v-show="opened">
        <ul class="menu-items">

          <li class="menu-item favorites" @click="showFavorites">
            <i class="fa fa-star" aria-hidden="true"></i>favorites
          </li>

          <li class="menu-item"><i class="fa fa-list" aria-hidden="true"></i>list collection
            <i class="fa fa-plus-circle" aria-hidden="true" @click="addList"></i>

            <ul class="menu-list">

              <li class="menu-list-li"
                  v-for="collection in collections()"
                  :key="collection.name"
                  @click="showCollection">
                <i class="fa fa-file-text-o" aria-hidden="true"></i>
                {{collection.name}}
                <i class="fa fa-trash-o" aria-hidden="true" @click.stop="removeCollection"></i>
              </li>

            </ul>
          </li>

        </ul>
      </div>
    </transition>

  </aside>

</template>


<script>

  export default {
    name: 'AsideMenu',
    data() {
      return {
        opened: false,
        FAVORITES: 'favorites',
        COLLECTIONS: 'collections',
      };
    },
    methods: {
      toggleMenu() {
        this.opened = !this.opened;
      },
      showFavorites() {
        this.$root.$emit(this.FAVORITES);
        this.opened = false;
      },
      addList() {
        this.$root.$emit('showPopup');
        this.opened = false;
      },
      collections() {
        return JSON.parse(localStorage.getItem(this.COLLECTIONS));
      },
      removeCollection(event) {
        const name = event.target.closest('.menu-list-li').textContent.trim();

        const collections = this.collections();

        for (let i = 0; i < collections.length; i += 1) {
          if (name === collections[i].name) {
            collections.splice(i, 1);
            break;
          }
        }
        localStorage.setItem(this.COLLECTIONS, JSON.stringify(collections));
        event.target.closest('.menu-list-li').remove();
      },
      showCollection(event) {
        const collections = this.collections();
        const name = event.target.closest('.menu-list-li').textContent.trim();
        let collection = [];

        for (let i = 0; i < collections.length; i += 1) {
          if (name === collections[i].name) {
            collection = collections[i].items;
          }
        }

        this.$root.$emit('show-collection', name, collection);
        this.opened = false;
      },
    },
  };

</script>


<style lang="scss">

  @import '../assets/scss/vars.scss';

  .menu {
    .menu-icon {
      position: fixed;
      top: 5px;
      left: 10px;
      z-index: 500;
      color: $accent;
      line-height: 1em;
      font-size: 2em;
      cursor: pointer;
      transition: all ease .4s;
      padding: 5px 10px;
      border-radius: 5px;
      &:hover {
        color: $accentHover;
        background-color: #ececec;
      }
      &:active {
        background-color: $accentHover;
        color: #fff;
      }
    }
    .menu-wrap {
      position: fixed;
      width: 350px;
      height: calc(100% - 50px);
      top: 50px;
      left: 0;
      background-color: #f9f9f9;
      z-index: 300;
      box-shadow: 0 2px 2px 2px rgba(0, 0, 0, 0.21);
      -webkit-transform: translateZ(0);
      overflow: auto;
      &.animated {
        animation-duration: .6s;
        animation-fill-mode: both;
      }
    }
    .menu-items {
      text-align: left;
      padding: 0;
      list-style: none;
      text-transform: uppercase;
      font-weight: 500;
      font-size: 1.1em;
      line-height: 2.3em;
      cursor: pointer;
      margin: 15px 20px;
      .menu-item {
        border-bottom: 1px solid rgba(108, 117, 125, 0.3);
        padding: 10px 5px;
        position: relative;
      }
      .favorites {
        transition: all ease .4s;
        &:hover {
          color: $accentHover;
          .fa-star {
            font-size: 1.6em;
            color: $accentHover;
          }
        }
      }
      i {
        padding-right: 15px;
        color: $accent;
        transition: all ease .4s;
        font-size: 1.2em;
      }
      .fa-plus-circle {
        color: #00dc00;
        font-size: 1em;
        position: absolute;
        top: 13px;
        margin-left: 5px;
        &:hover {
          color: #00ff00;
        }
      }
      .menu-list {
        list-style: none;
        padding: 0;
        margin-right: 30px;
        font-size: .8em;
        text-transform: none;
        .menu-list-li {
          padding: 0 15px;
          transition: all ease .4s;
          border-radius: 3px;
          position: relative;
          .fa-trash-o {
            position: absolute;
            right: 0;
            top: 12px;
            opacity: 0;
          }
          &:hover {
            background-color: #efefef;
            color: $accentHover;
            i {
              font-size: 1.6em;
              color: $accentHover;
            }
            .fa-trash-o {
              font-size: 1.2em;
              opacity: 1;
              color: #858585;
              &:hover {
                color: red;
              }
            }
          }
        }
      }
    }
  }
  @media only screen and (max-width : 480px){

    .menu {
      .menu-wrap {
        width: 280px;
      }
    }
  }

</style>
