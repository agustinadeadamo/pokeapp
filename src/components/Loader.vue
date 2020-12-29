<template>
  <div>
    <div class="loader">
    </div>
  </div>
</template>

<style scoped>
  .loader {
    margin: 100px auto;
    border: 1px solid #FCBEBE;
    border-top: 1px solid #FF1E56;
    border-radius: 50%;
    width: 70px;
    height: 70px;
    animation: spin 2s linear infinite;
    position: relative;
    z-index: 100;
  }

  @keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }
</style>