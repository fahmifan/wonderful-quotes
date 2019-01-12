<template>
  <main class="uk-container uk-padding-small">
    <header>
      <h1>Wonderful Quotes</h1>
    </header>
    <div v-show="isQuoteMax" class="uk-alert-danger" uk-alert>
      <a class="uk-alert-close" uk-close></a>
      <p>Max quotes are reached, please delete some quote before add new one!</p>
    </div>
    <progress v-if="!isQuoteMax" class="uk-progress" :value="quotes.length" :max="maxQuotes"></progress>
    <progress v-else class="uk-progress uk-progress-danger" :value="quotes.length" :max="maxQuotes"></progress>

    <p class="uk-text-center">{{ quotes.length }}/{{ maxQuotes }}</p>
    <form>
      <fieldset class="uk-fieldset">
        <legend class="uk-legend">Quote</legend>

        <div class="uk-margin-small">
          <textarea name="quote" rows="5" class="uk-textarea"
            v-model="newQuote"></textarea>
        </div>

        <button class="uk-button uk-button-primary"
          v-if="!isQuoteMax"
          @click.prevent="addQuote">Add</button>
        
        <button class="uk-button uk-button-primary"
          v-else
          @click.prevent="addQuote"
          disabled>Add</button>


      </fieldset>
    </form>
    <div uk-grid>
      <div v-for="(quote, index) in quotes"
          :key="index">
        <quote-item
          :id="index"
          :quote="quote"></quote-item>
        </div>
      </div>
  </main>
</template>

<script>
import QuoteItem from "./QuoteItem";
import { eventBus } from "../../main.js";

export default {
  components: {
    quoteItem: QuoteItem,
  },
  data() {
    return {
      quotes: [],
      newQuote: '',
      maxQuotes: 3,
      isQuoteMax: false,
    }
  },
  methods: {
    addQuote() {
      if(this.quotes.length >= this.maxQuotes) {
        this.isQuoteMax = true
        return
      }

      this.quotes.push(this.newQuote)
      this.newQuote = ''
    }
  },
  created() {
    eventBus.$on('quoteDeleted', (quoteIdx) => {
      this.quotes.splice(quoteIdx, 1)
    })
  },
  updated() {
    this.isQuoteMax = this.quotes.length >= this.maxQuotes
  },
}
</script>

<style lang="scss" scoped>
  form {
    width: 500px;
    margin: auto;
    margin-bottom: 16px;
  }

  .uk-progress-danger { // created class for customisation
    &::-webkit-progress-value { background-color: #F6696E; } // those are the overflowing colors
    &::-ms-fill { background-color: #F6696E; }
  }
</style>
