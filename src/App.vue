<script setup>
import {ref} from 'vue'

let num = ref(0)

</script>

<template>
  <!--Navbar importada do bootstrap-->
  <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
    <div class="container-fluid">
      <router-link class="navbar-brand" to="/">
        <img src="https://cdn-icons-png.flaticon.com/512/188/188987.png" alt="logo" width="30" heigth="24" class="d-inline-block align-text-top">
        PokeVite
      </router-Link> 
        <div class="collapse navbar-collapse" id="navbarNav">
      </div>
    </div>
  </nav>

  <!-- Content, para usar o Router -->
  <div class="main">
    <router-view>

    </router-view>
  </div>


  <!--Footer do projeto-->
 <footer class="bg-primary text-light">
  <div>
    <p class="named">Created by Everson |  
    <a href="https://github.com/Evers0n/PokeVite" style="color: white;">  Source code on Github</a> |        Powered by PokeApi.co</p>
  </div>
  </footer>
</template>

<style>
body {
  height: 100vh;
  background: rgb(25, 136, 180);
  background: linear-gradient(0deg, rgba(106, 196, 233, 0.9360119047619048) 0%, rbga(0,41,150,1) 100%);
}
footer {
  position: fixed;
  bottom: 0;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 56px;
}

.named{
  padding-top: 20px;
}

</style>