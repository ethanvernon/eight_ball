<template>
  <section>
    <b-field>
      <b-input
        id="input-box"
        placeholder="Please ask a question"
        validation-message="Input must end in question mark"
        v-model="inputBoxValue"
        @input="handleChange()"
        pattern="[^.!?]+\?"
      ></b-input>
    </b-field>
  </section>
</template>

<script>
import EventBus from "../event-bus";

export default {
  name: "InputBox",
  data() {
    return {
      inputBoxValue: ""
    };
  },
  methods: {
    handleChange() {
      EventBus.$emit("input-changed", this.inputBoxValue);
    }
  }
};
</script>

<style></style>
