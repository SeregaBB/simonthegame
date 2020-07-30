<template>
<div class="game">
    <div class="wrapper">
      <div class="center" @click="play">{{turn}}</div>
      <div id="btn_1" name="1" @click="playerRepeat" :class="[{toucheble: isToucheble}, 'top-left']"></div>
      <div id="btn_2" name="2" @click="playerRepeat" :class="[{toucheble: isToucheble}, 'top-right']"></div>
      <div id="btn_3" name="3" @click="playerRepeat" :class="[{toucheble: isToucheble}, 'bottom-left']"></div>
      <div id="btn_4" name="4" @click="playerRepeat" :class="[{toucheble: isToucheble}, 'bottom-right']"></div>
    </div>
    <form name="settings">
      <h2>Уровень сложности</h2>
      <div class="label-group">
        <label for="radio_one">Лёгкий</label>
        <label for="radio_two">Нормальный</label>
        <label for="radio_three">Сложный</label>
      </div>
      <div class="input-group">
        <input type="radio" id="radio_one" name="level" value="easy" v-model="level" />
        <input type="radio" id="radio_two" name="level" value="medium" v-model="level"/>
        <input type="radio" id="radio_three" name="level" value="hard" v-model="level"/>
      </div>
    </form>
</div>
</template>

<script>
const levels = {
  easy: 1500,
  medium: 1000,
  hard: 400
}
let counter = 0;
let waitToRepeat = false;
let arr = [1];
let playersClick = 0;



export default {
  name: "Game",
  data: ()=>({
    level: 'easy', 
    turn: 'Start Simon the game', 
    endScore: 0, 
    disabledButton: false, 
    isToucheble: false
  }),
  methods: {





  highlight(btn) {
     btn.classList.add('highlighted');
     setTimeout(() => {
       btn.classList.remove('highlighted');
     }, 350);
  },






  playerRepeat(event){
    if(!waitToRepeat) return false;
    const buttonNum = event.target.getAttribute('name');
    console.log(waitToRepeat, Number(buttonNum), playersClick, arr[playersClick]);
   const audio = new Audio(); // Создаём новый элемент Audio
	 audio.src = require(`../assets/${buttonNum}.mp3`) // Указываем путь к звуку "клика"
	 audio.play()
    if(waitToRepeat){
      if(arr[playersClick] !== Number(buttonNum))  {
        this.endScore = arr.length;
        this.turn = `Game Over! You're score ${arr.length} Let's try again!`;
        
        arr = [this.getRandomButton()];
        waitToRepeat = false;
        return;
      } 
      playersClick = playersClick + 1;
       if(arr.length === playersClick) {
         arr.push(this.getRandomButton());
         playersClick = 0; 
         setTimeout(()=>{
           this.play();
         }, 2000)
         
       }
      }
      
    },







getRandomButton() {
  const min = 1
  const max = 5
  return Math.floor(Math.random() * (max - min)) + min;
},







    play(){ 
      
      this.turn = 'My turn!';
      this.isToucheble = false;
      let timer = setInterval(() => {
           const button = document.querySelector(`#btn_${arr[counter]}`);
           const buttonNum = button.getAttribute('name');
              this.highlight(button);
              const audio = new Audio(); // Создаём новый элемент Audio
	 audio.src = require(`../assets/${buttonNum}.mp3`) // Указываем путь к звуку "клика"
	 audio.play()
              counter = counter + 1; 
            if (counter === arr.length) {
              clearInterval(timer);
              waitToRepeat = true;
              counter = 0
              setTimeout(()=>{
                 this.turn = 'You turn'
                 this.isToucheble = true;
              }, 1500)
              console.log(this, arr);
            }
            
       }, levels[this.level]);
       
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

.label-group, .input-group {
  display: flex;
  justify-content: space-between;
}

label, input {
  width: 100px;
  margin: 0;
  text-align: center;
}

h2 {
  text-align: center;
}

.wrapper {
  width: 500px;
  height: 500px;
  position: relative;
  display: block;
  border-radius: 100px;
  overflow: hidden;
  cursor: default;
  z-index: 0;

  div {
    cursor: pointer;
    filter: brightness(0.8);
    z-index: 998;

    &.toucheble:hover {
      filter: drop-shadow(0px 0px 20px rgb(117, 117, 117));
      filter: brightness(1);
    }

    &.toucheble:active {
      transition: filter .4s;
      filter: brightness(0.9);
    }
  }
}

div.highlighted {
  filter: brightness(1);
}

.top-left {
  width: 0;
  height: 0;
  border-top: 250px solid #03ffe2;
  border-right: 250px solid transparent;
  position: absolute;
}

.top-right {
  width: 0;
  height: 0;
  border-top: 250px solid #ff8c5e;
  border-left: 250px solid transparent;
  position: absolute;
  top: 0;
  right: 0;
}

.bottom-left {
  width: 0;
  height: 0;
  border-bottom: 250px solid #fffd82;
  border-right: 250px solid transparent;
  position: absolute;
  bottom: 0;
  left: 0;
}

.bottom-right {
  width: 0;
  height: 0;
  border-bottom: 250px solid #7f62ff;
  border-left: 250px solid transparent;
  position: absolute;
  bottom: 0;
  right: 0;
}

div.center {
  position: absolute;
    width: 200px;
    height: 200px;
    top: 50%;
    left: 50%;
    margin-left: -100px;
    margin-top: -100px;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 100px;
    font-weight: bolder;
    z-index: 999;
    border: 4px solid #ccc;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
    box-sizing: border-box;
    font-size: 20px;
    padding: 20px;
    text-align: center;
    &:hover{
      transition: .4s;
      background: rgb(187, 187, 187);
    }
}
</style>
