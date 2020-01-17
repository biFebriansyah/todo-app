<template>
  <div>
    <label for="checkAll">
      CheckAll
      <input
        type="checkbox"
        name="checkAll"
        id="checkAll"
        :checked="!anyRemaning"
        @change="changeAll"
      />
    </label>
  </div>
</template>

<script>
export default {
  name: "anyremaning",
  props: {
    anyRemaning: {
      type: Boolean,
      required: true
    }
  },
  methods: {
    changeAll() {
      this.$eventBus.$emit("check-all", this.anyRemaning);
    }
  }
};
</script>

<style lang="scss" scoped>
</style>