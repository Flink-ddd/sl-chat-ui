<template>
  <div id="app">
		<router-view></router-view>
    <div class="web-buttons-info">
      <p>@Latest update time 2023-11-29</p>
    </div>

  </div>
</template>

<script>


export default {
  name: 'App',
  components: {
  }
}
</script>

<style>
 @import './assets/style/global.css';
 
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  position: absolute;
  height: 100%;
  width: 100%;
  background-image: url(~@/assets/images/WechatIMG252.jpeg);
  background-size: cover;
}
 .web-buttons-info{
   text-align: center;
   position: absolute;
   color:white;
   bottom: 1%;
   width: 100%;
   cursor: pointer;
   &:hover {
     color: palegreen;
   }
 }
</style>
