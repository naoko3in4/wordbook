<template>
  <div>
    <nuxt />
  </div>
</template>

<style>
html {
  font-family: 'Source Sans Pro', -apple-system, BlinkMacSystemFont, 'Segoe UI',
    Roboto, 'Helvetica Neue', Arial, sans-serif;
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
  background: #e8edee;
}

*,
*:before,
*:after {
  box-sizing: border-box;
  margin: 0;
}

.button--blue {
  display: inline-block;
  border-radius: 4px;
  border: 2px solid #0a6095;
  color: #0a6095;
  font-weight: 700;
  text-decoration: none;
  padding: 10px 30px;
}

.button--blue:hover {
  color: #fff;
  background-color: #0a6095;
}

.button--green {
  display: inline-block;
  border-radius: 4px;
  border: 2px solid #3b8070;
  color: #3b8070;
  font-weight: 700;
  text-decoration: none;
  padding: 10px 30px;
}

.button--green:hover {
  color: #fff;
  background-color: #3b8070;
}

.button--grey {
  display: inline-block;
  border-radius: 4px;
  border: 2px solid #35495e;
  color: #35495e;
  font-weight: 700;
  text-decoration: none;
  padding: 10px 30px;
  margin-left: 15px;
}

.button--grey:hover {
  color: #fff;
  background-color: #35495e;
}

.container {
  margin: 0 auto;
  min-height: 100vh;
  align-items: center;
  text-align: center;
}

.back__btn {
  margin: 0 auto;
}
</style>
