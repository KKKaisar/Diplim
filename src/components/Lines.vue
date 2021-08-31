<template>
  <div>{{ "&nbsp;".repeat(spaces) }}{{ line }}</div>
</template>

<script>
export default {
  props: ["item", "index"],
  mounted() {
    console.log("DVDTXT");
    // this.$parent.divideText(this.item, this.index, this.$parent.lines);
  },
};
</script>

<style lang="scss" scoped>
</style>