<template>
  <div>
    <!-- <VueDraggableResizable
      drag-cancel=".cancel"
      :w="40"
      :h="10"
      :y="config['drag' + index] ? config['drag' + index].top : index * 40"
      :x="config['drag' + index] ? config['drag' + index].left : 0"
      @dragging="onDrag"
      @resizing="onResize"
      :parent="true"
      tabindex="0"
      :style="{ border: bdr }"
      :draggable="canceled"
      :resizable="false"
    >
      <div
        style="width: 100%; height: 100%"
        class="dragElement dragable"
        :class="'drag' + index"
        @click="activeItem('drag' + index)"
      >
        <div @click="activeItem('drag' + index)" class="drag_zone"></div>
        <VueDraggableResizable
          v-for="(line, i) in lines"
          :key="i"
          @resizing="divideText(item, i)"
          :w="config['drag' + i] ? config['drag' + i].width : 300"
          style="border: none"
          :y="30 * i"
          :h="20"
          :draggable="!canceled"
          :resizable="!canceled"
        >
          <div
            @click="activeItem('drag' + index)"
            @load="divideText(item, i)"
            :style="{ border: bdr }"
            style="width: 100%"
          >
            {{ line }}
          </div>
        </VueDraggableResizable>
      </div>
    </VueDraggableResizable> -->
    <div
      class="dragElement dragable"
      :class="'drag' + index"
      @click="activeItem('drag' + index)"
    >
      <div
        class="drag_zone"
        style="backround: red; height: 15px"
        @click="dragFunc($event)"
      ></div>
      <VueDragResize
        v-for="(line, i) in lines"
        :key="i"
        @resizing="divideText(item, i)"
        style="border: none"
        :y="30 * (i + 1)"
        :h="0"
        :isDraggable="!canceled"
        :isResizable="!canceled"
      >
        <div
          @click="activeItem('drag' + index)"
          :class="'drag' + index + i"
          :style="{ border: bdr }"
        >
          {{ line }}
        </div>
      </VueDragResize>
    </div>
  </div>
</template>

<script>
// import VueDraggableResizable from "vue-draggable-resizable";
import VueDragResize from "vue-drag-resize";
// import $ from "jquery";
// optionally import default styles
export default {
  data() {
    return {
      spaces: 0,
      active: "",
      lines: ["a  "],
      x: 0,
      y: 0,
      width: 0,
      height: 0,
    };
  },
  mounted() {
    // var partOne = $("#mainpipe").text().substring(0, 1);
    // var partTwo = $("#mainpipe")
    //   .text()
    //   .substring(1, $("#mainpipe").text().length - 1);
    // $("#mainpipe").html(partOne);
    // $("#runoff").html(partTwo);
    this.config["drag" + this.index] = document.querySelector(
      ".drag" + this.index
    ).style;
    this.config["drag" + this.index] = document.querySelector(
      ".drag" + this.index
    ).style;
    this.config["drag" + this.index] = document.querySelector(
      ".drag" + this.index
    ).style;
    for (
      let i = 0;
      i < document.querySelector(".drag" + this.index).children.length;
      i++
    ) {
      this.config["drag" + this.index + i] = document.querySelector(
        ".drag" + this.index + i
      ).style;
      console.log(i);
    }
    this.config = { ...this.config };
    this.divideText();
  },
  props: ["item", "index", "bdr", "canceled", "config"],
  components: {
    // VueDraggableResizable,
    VueDragResize,
  },
  methods: {
    onResize: function (x, y, width, height) {
      console.log(x, y, width, height);
    },
    onDrag: function (x, y) {
      console.log(this.config["drag" + this.$props.index], y);
      console.log(x, y);
    },
    activeItem: function (classs) {
      document.getElementsByClassName(
        this.$parent.active
      )[0].firstElementChild.style.backgroundColor = "red";

      this.$parent.active = classs;
      document.getElementsByClassName(
        classs
      )[0].firstElementChild.style.backgroundColor = "yellow";
    },
    divideText() {
      let iter = 0,
        item = this.item,
        id = this.index,
        words = [];
      item = `${item}`;
      for (const i of item) {
        words[iter] == undefined ? (words[iter] = "" + i) : (words[iter] += i);
        if (i == " ") iter++;
      }

      function getTextWidth(text, font) {
        // re-use canvas object for better performance
        var canvas =
          getTextWidth.canvas ||
          (getTextWidth.canvas = document.createElement("canvas"));
        var context = canvas.getContext("2d");
        context.font = font;
        var metrics = context.measureText(text);
        return metrics.width;
      }

      iter = 0;
      for (const i of words) {
        if (
          getTextWidth(this.lines[iter] + i, "normal 14px serif") <
          document.getElementsByClassName("drag" + id)[0].style.width
        ) {
          this.lines[iter] += i;
        }
        if (
          getTextWidth(this.lines[iter] + i, "normal 14px serif") >=
          document.getElementsByClassName("drag" + id)[0].style.width
        ) {
          ++iter;
        }
        console.log(
          id,
          i,
          getTextWidth(this.lines[iter] + i, "normal 14px serif"),
          document.getElementsByClassName("drag" + id)[0].offsetWidth,
          document.getElementsByClassName("drag" + id)[0]
        );
      }

      console.log(this.lines);
      this.lines = [...this.lines];
    },
    dragFunc: function (ss) {
      // dragFunc: function (ss, ev, index) {
      function moveAt(e) {
        ss.target.parentElement.style.left =
          e.pageX -
          document.getElementById("print_area").getBoundingClientRect().left +
          "px";
        ss.target.parentElement.style.top =
          e.pageY -
          40 -
          document.getElementById("print_area").getBoundingClientRect().top +
          "px";
      }
      document.getElementById("print_area").onmousemove = function (e) {
        moveAt(e);
      };
      ss.target.i = 1;
    },
    // move: function (id, k) {
    //   for (let i of document.getElementsByClassName(
    //     this.$parent.active + "child1"
    //   )) {
    //     if (k == "d") {
    //       console.log(i.textContent);
    //       ++this.spaces;
    //       // this.divideText();
    //     } else if (k == "u") {
    //       console.log(i.textContent);
    //       // i.innerHTML = '&npsp;' + i.innerHTML
    //       if (this.spaces > 0) --this.spaces;
    //     }
    //   }
    //   for (let i of document.getElementsByClassName(
    //     this.$parent.active + "child2"
    //   )) {
    //     if (k == "l") {
    //       console.log(i.style.left);
    //       i.style.left = `${Number(i.style.left.replace(/[^+\d]/g, "")) - 1}px`;
    //     } else if (k == "r") {
    //       console.log(i.style.left);
    //       i.style.left = `${Number(i.style.left.replace(/[^+\d]/g, "")) + 1}px`;
    //     }
    //   }
    // },
  },
};
</script>

<style lang="scss" scoped></style>