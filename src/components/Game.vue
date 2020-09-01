<template>
  <div class="game">
    <div class="note">
      <p>Level {{ currentLevel }}</p>
      <p>Current Stage {{ currentStage }}</p>
      <p>{{ loose ? 'You loose. Click "start" to new game' : "" }}</p>
    </div>
    <div class="wrapper">
      <Lamp
        lamp_position="top-left"
        :lamp_id="1"
        :allow_clicks="allowClick"
        @onPushLamp="checkUserClick"
      />
      <Lamp
        lamp_position="top-right"
        :lamp_id="2"
        :allow_clicks="allowClick"
        @onPushLamp="checkUserClick"
      />
      <Lamp
        lamp_position="bottom-left"
        :lamp_id="3"
        :allow_clicks="allowClick"
        @onPushLamp="checkUserClick"
      />
      <Lamp
        lamp_position="bottom-right"
        :lamp_id="4"
        :allow_clicks="allowClick"
        @onPushLamp="checkUserClick"
      />
      <div class="center">
        <div class="button-group">
          <label for="start">Start</label>
          <button
            class="start"
            name="start"
            @click="addButtonsToPush"
            :disabled="gameIsStarted"
          />
        </div>
        <div class="button-group">
          <label for="radio-group">Level</label>
          <div name="radio-group" class="radio-group">
            <p>
              <input
                type="radio"
                name="level"
                id="1"
                value="1"
                v-model="currentLevel"
              />
              1
            </p>
            <p>
              <input
                type="radio"
                name="level"
                id="2"
                value="2"
                v-model="currentLevel"
              />
              2
            </p>
            <p>
              <input
                type="radio"
                name="level"
                id="3"
                value="3"
                v-model="currentLevel"
              />
              3
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const levels = {
  1: 1500,
  2: 1000,
  3: 400
};
const defaultData = () => ({
  buttons: [],
  userStep: 0,
  currentLevel: 1,
  currentStage: 1,
  allowClick: false,
  gameIsStarted: false,
  loose: false
});

import Lamp from "../components/Lamp.vue";

export default {
  name: "Game",
  data: () => ({
    buttons: [],
    userStep: 0,
    currentLevel: 1,
    currentStage: 1,
    allowClick: false,
    gameIsStarted: false,
    loose: false
  }),
  methods: {
    getRandomButton() {
      const min = 1;
      const max = 5;
      return Math.floor(Math.random() * (max - min)) + min;
    },
    addButtonsToPush() {
      if (this.loose) {
        const currLevel = this.currentLevel;
        Object.assign(this.$data, defaultData());
        this.currentLevel = currLevel;
      }
      this.gameIsStarted = true;
      this.allowClick = false;
      const randomLampId = this.getRandomButton();
      this.buttons.push(randomLampId);
      this.showWhatNeedToPush();
    },
    showWhatNeedToPush() {
      const interval = levels[this.currentLevel];
      let step = 0;
      let intervalFunction = setInterval(() => {
        let lampId = this.buttons[step] - 1;
        this.$children[lampId].highlight(interval);
        step += 1;
        if (step >= this.buttons.length) {
          clearInterval(intervalFunction);
          this.allowClick = true;
        }
      }, interval);
    },
    checkUserClick(data) {
      const buttonForCompare = this.buttons[this.userStep];
      const usersClick = data.id;
      if (usersClick === buttonForCompare) {
        this.userStep += 1;
        if (this.userStep === this.buttons.length) {
          this.userStep = 0;
          this.addButtonsToPush();
        }
        return;
      }
      if (usersClick !== buttonForCompare) {
        this.gameIsStarted = false;
        this.allowClick = false;
        this.loose = true;
        return;
      }
    }
  },
  components: {
    Lamp
  }
};
</script>

<style scoped lang="scss">
$lamp_border: 10px solid black;
@font-face {
  font-family: "FunnyFont";
  src: url("../assets/fonts/1.ttf") format("truetype");
}

.note {
  font-family: "FunnyFont";
  font-size: 20px;
  width: 200px;
  height: 200px;
  position: absolute;
  top: 20%;
  right: 10%;
  transform: rotate(-15deg);
  background: url("../assets/note.png");
  background-size: cover;
  filter: brightness(0.75) drop-shadow(5px 5px 3px#00000080);
  padding: 50px;
  background-repeat: no-repeat;
}
.wrapper {
  width: 500px;
  height: 500px;
  position: relative;
  border-radius: 1000px;
  overflow: hidden;
  box-shadow: 11px 7px 9px 7px #00000080;
  border: 20px solid transparent;
  background-image: url("../assets/blackplastic.jpg");
  background-size: 200%;
}
.center {
  width: 200px;
  height: 200px;
  border: 20px black solid;
  position: absolute;
  top: 130px;
  left: 130px;
  border-radius: 1000px;
  background: url("../assets/holes.jpg");
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.button-group {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  font-weight: bolder;
  margin: 0 0 10px;
}
.radio-group {
  display: flex;
  flex-direction: row;
  color: #fff;
  p {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin: 5px;
    input {
      margin: 0;
      width: 25px;
      height: 25px;
      border-radius: 1000px;
      outline: none;
      border: 5px solid black;
      box-sizing: border-box;
      background-image: url("../assets/plastic.png");
      background-color: rgba(255, 253, 130, 0.9);
      background-blend-mode: color-burn;
      background-size: cover;
      box-shadow: 3px 2px 2px #4a4a4a8c;
      appearance: none;
      cursor: pointer;
      filter: brightness(0.8);
      &:checked {
        background-color: rgba(127, 98, 255, 0.9);
      }
      &:hover {
        filter: brightness(1);
      }
      &:active {
        transition: filter 0.1s;
        filter: brightness(0.9);
      }
    }
  }
}
label {
  color: green;
  font-size: 40px;
  text-shadow: #fff 0 0 0px;
}
.start {
  width: 25px;
  height: 25px;
  border-radius: 1000px;
  outline: none;
  background: rgba(255, 140, 94, 1);
  border: 5px solid black;
  box-sizing: border-box;
  background-image: url("../assets/plastic.png");
  background-blend-mode: color-burn;
  background-size: cover;
  box-shadow: 3px 2px 2px #4a4a4a8c;
  cursor: pointer;
  filter: brightness(0.8);
  &:hover {
    filter: brightness(1);
  }
  &:active {
    transition: filter 0.1s;
    filter: brightness(0.9);
  }
}
.top-left {
  background-color: rgba(3, 255, 226, 0.9);
  border-bottom: $lamp_border;
  border-right: $lamp_border;
}

.top-right {
  background-color: rgba(255, 140, 94, 0.9);
  border-bottom: $lamp_border;
  border-left: $lamp_border;
  top: 0;
  right: 0;
}

.bottom-left {
  background-color: rgba(255, 253, 130, 0.9);
  border-top: $lamp_border;
  border-right: $lamp_border;
  bottom: 0;
  left: 0;
}

.bottom-right {
  background-color: rgba(127, 98, 255, 0.9);
  border-top: $lamp_border;
  border-left: $lamp_border;
  bottom: 0;
  right: 0;
}
</style>
