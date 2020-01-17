<template>
  <div>
    <div>{{remining}} items left</div>
  </div>
</template>

<script>
export default {
  name: "remaning",
  props: {
    remining: {
      type: Number,
      required: true
    }
  }
};
</script>

<style lang="scss" scoped>
</style>