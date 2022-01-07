<template>
  <v-app id="app">
    <router-view/>
  </v-app>
</template>

<style lang="scss">
// @import '~normalize.css';
// @import '~reset-css/_reset.scss';
* {
    box-sizing: border-box;
}
.loader{
    display: flex;
    height: 100vh;
    align-items: center;
    justify-content: center;
}
</style>
