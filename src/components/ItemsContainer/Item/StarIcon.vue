<template>

  <i class="fa star" :class="active ? 'fa-star' : 'fa-star-o'" @click="toggleFavorite"></i>

</template>

<script>

  export default {
    name: 'Star',
    data() {
      return {
        active: false,
        FAVORITES: 'favorites',
      };
    },
    methods: {
      toggleFavorite(event) {
        const itemObj = {
          src: '',
          keys: [],
        };
        const favorites = JSON.parse(localStorage.getItem(this.FAVORITES)) || [];
        const item = event.target.closest('.thumb');
        const src = item.getElementsByTagName('img')[0].getAttribute('src');
        const ul = item.getElementsByClassName('item-tag');

        for (let i = 0; i < ul.length; i += 1) {
          const str = ul[i].textContent.slice(1);
          itemObj.keys.push(str);
        }
        itemObj.src = src;

        if (this.active) {
          for (let i = 0; i < favorites.length; i += 1) {
            if (itemObj.src === favorites[i].src) {
              favorites.splice([i], 1);
              localStorage.setItem(this.FAVORITES, JSON.stringify(favorites));

              break;
            }
          }
        } else {
          favorites.push(itemObj);
          localStorage.setItem(this.FAVORITES, JSON.stringify(favorites));
        }

        this.active = !this.active;
      },
    },
    mounted() {
      const favorites = JSON.parse(localStorage.getItem(this.FAVORITES)) || [];
      const src = this.$parent.$el.getElementsByTagName('img')[0].getAttribute('src');

      for (let i = 0; i < favorites.length; i += 1) {
        if (src === favorites[i].src) {
          this.active = !this.active;
          break;
        }
      }
    },
  };

</script>

<style lang="scss">
  @import "../../../assets/scss/vars.scss";

  .fa-star-o, .star {
    opacity: 0;
    position: absolute;
    color: rgba(255, 255, 255, 0.75);
    font-size: 1.6em;
    top: 8px;
    right: 8px;
    z-index: 150;
    transition: all .4s ease;

    .star {
      opacity: 1;

    }
    &:hover {
      color: $colorStar;
      animation: pulse .8s infinite;
    }
  }
  @media only screen and (max-width : 992px){
    .fa-star-o, .star {
      opacity: .5;
    }
  }
  @media only screen and (max-width : 320px){
    .fa-star-o, .star {
      font-size: 1.4em;
    }
  }


</style>
