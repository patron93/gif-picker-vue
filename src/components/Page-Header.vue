<template>
  <header>
    <div class="container-fluid">
      <div class="row align-content-center justify-content-center">
        <h1>GIF Picker</h1>
      </div>
    </div>
  </header>
</template>

<script>
  export default {
    name: 'PageHeader',
  };
</script>

<style lang="scss">
  header {
    position: fixed;
    height: 50px;
    width: 100%;
    top: 0;
    left: 0;
    line-height: 50px;
    background-color: #fff;
    box-shadow: 0 0 3px 2px rgba(0, 0, 0, 0.2);
    cursor: default;
    color: rgba(0, 0, 0, 0.55);
    transition: all ease .8s;
    z-index: 400;
    font-family: 'raleway', sans-serif;
    -webkit-transform: translateZ(0);
    &:hover {
      color: #2194e7;
      box-shadow: 0 0 3px 2px rgba(33, 148, 231, 0.49);
    }
    h1 {
      margin: 0;
    }
  }
</style>
