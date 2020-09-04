<template>
  <div id="app">
    <div class="game-box">
      <div class="row">
        <div class="box box1" v-on:click="clickedBox(1)" v-bind:class="{ active: isBoxOneActive}"></div>
        <div class="box box2" v-on:click="clickedBox(2)" v-bind:class="{ active: isBoxTwoActive}"></div>
      </div>
      <div class="row">
        <div class="box box3" v-on:click="clickedBox(3)" v-bind:class="{ active: isBoxThreeActive}"></div>
        <div class="box box4" v-on:click="clickedBox(4)" v-bind:class="{ active: isBoxFourActive}"></div>
      </div>

      <div class="controls-box">
        <div
          class="hard-mode-toggle"
          v-on:click="changeMode()"
          v-bind:class="{red: isHardModeOn}"
        >
          <span v-if="isHardModeOn">Hard</span>
          <span v-else>Normal</span>
        </div>
        <div class="start-button" v-on:click="changeState()" v-bind:class="{red: isPlaying}">&nbsp;</div>
        <div class="counter">{{step}}</div>
      </div>
    </div>
  </div>
</template>

<script>
var timer;

export default {
  name: "app",

  data() {
    return {
      isHardModeOn: false,
      step: "-",
      userTurn: false,
      state: "Start",
      isPlaying: false,
      pattern: [],
      index: 0,
      isBoxOneActive: false,
      isBoxTwoActive: false,
      isBoxThreeActive: false,
      isBoxFourActive: false,
      score: 0,
    };
  },
  methods: {
    changeState: function () {
      if (this.state === "Start") {
        this.state = "Stop";
        this.isPlaying = true;
        this.step = 0;
        this.computerTurn();
      } else {
        this.resetGame();
      }
    },
    computerTurn: function () {
      var self = this;
      this.index = 0;
      this.step++;
      this.pattern.push(this.getRandomNumberOneToFour());
      this.showPattern(function () {
        self.userTurn = true;
      });
    },
    clickedBox: function (boxNum) {
      var self = this;
      if (this.state === "Start") {
        return;
      }
      this.clickEffect(boxNum);
      var isCorrect = this.checkPattern(boxNum);
      if (!isCorrect) {
          self.showPattern();
      } else {
        if (this.index === this.pattern.length - 1) {
          this.score++;
          setTimeout(function () {
            self.userTurn = false;
            self.computerTurn();
          }, 300);
        } else {
          this.index++;
        }
      }
    },
    getRandomNumberOneToFour: function () {
      return Math.floor(Math.random() * 4 + 1);
    },
    checkPattern: function (boxNum) {
      return this.pattern[this.index] === boxNum;
    },
    resetGame: function () {
      this.step = "--";
      this.userTurn = false;
      this.state = "Start";
      this.score = 0;
      this.isPlaying = false;
      this.pattern = [];
      this.index = 0;
    },
    showPattern: function () {
      var self = this;
      var i = 0;
      if (this.isHardModeOn) {timer = setInterval(function () {
        if (i >= self.pattern.length) {
          self.stopInterval();
        }
        self.clickEffect(self.pattern[i]);
        i++;
      }, 250);}
      else {timer = setInterval(function () {
        if (i >= self.pattern.length) {
          self.stopInterval();
        }
        self.clickEffect(self.pattern[i]);
        i++;
      }, 700);}
    },
    stopInterval: function () {
      clearInterval(timer);
    },
    changeMode: function () {
      if (this.state !== "Start") {
        return;
      }
      if (this.isHardModeOn) {
        this.isHardModeOn = false;
      } else {
        this.isHardModeOn = true;
      }
    },
    clickEffect: function (boxNum) {
      var self = this;
      switch (boxNum) {
        case 1:
          this.isBoxOneActive = true;
          setTimeout(function () {
            self.isBoxOneActive = false;
          }, 100);
          break;
        case 2:
          this.isBoxTwoActive = true;
          setTimeout(function () {
            self.isBoxTwoActive = false;
          }, 100);
          break;
        case 3:
          this.isBoxThreeActive = true;
          setTimeout(function () {
            self.isBoxThreeActive = false;
          }, 100);
          break;
        case 4:
          this.isBoxFourActive = true;
          setTimeout(function () {
            self.isBoxFourActive = false;
          }, 100);
          break;
      }
      return;
    },
  },
};
</script>

<style>
html {
  margin: 0px;
  padding: 0px;
}

#app {
  display: flex;
  justify-content: center;
  align-items: center;
}

.game-box {
  position: absolute;
  top: 150px;
}

.controls-box {
  position: absolute;
  top: 70px;
  left: 60px;
  z-index: 500;
  width: 170px;
  height: 170px;
  border: 10px solid #323232;
  border-radius: 100%;
  background-color: grey;
  padding: 0 auto;
  text-align: center;
}

.box {
  display: inline-block;
  width: 150px;
  height: 150px;
  font-size: 64px;
  text-align: center;
  border: 10px solid #323232;
  box-shadow: 5px 10px 10px #323232;
  margin-left: -10px;
  margin-bottom: -10px;
  cursor: pointer;
}

.box1 {
  background-color: #e84545;
  border-radius: 100% 0 0 0;
}

.box2 {
  background-color: #3f72af;
  border-radius: 0 100% 0 0;
}

.box3 {
  background-color: #11d3bc;
  border-radius: 0 0 0 100%;
}

.box4 {
  background-color: #ffd460;
  border-radius: 0 0 100% 0;
}

.active {
  background-color: hsl(50, 50%, 10%);
}

.start-button {
  display: inline-block;
  width: 40px;
  height: 40px;
  border-radius: 100%;
  border: 5px solid #323232;
  box-shadow: 5px 5px 5px #323232;
  z-index: 800;
  background-color: #07d807;
  text-align: center;
  font-weight: 900;
  font-size: 22px;
  margin-top: 15%;
  margin-left: 10px;
  cursor: pointer;
}
.hard-mode-toggle {
  display: block;
  width: 50px;
  height: 20px;
  border-radius: 5px;
  border: 3px solid #323232;
  box-shadow: 2px 2px 2px #323232;
  z-index: 800;
  background-color: orange;
  text-align: center;
  font-weight: 500;
  margin-top: 10%;
  margin-left: 50px;
  padding: 2px 5px;
  cursor: pointer;
}
.counter {
  display: inline-block;
  width: 50px;
  height: 30px;
  border: 5px solid #323232;
  border-radius: 10px;
  z-index: 800;
  margin-left: 10px;
  text-align: center;
  font-weight: 900;
  font-family: monospace;
  font-size: 22px;
  color: red;
  background-color: maroon;
}
.red {
  background-color: red;
}

@media only screen and (max-width: 670px) {
  h1 {
    font-size: 36px;
  }
  .game-box {
    top: 100px;
  }
  .box {
    width: 120px;
    height: 120px;
  }
  .controls-box {
    width: 110px;
    height: 110px;
  }
  .counter,
  .start-button {
    width: 30px;
    height: 30px;
    margin-top: 10%;
    margin-left: 8px;
    font-size: 18px;
  }
  .hard-mode-toggle {
    display: block;
    width: 45px;
    height: 15px;
    border-radius: 5px;
    border: 2px solid #323232;
    box-shadow: 2px 2px 2px #323232;
    z-index: 800;
    background-color: orange;
    text-align: center;
    font-weight: 500;
    margin-top: 5%;
    margin-left: 30px;
    padding: 1px 3px;
    cursor: pointer;
  }
}
</style>
