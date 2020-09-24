<template>
  <span>
    <div class="game" v-if='gameStarted'>
      <div class="circle" 
        v-for="c in circles" :key="c.id" 
        v-bind:style="{ top: c.y+'px', left: c.x+'px', width: circleWidth+'px', height: circleHeight+'px' }"
        v-bind:class="{ hide: c.clicked }"
        v-on:click="circleClicked(c)">
      </div>
    </div>
    <div class="btn-container" v-else>
      <button v-on:click="startGame()" class="btn">Nouvelle partie</button>
    </div>
    <div class="footer" v-bind:style="{height: footerHeight+'px'}">
      <div class="timer" v-if='gameStarted' v-bind:style="{'line-height': footerHeight+'px'}">
        {{time}}s
      </div>
      <div class="score" v-bind:style="{'line-height': footerHeight+'px'}">
        Score : {{score}}
      </div>
    </div>
  </span>
</template>

<script>
export default {
  name: 'Game',
  props: {
    
  },
  data: () => {
    return {
      circles: [],
      score: 0,
      time: 10,
      gameStarted: false,
      circleLeft: 0,

      circleWidth: 50,
      circleHeight: 50,
      footerHeight: 66,
      roundTime: 10,
      nbCircle: 5,

      interval: null
    }
  },
  methods: {
    generateRandomCircles: function(quantity){
      this.circles = []
      for (let index = 0; index < quantity; index++) {
        let x = Math.random() * window.innerWidth - this.circleWidth
        let y = Math.random() * window.innerHeight - this.circleHeight - this.footerHeight
        if(x < 0) x = 0
        if(y < 0) y = 0
        this.circles.push({ id: index, x, y, clicked: false })
      }
    },

    startRound: function() {
      this.circleWidth *= 0.75;
      this.circleHeight *= 0.75;
      this.time = this.roundTime;
      this.circleLeft = this.nbCircle;
      this.generateRandomCircles(this.nbCircle);
      clearInterval(this.interval);
      this.interval = setInterval(function() {
          if(this.time > 0){
            this.time = this.time - 1;
            if(this.time === 0){
              clearInterval(this.interval);
              this.gameStarted = false;
            }
          }
      }.bind(this), 1000);
    },

    initConfig: function(){
      this.circleWidth = 50;
      this.circleHeight = 50;
      this.footerHeight = 66;
      this.roundTime = 10;
      this.nbCircle = 5;
    },

    startGame: function(){
      this.score = 0
      this.initConfig();
      this.startRound();
      this.gameStarted = true;
    },

    circleClicked: function(circle){
      circle.clicked = true;
      this.circleLeft--;
      this.score++;
      if(this.circleLeft === 0)
        this.startRound();
    }
  }
}

/* Build a web page with Vuejs (and any other library), the user sees 5 circles appearing randomly on the screen. The rules are simple :

The player has 10 seconds at the first round to click on all the circles
A score board is displayed at the bottom of the page
Each circle clicked reward 1 point to the player
At the next round :
the time and the size of circles will be reduced by 25%
the scoreboard will remain until the player loses
if the player loses, reset the game

Bonus :

The circles shrink to nothingness during the round
 */

</script>

<style scoped>
  .circle {
    position: absolute;
    width: 20px;
    height: 20px;
    border-radius: 50px;
    background: firebrick;
  }

  .footer{
    background: black;
    color: white;
    position: absolute;
    bottom: 0px;
    width: 100%;
    display: flex;
  }

  .footer > div {
    justify-content: space-between;
    flex: 1;
  }

  .timer{
    font-size: 30px;
    margin: 0 10px
  }

  .score{
    font-size: 30px;
    margin: 0 10px;
    text-align: right;
  }

  .hide{
    display: none;
  }

  .btn{
    height: 44px;
    font-size: 22px;
  }

  .btn-container{
    text-align: center;
    padding-top: 100px;
  }
</style>
