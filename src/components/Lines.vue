<template>
  <div
    tabindex="0"
    class="dragElement dragable"
    @click="$emit('activeitem', 'drag' + index)"
    @keydown.down="move(index, 'd')"
    @keydown.up="move(index, 'u')"
    @keydown.left="move(index, 'l')"
    @keydown.right="move(index, 'r')"
    :class="'drag' + index"
  >
    <div
      class="drag_zone"
      onmouseup=""
      onclick="vm.dragFunc(this.parentElement,event)"
    ></div>
    <div
      :style="{ border: bdr }"
      v-for="(line, i) in lines[index]"
      :key="i"
      :class="'drag' + index + '' + i"
      @click="$emit('activechild', 'drag' + index + '' + i)"
    >
      {{ line }}
    </div>
  </div>
</template>

<script>
export default {
  props: ["item", "index", "bdr", "lines", "spaces"],
  mounted() {
    console.log("DVDTXT");
    // this.$parent.divideText(
    //   this.item,
    //   this.index,
    //   this.lines,
    //   "drag" + this.index
    // );
  },
};
</script>

<style lang="scss" scoped>
</style>