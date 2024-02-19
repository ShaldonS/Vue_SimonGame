<template>
  <div class="game">
    <h2>Simon Game</h2>
    <span>Выберите уровень: </span>
    <select v-model="level" @change="onChangeLevel($event)">
      <option value="Easy" selected="selected">Easy</option>
      <option value="Medium">Medium</option>
      <option value="Hard">Hard</option>
    </select>
    <div class="circle">
      <div class="btnsContainer">
        <button @click="checkAnswer(0)" id="left-top-btn" :class="{'active': leftTopActive ? '.active' : ''}"></button>
        <button @click="checkAnswer(1)" id="right-top-btn" :class="{'active': rightTopActive ? '.active' : ''}"></button>
        <button @click="checkAnswer(2)" id="right-bottom-btn" :class="{'active': leftBottomActive ? '.active' : ''}"></button>
        <button @click="checkAnswer(3)" id="left-bottom-btn" :class="{'active': rightBottomActive ? '.active' : ''}"></button>
      </div>
      <div class="innerCircle circle">
        <button id="start-btn" @click="startGame()">{{startBtnMsg}}</button>
      </div>
    </div>
  </div>
</template>

<script>
const timer = ms => new Promise(res => setTimeout(res, ms))
export default {
  name: 'SimonGame',
  data() {
    return {
      startBtnMsg: 'Начать',
      gameStarted: false,
      count: 0,
      delay: 1.5,
      level: "Easy",
      leftTopActive: false,
      rightTopActive: false,
      leftBottomActive: false,
      rightBottomActive: false,
      answer: [],
      lastAnswerPos: 0,
    }
  },
  methods: {
    startGame() {
      this.startBtnMsg='Начать'
      this.gameStarted=!this.gameStarted;
      this.count=0;
      this.delay=1.5;
      this.answer=[];
      this.lastAnswerPos=0;

      this.nextStep();
    },
    async nextStep() {
      this.count++;
      let cnt = this.count;
      this.startBtnMsg=cnt;
      this.lastAnswerPos=0;
      
      this.answer=[];
      for(let i=0; i<cnt; ++i) {
          let randNum =  Math.floor(Math.random() * 4);
          this.answer.push(randNum);

          switch(randNum) {
            case 0:
              this.leftTopActive=true;
              break;
            case 1:
              this.rightTopActive=true;
              break;
            case 2:
              this.leftBottomActive=true;
              break;
            case 3:
              this.rightBottomActive=true;
              break;
          }
          setTimeout(() => {
            switch(randNum) {
              case 0:
                this.leftTopActive=false;
                break;
              case 1:
                this.rightTopActive=false;
                break;
              case 2:
                this.leftBottomActive=false;
                break;
              case 3:
                this.rightBottomActive=false;
                break;
            }
          }, 400)

          await timer(1000*this.delay);
      }
    },
    async checkAnswer(btnNum) {
      switch(btnNum) {
        case 0:
          this.leftTopActive=true;
          break;
        case 1:
          this.rightTopActive=true;
          break;
        case 2:
          this.leftBottomActive=true;
          break;
        case 3:
          this.rightBottomActive=true;
          break;
      }
      await timer(400);
      switch(btnNum) {
        case 0:
          this.leftTopActive=false;
          break;
        case 1:
          this.rightTopActive=false;
          break;
        case 2:
          this.leftBottomActive=false;
          break;
        case 3:
          this.rightBottomActive=false;
          break;
      }
      if(btnNum==this.answer[this.lastAnswerPos]) {
        if(++this.lastAnswerPos == this.answer.length)
          this.nextStep();
      }
      else {
        alert("Вы проиграли");
        this.startBtnMsg='Начать';
      }
    },
    onChangeLevel(event) {
      let val = event.target.value;
      switch(val) {
        case 'Easy':
          this.delay=1.5
          break;
        case 'Medium':
          this.delay=1
          break;
        case 'Hard':
          this.delay=0.4
          break;
      }
      console.log(event.target.value)
    }
  }
}
</script>

<style scoped>
.circle {
  position: relative;
  border-radius: 50%;
  margin: auto;
  width: 45vmin;
  height: 45vmin;
  background: rgba(20, 20, 58, 0.6);
  overflow: hidden;
  border: 15px solid rgba(20, 20, 58, 0.6);
}
.innerCircle {
  position: absolute;
  top: 29%;
  left: 28%;
  width: 20vmin;
  height: 20vmin;
  padding: 0;
  background: rgba(20, 20, 58, 1.0);
  border: 1px rgba(20, 20, 58, 0.6);
}
.innerCircle button{
  width: 100%;
  height: 100%;
  cursor: pointer;
  background: none;
  color: white;
  font-size: 1.6em;
  border-radius: 50%;
  border: none
}
.innerCircle button:hover{
  background: rgb(22, 127, 175);
  color: white;
}
.btnsContainer {
  position: relative;
}
.btnsContainer button {
  display: inline-block;
  width: 22vmin;
  height: 22vmin;
  opacity: 0.3;
  border: none;
  /*border: 20px solid rgba(20, 20, 58, 1);*/
}
#left-top-btn {
  background: rgb(66, 211, 66);
  float: left;
}
#right-top-btn {
  background: rgb(59, 48, 214);
  float: right;
}
#right-bottom-btn {
  background: rgb(211, 66, 199);
  float: right;
  margin-top: 10px;
}
#left-bottom-btn {
  background: rgb(189, 211, 66);
  float: left;
  margin-top: 10px;
}
.active {
  opacity: 1 !important;
}
</style>
