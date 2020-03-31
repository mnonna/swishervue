<template>
  <div class="home">
    <div class="input-container">
      <h1>Swisher</h1>
      <input id="search" name="search" autofocus v-model="searchInput" @input="handleInput"/>
      <label for="search"><small>Search for a NBA team</small></label>
      <div class="team-info-wrapper" v-for="item in results" :key="item.teamID">
        <img :src="item.logo" />
        <p>Full team name: {{ item.fullName }}</p>
        <p>City: {{ item.city }}</p>
        <p>Conference played: {{ item.leagues.standard.confName }}</p>
        <p>Division played: {{ item.leagues.standard.divName }}</p>
        <!-- eslint-disable-next-line -->
        <button class="check-team-stats-button" type="button" @click="getTeamStats(item.teamId)">Stats</button>
      </div>
    </div>
    <div ref="teamSeasonStats" v-if="showStats" class="team-stats-container">
      <div class="stats-container-nav">
        <div class="quit-team-stats" @click="closeStats">
          <div class="line-left-right"></div>
          <div class="line-right-left"></div>
        </div>
      </div>
      <div class="team-stats">
        <div class="team-stats-image" v-for="item in results" :key="item.teamID">
          <img :src="item.logo" />
          <p>{{ item.fullName }}</p>
        </div>
        <div class="team-stats-text" v-for="stat in stats" :key="stat.teamId">
          <p>Wins: {{ stat.win }}</p>
          <p>Losses: {{ stat.loss }}</p>
          <p>Games behind: {{ stat.gamesBehind }}</p>
          <p>Conference rank: {{ stat.conference.rank }}</p>
          <p>Division rank: {{ stat.division.rank }}</p>
          <p>Win percentage: {{ stat.winPercentage }}</p>
          <p>Loss percentage: {{ stat.lossPercentage }}</p><br>
          <p><small>* Stats for season 2017/2018</small></p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import debounce from 'lodash.debounce';

const api = 'https://api-nba-v1.p.rapidapi.com/teams/nickName/';
const apiStats = 'https://api-nba-v1.p.rapidapi.com/standings/standard/2018/teamId/';

export default {
  name: 'Home',
  data() {
    return {
      searchInput: ' ',
      results: [],
      currentTeamID: ' ',
      stats: [],
      showStats: false,
      currLogo: ' ',
    };
  },
  methods: {
    // eslint-disable-next-line
    handleInput: debounce(function(){ 
      axios({ method: 'GET', url: `${api}${this.searchInput}`, headers: { 'x-rapidapi-key': '7dc2c913d6mshd6d042f74dab9cfp135731jsn240571f9ae54' } }).then((response) => { this.results = response.data.api.teams; }).catch((error) => { console.log(error); });
    }, 300),
    // eslint-disable-next-line
    getTeamStats: debounce(function(teamID){
      axios({ method: 'GET', url: `${apiStats}${teamID}`, headers: { 'x-rapidapi-key': '7dc2c913d6mshd6d042f74dab9cfp135731jsn240571f9ae54' } }).then((response) => { this.stats = response.data.api.standings; this.showStats = true; }).catch((error) => { console.log(error); });
    }),
    closeStats() {
      this.showStats = false;
    },
  },
};
</script>

<style lang="scss" scoped>
  @import url('https://fonts.googleapis.com/css?family=Lobster|Montserrat&display=swap');

  .home{
    position: absolute;
    width: 100%;
    min-height: 100vh;
    height: auto;
    background: linear-gradient(rgba(71, 71, 71, 0.671), rgba(117, 117, 117, 0.548)), url("./img/basketball-2258651_1920.jpg");
    background-size: cover;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    .input-container{
      align-self: flex-end;
      margin-right: 500px;
      width: 400px;
      min-height: 300px;
      height: auto;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      label{
        margin-top: 10px;
        color: white;
        font-family: 'Montserrat', sans-serif;
        font-size: 1em;
      }
      input{
        width: 100%;
        border: 0;
        height: 30px;
        border-bottom: 2px solid black;
        background-color: transparent;
        text-align: center;
        color: white;
        outline: 0;
        font-family: 'Montserrat', sans-serif;
        font-size: 1em;
      }
      h1{
        font-family: 'Lobster', cursive;
        font-size: 60px;
        letter-spacing: 3px;
        color: white;
      }
    }
    .team-info-wrapper{
      margin-top: 50px;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      img{
        width: 200px;
        height: 200px;
      }
      p{
        font-family: 'Montserrat', sans-serif;
        font-size: 1em;
        color: white;
      }
      .check-team-stats-button{
        cursor: pointer;
        margin-top: 5px;
        padding: 20px 30px;
        outline: 0;
        background-color:rgba(0, 0, 0, 0.918);
        color: white;
        font-family: 'Montserrat', sans-serif;
        font-size: 1em;
        border: none;
      }
      .check-team-stats-button:hover{
        color: rgb(0, 0, 0);
        background-color: rgb(221, 145, 46);
      }
    }
    .team-stats-container{
      position: absolute;
      min-width: 800px;
      min-height: 500px;
      height: auto;
      background-color: rgba(250, 235, 215, 0.897);
      display: flex;
      flex-direction: column;
      border-radius: 20px;
      box-shadow: 6px 8px rgba(148, 148, 148, 0.377);
      .stats-container-nav{
        display: flex;
        justify-content: flex-end;
        align-items: center;
        width: 100%;
        height: 60px;
        .quit-team-stats{
          position: relative;
          margin-right: 20px;
          width: 40px;
          height: 40px;
          cursor: pointer;
          display: flex;
          align-items: center;
          .line-left-right{
            position: absolute;
            height: 4px;
            width: 100%;
            background-color: black;
            transform: rotate(45deg);
            transition: 1s cubic-bezier(0.19, 1, 0.22, 1) .2s;
          }
          .line-right-left{
            position: absolute;
            height: 4px;
            width: 100%;
            background-color: black;
            transform: rotate(-45deg);
            transition: 1s cubic-bezier(0.19, 1, 0.22, 1) .2s;
          }
        }
        .quit-team-stats:hover .line-right-left{
          background-color: rgb(221, 145, 46);
          transform: rotate(45deg);
        }
        .quit-team-stats:hover .line-left-right{
          background-color: rgb(221, 145, 46);
          transform: rotate(-45deg);
        }
      }
      .team-stats{
        width: 100%;
        min-height: 440px;
        height: auto;
        display: flex;
        align-items: center;
        justify-content: space-around;
        .team-stats-image{
          position: relative;
          width: 300px;
          height: 300px;
          img{
            width: 100%;
            height: 90%;
          }
          p{
            font-family: 'Montserrat', sans-serif;
            font-size: 1.5em;
            text-align: center;
          }
        }
        .team-stats-text p{
          font-family: 'Montserrat', sans-serif;
          font-size: 1em;
        }
      }
    }
  }
  @media screen and (max-width: 320px) {
    .home{
    .input-container{
      margin-right: 0;
      width: 100%;
      label{
        font-size: 1em;
      }
      input{
        width: 80%;
        height: 30px;
        font-size: 1em;
      }
      h1{
        font-size: 35px;
        letter-spacing: 3px;
      }
    }
    .team-info-wrapper{
      p{
        text-align: center;
        font-size: 1em;
      }
      .check-team-stats-button{
        cursor: pointer;
        margin-top: 5px;
        margin-bottom: 20px;
        font-size: 1em;
      }
    }
    .team-stats-container{
      min-width: 300px;
      min-height: 500px;
      height: auto;
      .stats-container-nav{
        width: 100%;
        height: 60px;
        .quit-team-stats{
          margin-right: 20px;
          width: 40px;
          height: 40px;
          .line-left-right{
            height: 4px;
            width: 100%;
          }
          .line-right-left{
            height: 4px;
            width: 100%;
          }
        }
      }
      .team-stats{
        width: 100%;
        min-height: 440px;
        height: auto;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: space-between;
        .team-stats-image{
          width: 200px;
          height: 250px;
          img{
            width: 100%;
            height: 70%;
          }
          p{
            font-size: 1.2em;
          }
        }
        .team-stats-text p{
          font-size: 1em;
        }
      }
    }
  }
  }
  @media screen and (min-width: 320px) and (max-width: 500px) {
    .home{
    .input-container{
      margin-right: 0;
      width: 100%;
      label{
        font-size: 1em;
      }
      input{
        width: 80%;
        height: 30px;
        font-size: 1em;
      }
      h1{
        font-size: 35px;
        letter-spacing: 3px;
      }
    }
    .team-info-wrapper{
      p{
        text-align: center;
        font-size: 1em;
      }
      .check-team-stats-button{
        cursor: pointer;
        margin-top: 5px;
        margin-bottom: 20px;
        font-size: 1em;
      }
    }
    .team-stats-container{
      min-width: 300px;
      min-height: 500px;
      height: auto;
      .stats-container-nav{
        width: 100%;
        height: 60px;
        .quit-team-stats{
          margin-right: 20px;
          width: 40px;
          height: 40px;
          .line-left-right{
            height: 4px;
            width: 100%;
          }
          .line-right-left{
            height: 4px;
            width: 100%;
          }
        }
      }
      .team-stats{
        width: 100%;
        min-height: 440px;
        height: auto;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: space-between;
        .team-stats-image{
          width: 200px;
          height: 250px;
          img{
            width: 100%;
            height: 70%;
          }
          p{
            font-size: 1.2em;
          }
        }
        .team-stats-text p{
          font-size: 1em;
        }
      }
    }
  }
  }
  @media screen and (min-width: 500px) and (max-width: 768px) {
    .home{
    .input-container{
      margin-right: 0;
      width: 100%;
      label{
        font-size: 1em;
      }
      input{
        width: 50%;
        height: 30px;
        font-size: 1em;
      }
      h1{
        font-size: 35px;
        letter-spacing: 3px;
      }
    }
    .team-info-wrapper{
      p{
        text-align: center;
        font-size: 1em;
      }
      .check-team-stats-button{
        cursor: pointer;
        margin-top: 5px;
        margin-bottom: 20px;
        font-size: 1em;
      }
    }
    .team-stats-container{
      min-width: 400px;
      min-height: 500px;
      height: auto;
      .stats-container-nav{
        width: 100%;
        height: 60px;
        .quit-team-stats{
          margin-right: 20px;
          width: 40px;
          height: 40px;
          .line-left-right{
            height: 4px;
            width: 100%;
          }
          .line-right-left{
            height: 4px;
            width: 100%;
          }
        }
      }
      .team-stats{
        width: 100%;
        min-height: 440px;
        height: auto;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: space-between;
        .team-stats-image{
          width: 200px;
          height: 250px;
          img{
            width: 100%;
            height: 70%;
          }
          p{
            font-size: 1.2em;
          }
        }
        .team-stats-text p{
          font-size: 1em;
        }
      }
    }
  }
  }
  @media screen and (min-width: 769px) and (max-width: 1024px) {
    .home{
    .input-container{
      margin-right: 0;
      width: 100%;
      label{
        font-size: 1em;
      }
      input{
        width: 50%;
        height: 30px;
        font-size: 1em;
      }
      h1{
        font-size: 35px;
        letter-spacing: 3px;
      }
    }
    .team-info-wrapper{
      p{
        text-align: center;
        font-size: 1em;
      }
      .check-team-stats-button{
        cursor: pointer;
        margin-top: 5px;
        margin-bottom: 20px;
        font-size: 1em;
      }
    }
    .team-stats-container{
      min-width: 400px;
      min-height: 500px;
      height: auto;
      .stats-container-nav{
        width: 100%;
        height: 60px;
        .quit-team-stats{
          margin-right: 20px;
          width: 40px;
          height: 40px;
          .line-left-right{
            height: 4px;
            width: 100%;
          }
          .line-right-left{
            height: 4px;
            width: 100%;
          }
        }
      }
      .team-stats{
        width: 100%;
        min-height: 440px;
        height: auto;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: space-between;
        .team-stats-image{
          width: 200px;
          height: 250px;
          img{
            width: 100%;
            height: 70%;
          }
          p{
            font-size: 1.2em;
          }
        }
        .team-stats-text p{
          font-size: 1em;
        }
      }
    }
  }
  }
</style>
