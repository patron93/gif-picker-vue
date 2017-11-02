<template>

  <transition name="custom"
              enter-active-class="animated fadeIn"
              leave-active-class="animated fadeOut">

    <div class="popup-wrap" v-if="showing" @keyup.enter="addCollection">
      <div class="popup-dialog">
        <div class="container">
          <div class="popup-close-btn" @click="showing = !showing"><i class="fa fa-times" aria-hidden="true"></i></div>

          <div class="row justify-content-md-center">
            <p class="popup-message" :class="{ error: showError }">{{showMessage}}</p>
          </div>

          <div class="row justify-content-md-center">
            <div class="col-9">
              <input class="popup-input" name="popup" type="text" placeholder="enter name"
                     autofocus
                     v-model="value"
                     :class="{ error: showError }">
            </div>

            <div class="col-3">
              <div class="popup-add-btn" @click="addCollection">add</div>
            </div>
          </div>

        </div>
      </div>
    </div>

  </transition>
</template>


<script>

  export default {
    name: 'Popup',
    data() {
      return {
        showMessage: '',
        messages: {
          start: 'pls enter the name of the collection',
          errorEmpty: 'name can not be empty',
          errorExist: 'that name already exists',
        },
        showing: false,
        showError: false,
        value: '',
        COLLECTIONS: 'collections',
      };
    },
    methods: {
      addCollection() {
        const collections = JSON.parse(localStorage.getItem(this.COLLECTIONS)) || [];
        if (!this.value) {
          this.showError = true;
          this.showMessage = this.messages.errorEmpty;
          return;
        }

        if (collections.length === 0) {
          collections.push({
            name: this.value,
            items: [],
          });
          localStorage.setItem(this.COLLECTIONS, JSON.stringify(collections));

          this.showing = false;
          this.value = '';
        } else {
          for (let i = 0; i < collections.length; i += 1) {
            const nameTrim = this.value.trim();
            if (nameTrim === collections[i].name) {
              this.showError = true;
              this.showMessage = this.messages.errorExist;
              break;
            } else if (!this.value) {
              this.showError = true;
              this.showMessage = this.messages.errorEmpty;
            } else {
              collections.push({
                name: this.value,
                items: [],
              });
              localStorage.setItem(this.COLLECTIONS, JSON.stringify(collections));
              this.showing = false;
              this.value = '';
            }
          }
        }
      },

    },
    mounted() {
      this.showMessage = this.messages.start;
      this.$root.$on('showPopup', () => {
        this.showing = true;
      });
      window.addEventListener('keydown', (event) => {
        if (event.key === 'Escape') {
          this.showing = false;
        }
      });
    },
  };

</script>


<style lang="scss">

  @import '../assets/scss/vars.scss';

  .popup-wrap {
    display: flex;
    position: fixed;
    top: 0;
    left: 0;
    height: 100vh;
    width: 100vw;
    z-index: 1000;
    background-color: rgba(0, 0, 0, 0.53);
    justify-content: center;
    &.animated {
      animation-duration: .6s;
      animation-fill-mode: both;
    }
    .popup-dialog {
      display: flex;
      width: 50%;
      height: 200px;
      background-color: $headerColor;
      align-self: center;
      position: relative;
      justify-content: center;
      padding: 20px 20px;
      box-shadow: 0 0 10px 2px rgba(0, 0, 0, 0.78);
      .error {
        color: red;
        border: none;

      }
      .popup-message {
        font-size: 1.2em;
        display: inline;
        margin-top: 40px;
        margin-bottom: 10px;
        cursor: default;

      }
      .popup-close-btn {
        position: absolute;
        font-size: 1.2em;
        top: 10px;
        right: 15px;
        transition: all ease .4s;
        cursor: pointer;
        &:hover {
          color: $accentHover;
        }

      }

      .popup-input {
        font-family: $default-font;
        font-size: 1.2em;
        align-self: center;
        height: 1.7em;
        width: 100%;
        border-radius: 5px;
        border: solid 1px $textColor;
        padding-left: 10px;
        outline: none;
        transition: all ease-out .4s;
        color: $textColor;

        &::placeholder {
          color: rgba(0, 0, 0, 0.32);
        }
        &:focus {
          border-radius: 5px;
          border: 1px solid $accentHover;
          box-shadow: 0 0 2px 1px $accentHover;
        }
      }
      .error {

        &:focus {
          border: 1px solid red;
          box-shadow: 0 0 2px 1px red;
        }
      }
      .col-3, .col-9 {
        padding: 0 5px;
      }
      .popup-add-btn {
        line-height: 1.6em;
        width: 100%;
        height: 1.7em;
        border: 1px solid $accent;
        font-size: 1.2em;
        border-radius: 5px;
        text-align: center;
        color: $accent;
        cursor: pointer;
        transition: all ease .3s;
        &:hover {
          background-color: $accentHover;
          color: $headerColor;
          box-shadow: 0 0 3px 1px rgba(34, 164, 255, 0.61);
        }
      }
    }
  }

  @media only screen and (max-width: 992px) {
    .popup-wrap {
      .popup-dialog {
        width: 80%;
        .popup-message {
          width: 100%;
          margin-top: 40px;
          margin-bottom: 20px;
        }
      }
    }
  }

  @media only screen and (max-width: 768px) {
    .popup-wrap {
      .popup-dialog {
        .popup-message {
          font-size: 1em;
        }
      }
    }
  }

  @media only screen and (max-width: 480px) {
    .popup-wrap {
      .popup-dialog {
        width: 90%;
        .popup-input {
          font-size: 1em;
        }
        .popup-add-btn {
          font-size: 1em;
        }
      }
    }
  }

</style>
