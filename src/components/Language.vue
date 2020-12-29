<template>
  <div class="language-container">

    <div>
      <button class="language-button" @click="onClickCountryEs">
        <img :src="ArgIcon" alt="argentina" />
      </button>

      <button class="language-button" @click="onClickCountryEn">
        <img :src="UkIcon" alt="uk" />
      </button>
    </div>

  </div>
</template>

<script>
// Assets
import ArgIcon from '../assets/arg.png'
import UkIcon from '../assets/uk.png'

export default {
  data() {
    return {
      ArgIcon,
      UkIcon
    }
  },
  methods: {
    onClickCountryEs() {
      this.$emit('handle-click-language', 'es')
    },
    onClickCountryEn() {
      this.$emit('handle-click-language', 'en')
    }
  }
}
</script>

<style scoped>
.language-container {
  display: flex;
  justify-content: space-around;
  margin: 20px 0;
}

.language-button {
  margin-right: 10px;
}

img {
  width: 30px;
}
</style>