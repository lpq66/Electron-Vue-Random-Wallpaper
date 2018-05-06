<template>
  <div id="wrapper">
    <random-img></random-img>
  </div>
</template>

<script>
  import RandomImg from './Random/RandomImg'
  export default {
    name: 'landing-page',
    components: { RandomImg },
    data () {
      return {
      }
    }
  }
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Source+Sans+Pro");

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: "Source Sans Pro", sans-serif;
  background: #333;
  color: #fff;
}

header {
  height: 2rem;
  width: 100%;
  background: #222;
}

.images {
  margin-top: 2rem;
}

.form {
  margin-bottom: 1rem;
}

img {
  width: 100%;
  height: auto;
}
#wrapper {
  height: 100%;
  width: 100vw;
}
</style>
