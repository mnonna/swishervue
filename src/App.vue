<template>
  <div class="app">
    <div class="nav">
      <router-link class="about-link" to="/about">About</router-link>
      <router-link class="swisher-link" to="/">Swisher</router-link>
    </div>
    <transition name="cards-animation" enter-active-class="animated fadeIn" leave-active-class="animated fadeOut">
      <router-view></router-view>
    </transition>
  </div>
</template>

<style lang="scss" scoped>
@import url('https://fonts.googleapis.com/css?family=Lobster|Montserrat&display=swap');
@import url('https://cdn.jsdelivr.net/npm/animate.css@3.7.2/animate.min.css');
  .app{
    display: flex !important;
    width: 100%;
    height: 100%;
    position: absolute;
      .nav{
        position: fixed;
        z-index: 2;
        width: 200px;
        display: flex;
        height: 5%;
        justify-content: space-evenly;
        align-items: center;
        a{
          text-decoration: none;
          color: white;
          font-size: 1em;
          font-family: 'Montserrat', sans-serif;
        }
        a:hover{
          color: black;
        }
     }
    .footer{
      align-self: flex-end;
      width: 100%;
      z-index: 2;
      height: 40px;
      display: flex;
      justify-content: center;
      align-items: center;
      font-family: 'Montserrat', sans-serif;
      font-size: 1em;
      color: rgb(0, 0, 0);
    }
  }
  @media screen and (max-width: 500px) {
    .app{
      .nav{
        width: 100%;
        justify-content: space-between;
        .about-link{
          padding-left: 10px;
        }
        .swisher-link{
          padding-right: 10px;
        }
      }
    }
  }
  @media screen and (min-width: 501px) and (max-width: 768px) {
    .app{
      .nav{
        padding-top: 5px;
        width: 100%;
        justify-content: space-between;
        .about-link{
          padding-left: 10px;
        }
        .swisher-link{
          padding-right: 10px;
        }
      }
    }
  }
    @media screen and (min-width: 769px) and (max-width: 1024px) {
    .app{
      .nav{
        padding-top: 5px;
        width: 100%;
        justify-content: flex-start;
        .about-link{
          padding-left: 10px;
          padding-right: 10px;
        }
      }
    }
  }
</style>
