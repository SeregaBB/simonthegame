<template>
  <button
    type="button"
    :id="lamp_id"
    @click="clickHandler"
    :class="[{ highlighted: isHighlighted }, lamp_position, 'button']"
    :disabled="!allow_clicks"
  />
</template>

<script>
export default {
  name: "Lamp",
  data: () => ({
    isHighlighted: false
  }),
  props: {
    lamp_id: Number,
    lamp_position: String,
    allow_clicks: Boolean
  },
  methods: {
    clickHandler() {
      const audio = new Audio();
      audio.src = require(`../assets/${this.lamp_id}.mp3`);
      audio.play();
      this.$emit("onPushLamp", {
        id: this.lamp_id
      });
    },
    highlight(timeout) {
      const correctTimout = 100;
      const highlightTimeout = timeout - correctTimout;
      this.isHighlighted = true;
      const audio = new Audio();
      audio.src = require(`../assets/${this.lamp_id}.mp3`);
      audio.play();
      setTimeout(() => {
        this.isHighlighted = false;
      }, highlightTimeout);
    }
  }
};
</script>

<style lang="scss">
.button {
  width: 250px;
  height: 250px;
  margin: 0;
  padding: 0;
  position: absolute;
  border: 0;
  background-image: url("../assets/plastic.png");
  background-size: 300%;
  background-blend-mode: color-burn;
  outline: none;
  cursor: pointer;
  filter: brightness(0.8);
  &:not([disabled]):hover,
  &.highlighted {
    filter: brightness(1);
  }
  &:active {
    transition: filter 0.1s;
    filter: brightness(0.9);
  }
}
</style>
