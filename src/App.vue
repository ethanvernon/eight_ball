<template>
  <div id="app">
    <h1>Magic 8-Ball</h1>
    <img alt="Vue logo" src="../public/magicBallStart.png" />
    <QueryResult :result="result" />
    <InputBox />
    <SubmitButton :disabled="disabled" :loading="loading" />
  </div>
</template>

<script>
import SubmitButton from "./components/SubmitButton.vue";
import InputBox from "./components/InputBox.vue";
import QueryResult from "./components/QueryResult.vue";
import EventBus from "./event-bus";
import axios from "axios";

export default {
  name: "App",
  components: {
    SubmitButton,
    InputBox,
    QueryResult
  },
  data() {
    return {
      inputBoxValue: "",
      result: "",
      loading: false,
      disabled: false
    };
  },
  mounted() {
    EventBus.$on("input-changed", input => {
      this.inputBoxValue = input;
    });

    EventBus.$on("submit-clicked", () => {
      this.handleSubmit();
    });
  },
  methods: {
    handleSubmit() {
      if (this.endsInQuestionMark()) {
        this.loading = true;
        this.disabled = true;
        this.makeRequest();
      }
    },
    makeRequest() {
      let params = encodeURIComponent(this.inputBoxValue);
      let uri = "https://8ball.delegator.com/magic/JSON/" + params;

      axios
        .get(uri)
        .then(result => {
          console.log("result:", result);
          this.loading = false;
          this.disabled = false;

          this.result = result.data.magic.answer;
        })
        .catch(err => {
          console.log("err:", err);
          this.apiError();
        });
    },
    endsInQuestionMark() {
      if (
        this.inputBoxValue[this.inputBoxValue.length - 1] == "?" &&
        this.inputBoxValue.length > 0
      ) {
        return true;
      }

      if (!this.inputBoxValue.length > 0) {
        this.warning();
      }
    },
    warning() {
      this.$buefy.snackbar.open({
        message: "Please input a question",
        type: "is-warning",
        position: "is-top"
      });
    },
    apiError() {
      this.$buefy.snackbar.open({
        message: "There was an error, please try again",
        type: "is-danger",
        position: "is-top"
      });
      this.loading = false;
      this.disabled = false;
    }
  }
};
</script>

<style lang="scss">
#app {
  display: flex;
  align-items: center;
  flex-direction: column;
  font-family: "Josefin Sans", sans-serif;
  color: #000;

  h1 {
    font-size: 60px;
  }

  p {
    font-size: 20px;
  }

  img {
    max-height: 50vh;
  }
}
</style>
