<template>
  <div class="col-12 tags-panel">

    <ul :class="more ? 'all-tags' : 'popular'">
      <li :class="more ? 'all-tags-li' : 'popular-li'" v-for="tag in tags" @click="search">#{{tag.name}}</li>
    </ul>

    <div class="toggle-tags" @click="allTags">
      <p><span>show {{ more ? 'less' : 'more'}} tags</span></p>
    </div>

  </div>
</template>


<script>

  export default {
    name: 'TagsPanel',
    props: ['collections'],
    data() {
      return {
        more: false,
      };
    },
    computed: {
      tags() {
        const tagsArr = [];
        const tagsObj = {};

        for (let i = 0; i < this.collections.length; i += 1) {
          for (let j = 0; j < this.collections[i].keys.length; j += 1) {
            const tag = this.collections[i].keys[j];
            if (tagsObj[tag]) {
              tagsObj[tag] += 1;
            } else {
              tagsObj[tag] = 1;
            }
          }
        }

        Object.keys(tagsObj).forEach((key) => {
          tagsArr.push({
            name: key,
            value: tagsObj[key],
          });
        });

        tagsArr.sort((a, b) => b.value - a.value);
        return tagsArr;
      },
    },
    methods: {
      search(event) {
        this.$root.$emit('search', event.target.textContent.slice(1));
      },
      allTags() {
        this.more = !this.more;

        if (!this.more) {
          document.querySelector('.all-tags').scrollTop = 0;
        }
      },
    },
  };

</script>


<style lang="scss">

  @import '../assets/scss/vars.scss';

  .tags-panel {
    color: $accent;
    font-size: .9em;
    cursor: pointer;
    padding-top: 15px;
    padding-bottom: 15px;
    .popular {
      list-style-type: none;
      padding: 0;
      margin: 0;
      text-align: center;
      font-weight: 500;
      max-height: 1.9em;
      overflow: hidden;
      line-height: 2em;
      transition: all ease .3s;
      .popular-li {
        list-style-type: none;
        display: inline-block;
        padding: 0 10px;
        transition: all ease .3s;
        &:hover {
          color: #22bbff;
          text-shadow: 0 0 3px rgba(33, 148, 231, 0.44);
        }
      }
    }
  }
  .toggle-tags {
    display: inline;
    text-align: center;
    p {
      margin: 20px 0;
      text-transform: uppercase;
      font-weight: 500;
      font-size: .9em;
      color: $textColor;
      transition: all ease .5s;
      &:hover {
        color: #3dbdff;
        text-shadow: 0 0 7px rgba(33, 148, 231, 0.31);
        span {
          &::after, &::before {
            background-color: #3dbdff;
            box-shadow: 0 0 7px rgba(33, 148, 231, 0.61);
          }
        }
      }
      span {

        &::after, &::before {
          content: '';
          height: 1px;
          width: 20%;
          position: relative;
          vertical-align: middle;
          background-color: rgba(108, 117, 125, 0.45);
          display: inline-block;
          margin: 0 10px;
          transition: all ease .3s;
        }
      }
    }
  }
  .all-tags {
    list-style-type: none;
    margin: 0;
    text-align: center;
    font-weight: normal;
    font-size: .9em;
    line-height: 2em;
    max-height: 300px;
    transition: all ease-in .5s;
    background-color: $headerColor;
    padding: 20px 15px;
    overflow: auto;
    .all-tags-li {
      padding: 1px 10px;
      margin: 3px 3px;
      display: inline-block;
      background-color: #f1f1f1;
      border: 1px solid rgba(134, 134, 134, 0);
      transition: all ease .3s;
      color: $textColor;
      font-size: .9em;
      &:hover {
        color: $accent;
        box-shadow: 0 0 15px 1px rgba(33, 148, 231, 0.61);
        border: 1px solid #22a4ff;
      }
    }
  }

</style>
