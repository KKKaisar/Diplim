<template>
  <div
    class="dragElement dragable"
    @click="$emit('activeitem', 'drag' + index)"
    :class="'drag' + index"
    :style="{ top: index * 100 + 'px' }"
  >
    <div
      class="drag_zone"
      onmouseup=""
      onclick="vm.dragFunc(this.parentElement,event)"
      @click="$emit('activechild', 'drag')"
    ></div>
    <div
      :style="{ border: bdr }"
      style="position: relative"
      v-for="(line, i) in lines[index]"
      :key="i"
      :class="'drag' + index + '' + i"
      @click="$emit('activechild', 'drag' + index + '' + i)"
      tabindex="0"
      @keydown.down="move(index, 'd')"
      @keydown.up="move(index, 'u')"
      @keydown.left="$emit('movel', index, 'l')"
      @keydown.right="$emit('mover', index, 'r')"
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