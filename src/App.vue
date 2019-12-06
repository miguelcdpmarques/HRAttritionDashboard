<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<style lang="scss">
body {
  background-color: #F2EFEF;
}
h1 {
  font-size: 1.25rem;
  font-weight: 600;
  margin: 1rem 2rem;
  color: black;
}

// Import Bulma's core
@import "~bulma/sass/utilities/_all";

$primary: #0B0D3E;

// Import Bulma and Buefy styles
@import "~bulma";
@import "~buefy/src/scss/buefy";
</style>
