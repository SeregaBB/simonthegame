<template>
  <div class="game">
    <div class="wrapper">
      <div class="center" @click="play">{{centerText}}</div>
      <div
        id="btn_1"
        name="1"
        @click="playerRepeat"
        :class="[{toucheble: waitToRepeat, highlighted: btn_1}, 'top-left']"
      ></div>
      <div
        id="btn_2"
        name="2"
        @click="playerRepeat"
        :class="[{toucheble: waitToRepeat, highlighted: btn_2}, 'top-right']"
      ></div>
      <div
        id="btn_3"
        name="3"
        @click="playerRepeat"
        :class="[{toucheble: waitToRepeat, highlighted: btn_3}, 'bottom-left']"
      ></div>
      <div
        id="btn_4"
        name="4"
        @click="playerRepeat"
        :class="[{toucheble: waitToRepeat, highlighted: btn_4}, 'bottom-right']"
      ></div>
    </div>
    <form name="settings">
      <span class="stage">Уровень {{score}}</span>
      <h2>Уровень сложности</h2>
      <div class="label-group">
        <label for="radio_one">Лёгкий</label>
        <label for="radio_two">Нормальный</label>
        <label for="radio_three">Сложный</label>
      </div>
      <div class="input-group">
        <input type="radio" id="radio_one" name="level" value="easy" v-model="level" />
        <input type="radio" id="radio_two" name="level" value="medium" v-model="level" />
        <input type="radio" id="radio_three" name="level" value="hard" v-model="level" />
      </div>
    </form>
  </div>
</template>

<script>
const levels = {
  easy: 1500,
  medium: 1000,
  hard: 400
};

const defaultSettings = ()=>({
level: "easy",
    
    endScore: 0,
    score: 1,
    waitToRepeat: false,
    playersClick: 0,
    arr: [1], 
    counter: 0,
    btn_1: false,
    btn_2: false,
    btn_3: false,
    btn_4: false
})



export default {
  name: "Game",
  data: () => ({
    level: "easy",
    centerText: "Начать Simon the game",
    endScore: 0,
    score: 1,
    waitToRepeat: false,
    playersClick: 0,
    arr: [1], //массив кнопок, которые нажимает функция (будет пополняться с каждым новым уровнем)
    counter: 0,
    btn_1: false,
    btn_2: false,
    btn_3: false,
    btn_4: false
  }),
  methods: {
    //метод, который подсвечивает нажатые игроком или функцией кнопки
    highlight(btn) { 
      this[`btn_${btn}`] = true;
      setTimeout(() => {
        this[`btn_${btn}`] = false;
      }, 350);
    },
 
   //метод, который отслеживает нажатие кнопок и сравнивает верно ли нажато
    playerRepeat(event) {
      if (!this.waitToRepeat) return false;
      const buttonNum = event.target.getAttribute("name");
      
      this.highlight(buttonNum); 
      const audio = new Audio(); 
      audio.src = require(`../assets/${buttonNum}.mp3`); 
      audio.play(); 
      if (this.waitToRepeat) { // проверяем, ожидает ли функция нажатий от игрока
        if (this.arr[this.playersClick] !== Number(buttonNum)) { //если не соответсвтует цифре в массиве - конец игры
          this.endScore = this.arr.length;
          this.centerText = `Game Over! Ты дошёл до ${this.arr.length} уровня. Ещё раз?`;
     
          Object.assign(this.$data, defaultSettings());
          this.disableCenterButton = false;
          this.waitToRepeat = false;
          return;
        }
        this.playersClick = this.playersClick + 1; //если соответствует, увеличиваем счётчик нажатий и идём дальше
        if (this.arr.length === this.playersClick) { //если повторили все кнопки - продолжаем игру
          this.arr.push(this.getRandomButton());
          this.playersClick = 0;
          setTimeout(() => {
            this.play();
          }, 2000);
        }
      }
    },

//метод, который возвращает рандомный номер кнопки от 1 до 4 включительно
    getRandomButton() {
      const min = 1;
      const max = 5;
      return Math.floor(Math.random() * (max - min)) + min;
    },

//метод который показывает, что нажимать
    play() {
      
      this.disableCenterButton = true;
      this.centerText = "Запоминай!";
      this.score = this.arr.length;
      this.waitToRepeat = false;
      let timer = setInterval(() => {
        this.highlight(Number(this.arr[this.counter]));
        const audio = new Audio(); // Создаём новый элемент Audio
        audio.src = require(`../assets/${this.arr[this.counter]}.mp3`); // Указываем путь к звуку "клика"
        audio.play();
        this.counter = this.counter + 1;
        if (this.counter === this.arr.length) {
          clearInterval(timer);
          
          this.counter = 0;
          setTimeout(() => {
            this.centerText = "Повторяй!";
            this.waitToRepeat = true;
          }, 1500);
          
        }
      }, levels[this.level]);
    }
  }
};
</script>


<style scoped lang="scss">
.label-group,
.input-group {
  display: flex;
  justify-content: space-between;
}

label,
input {
  width: 100px;
  margin: 0;
  text-align: center;
}

h2 {
  text-align: center;
}
.stage {
  width: 100%;
  text-align: center;
  font-size: 15px;
  font-weight: bold;
  display: block;
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
      transition: filter 0.1s;
      filter: brightness(0.9);
    }
    &.highlighted {
      filter: brightness(1.1);
    }
    &.highlighted:hover {
      filter: brightness(1.1);
    }
  }
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
  &:hover {
    transition: 0.4s;
    background: rgb(187, 187, 187);
  }
}
</style>
