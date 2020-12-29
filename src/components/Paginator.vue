<template>
  <div class="paginator">
    <button @click="onClickPrevious" >Previous</button>
    <button @click="onClickNext" >Next</button>
  </div>
</template>

<script>

export default {
  props: {
    previous: {
      type: Boolean,
      default: false
    },
    next: {
      type: Boolean,
      default: false
    }
  },
  methods: {

    /**
     * Function that is executed when clicking on next button
     */
    onClickNext() {
        this.$emit('handle-click-next')
    },

    /**
     * Function that is executed when clicking on previous button
     */
    onClickPrevious() {
        this.$emit('handle-click-previous')
    }
  }
}
</script>

<style scoped>
.paginator {
  margin: 20px 0 20px 0;
  display: flex;
  justify-content: space-around;
}

button {
  margin: 0 10px 0 10px;
  background-color: #EFF3F3;
  padding: 10px 20px;
  border-radius: 5px;
}
</style>