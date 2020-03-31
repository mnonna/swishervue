<template>
  <div class="about">
    <h1>What is Swisher ?</h1>
    <section>
      <p>Swisher is a simple Vue app for searching infos about NBA teams.</p>
      <p>Type in a nickname of your team (eg. Rockets etc, not a full name - like Houston Rockets ) and see infos about it :)</p>
      <p>App uses an 'API-NBA' API, delivered by rapidapi.com</p>
    </section>
  </div>
</template>

<style lang="scss" scoped>
  @import url('https://fonts.googleapis.com/css?family=Lobster|Montserrat&display=swap');
  .about{
    position: absolute;
    width: 100%;
    height: 100%;
    background: linear-gradient(rgba(71, 71, 71, 0.671), rgba(117, 117, 117, 0.548)), url("./img/basketball-2258651_1920.jpg");
    background-size: cover;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    font-family: 'Montserrat', sans-serif;
    color: white;
    p{
      text-align: center;
    }
  }
  @media screen and (max-width: 1024px) {
    .about{
      h1{
        font-size: 1.2em;
      }
      section{
        width: 80%;
        p{
          font-size: .8em;
        }
      }
    }
  }
</style>
