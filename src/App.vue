<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<style>
:root {
  box-sizing: border-box;
}
*,
::before,
::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
html {
  font-family: sans-serif; /* 1 */
  -ms-text-size-adjust: 100%; /* 2 */
  -webkit-text-size-adjust: 100%; /* 2 */
}
ul,
ol {
  list-style: none;
}
</style>
