<template>
  <span>
    <h4>
      {{ outOf5 }}
      <i class='fa fa-star' />
    </h4>
    {{ review }} out of 100
  </span>
</template>

<script>
  export default {
    props: {
      review: Number
    },
    computed: {
      outOf5() {
        return (this.review / 100) * 5
      }
    }
  }
</script>
