<template>
  <span>
  	{{ name }}
    <h4>
      {{ description }}
    </h4>
  </span>
</template>

<script>
  export default {
    props: {
      name: String,
      description: String
    }
  }
</script>
