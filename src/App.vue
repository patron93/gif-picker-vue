<template>
  <div id="app">
    <page-header/>
    <aside-menu/>
    <tags-panel :collections="gifCollections"/>

    <items-container :collections="gifCollections"/>
    <scroll-top/>
    <popup/>
  </div>
</template>

<script>
  import Gif from './components/gif';
  import PageHeader from './components/Page-Header';
  import TagsPanel from './components/Tag-Panel';
  import ItemsContainer from './components/Items-Container';
  import AsideMenu from './components/AsideMenu';
  import Popup from './components/Popup';
  import ScrollTop from './components/ScollTop';

  export default {
    name: 'app',
    components: {
      PageHeader,
      TagsPanel,
      ItemsContainer,
      AsideMenu,
      Popup,
      ScrollTop,
    },
    data() {
      return {
        gifCollections: Gif.items,
      };
    },
  };
</script>

<style lang="scss">

  @import "../static/bootstrap/dist/css/bootstrap-grid.min.css";
  @import "../static/font-awesome/font-awesome.min.css";
  @import "../static/animate.min.css";
  @import "assets/scss/fonts";

  .select {
    height: 0;
    font-size: 0;
  }

  #app {
    font-family: 'raleway', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
