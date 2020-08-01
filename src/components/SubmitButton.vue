<template>
  <div class="submit-button">
    <b-button
      :loading="loading"
      :disabled="disabled"
      type="is-info"
      @click="handleSubmit()"
      >Submit button</b-button
    >
  </div>
</template>

<script>
import EventBus from "../event-bus";

export default {
  name: "SubmitButton",
  props: ["loading", "disabled"],
  methods: {
    handleSubmit() {
      EventBus.$emit("submit-clicked");
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss" scoped>
div {
  margin: 2rem;
}
</style>
