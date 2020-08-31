<template>
  <button
    type="button"
    :id="lamp_id"
    @click="highlight"
    :class="`button ${lamp_position}`"
  />
</template>

<script>
export default {
  name: "Lamp",
  data: () => ({}),
  props: {
    lamp_id: Number,
    lamp_position: String
  },
  methods: {
    highlight() {
      console.log(this.lamp_id);
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
  &:hover {
    filter: brightness(1);
  }
  &:active {
    transition: filter 0.1s;
    filter: brightness(0.9);
  }
}
</style>
