<template>
  <div class="container">
    <h1>Quotes Added</h1>
    <div class="progress">
      <div v-bind:style="{width: width + '0%'}" class="progress-bar progress-bar-striped progress-bar-animated" role="progressbar" aria-valuemin="0" aria-valuemax="10">{{ appQuotes.length }} / 10</div>
    </div>
    <InputForm></InputForm>
    <QuoteContainer></QuoteContainer>
  </div>

</template>

<script>
  import InputForm from "./components/InputForm.vue"
  import QuoteContainer from "./components/QuoteContainer.vue"
  import { EventBus } from "./main.js"

  export default {
    data: function() {
      return {
        progressClasses: [],
        appQuotes: [],
        receivedQuote: "", 
        width: 0,
        alertVisible: false
      }
    },
    methods: {
      setUpQuote(payload) {
        this.receivedQuote = payload;

        // Emit new array version to EventBus
        EventBus.$emit("quotesArrayNew", this.appQuotes);

        if (this.appQuotes.length < 10) {
          this.width = this.appQuotes.length + 1;
          this.appQuotes.push(payload);
        } else {
          console.log("You are out of space for quotes!");
          this.alertVisible = true; 
          this.emitAlert();
        }
      },
      emitAlert() {

        // Emit truthy alert to EventBus
        EventBus.$emit("alertOn", this.alertVisible);
      },
      refreshTopLevelArray(newArray) {
        this.appQuotes = newArray;
      }
    },
    components: {
      InputForm: InputForm,
      QuoteContainer: QuoteContainer
    },
    mounted () {
      EventBus.$on("quoteSent", (payload) => {
        this.setUpQuote(payload);
      });
      EventBus.$on("ArrayDeletion", (arrayPayload) => {
        this.refreshTopLevelArray(arrayPayload);
      });
      EventBus.$on("minusOne", (payload) => {
        this.width = this.width - payload; 
      });
    }
  }
  
</script>

<style scoped>

  .container {
    margin-top: 30px;
  }

</style>