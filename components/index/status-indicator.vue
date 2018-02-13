<template>
  <span class='status' :class='{ red: !status, green: status }'>
  </span>
</template>

<script>
  export default {
    props: {
      status: Boolean
    }
  }
</script>

<style>
  .status {
    display: inline-block;
    border-radius: 50%;
    height: 10px;
    width: 10px;
  }

  .red {
    background: red;
  }

  .green {
    background: green;
  }
</style>
