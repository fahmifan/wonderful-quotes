<template>
  <div class="quote uk-card uk-card-default uk-card-body uk-card-hover"
    @mouseover="isShow = true" @mouseout="isShow = false">
    <p class="uk-card-title">{{ quote }}</p>
    <span uk-icon="trash" class="uk-margin-small-right icon"
      v-show="isShow" 
      @click="deleteQuote()"></span>
  </div>
</template>

<script>
import { eventBus } from "../../main.js" 
export default {
  props: ['quote', 'id'],
  data() {
    return {
      isShow: false,
    }
  },
  methods: {
    deleteQuote() {
      eventBus.deleteQuote(this.id)
    }
  }
}
</script>

<style lang="scss" scoped>
.quote {
  cursor: pointer;
  p {
    font-family: 'Vibur', cursive;
    font-size: 32px;
  }
  &:hover {
    background-color: #1E87F0;
    p {
      color: #fff;
    }

    .icon {
      fill: #fff;
      color: #fff;

      &:hover {
        opacity: 0.7;
      }
    }
  }
}
</style>
