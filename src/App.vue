<template>
  <div id="wrapper">
    <main class="main">
      <div class="manager">
        <div class="menu">
          <div class="mode">Paper Format</div>
          <div class="mode">
            <select name="" id="" style="width: 100%">
              <option value="a4">A4</option>
              <option value="a5">A5</option>
            </select>
          </div>
        </div>
        <div class="menu">
          <div class="mode">Font size</div>
          <div class="mode">
            <button @click="changeFontSize(1)">+1</button
            ><button @click="changeFontSize(-1)">-1</button>
          </div>
        </div>
        <div class="menu">
          <div class="mode">Width</div>
          <div class="mode">
            <button :style="{ background: sb }" @click="showBorder()">
              Show Border</button
            ><button @click="changeWidth(1)">+1</button
            ><button @click="changeWidth(-1)">-1</button
            ><button @click="changeWidth(10)">+10</button
            ><button @click="changeWidth(-10)">-10</button>
          </div>
        </div>
        <div class="menu">
          <div class="mode">Line Height</div>
          <div class="mode">
            <button @click="changeLineHeight(0.1)">+1</button
            ><button @click="changeLineHeight(-0.1)">-1</button>
          </div>
        </div>
        <div class="menu">
          <div class="mode">Text Align</div>
          <select
            @change="changeAlign"
            v-model="align"
            class="mode"
            name="align"
            id="align"
          >
            <option v-for="(i, j) in text_aligns" :value="i" :key="j">
              {{ i }}
            </option>
          </select>
        </div>
        <div class="menu">
          <div class="mode">Upload template</div>
          <div class="mode">
            <input
              id="file"
              name="Template"
              accept="image/*"
              type="file"
              @change="showTemplate($event)"
            />
          </div>
        </div>
        <div class="menu">
          <a
            @click="saveConfig()"
            class="mode"
            id="save_config"
            download="config.json"
            >Save configuration
          </a>
        </div>
        <div class="menu">
          <div class="mode">Upload configuration</div>
          <div class="mode">
            <input
              type="file"
              accept=".json"
              onchange="vm.uploadConf(this)"
              id="upload_config"
            />
          </div>
        </div>
        <div class="menu">
          <div class="mode" @click="test(1)">PRINT</div>
          <input
            class="mode"
            accept=".json"
            type="file"
            id="print-btn"
            onchange="vm.print(this)"
          />
        </div>
      </div>
      <div id="print_area">
        <div class="container" v-for="(student, i) in students" :key="i">
          <Lines
            v-for="(item, index) in student"
            :key="index"
            :item="item"
            :index="index"
            :bdr="bdr"
            :lines="lines"
            @activeitem="activeItem"
            @activechild="activeChild"
            :spaces="spaces"
            @movel="move"
            @mover="move"
          >
            <!-- < div
                    : style = "{border:bdr}"
                    style = "display: block; z-index: 30"
                    v -for= "text, id in divideText(item)"
                    class= "dragable"
                    : class='"text"+id'
                    >
                    {{ text }}
                  </div > -->
          </Lines>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import Lines from "./components/Lines.vue";

let students = [
  [
    "Number",
    "Fullname Fullname Fullname Fullname Fullname Fullname Fullname Fullname Fullname Fullname",
    "Univercity Univercity Univercity Univercity Univercity Univercity Univercity Univercity",
    "Programmer developer tester phisyc scientist Programmer developer tester phisyc scientist",
  ],
];
let text_aligns = [
  "start",
  "center",
  "end",
  "left",
  "right",
  "justify",
  "inherit",
  "initial",
  "unset",
];
for (let i of document.getElementsByClassName("dragElement")) {
  i.style.fontSize = "14px";
}

window.oncontextmenu = function () {
  document.getElementById("print_area").onmousemove = null;
  return false;
};

export default {
  // el: "#wrapper",
  data() {
    return {
      emp: 0,
      fontSize: "14px",
      bdr: "1px solid #000",
      sb: "red",
      align: "left",
      active: "drag0",
      active_child: "drag00",
      // students: [],
      students,
      text_aligns,
      config: {},
      configjson: "{}",
      lines: [
        // "aaaa", "bbbb"
      ],
      spaces: 0,
    };
  },
  components: {
    Lines,
  },
  computed: {},
  mounted() {
    // fetch("https://jsonplaceholder.typicode.com/todos?_limit=10")
    // fetch("https://api.github.com/users/hadley/orgs")
    //     .then((response) => response.json())
    //     .then((json) => {
    //         setTimeout(() => {
    //             this.students = json;
    //             // this.loading = false;
    //         }, 1000);
    //     });

    window.vm = this;
    setTimeout(() => {
      for (let i of document.getElementsByClassName("dragable")) {
        for (let j of i.children) {
          if (!j.classList.contains("drag_zone")) {
            j.style.width = j.offsetWidth + "px";
          }
        }
      }
    }, 100);
  },
  methods: {
    test: function name(ss) {
      console.log(ss);
    },
    dragFunc: function (ss) {
      function moveAt(e) {
        ss.style.left =
          e.pageX -
          2 -
          document.getElementById("print_area").getBoundingClientRect().left -
          window.scrollX +
          "px";
        ss.style.top =
          e.pageY -
          2 -
          document.getElementById("print_area").getBoundingClientRect().top -
          window.scrollY +
          "px";
      }
      document.getElementById("print_area").onmousemove = function (e) {
        moveAt(e);
      };
    },
    activeItem: function (classs) {
      this.active = classs;
      this.align = document.querySelector("." + this.active).style.textAlign;
      for (let i of document.getElementsByClassName("drag_zone")) {
        i.parentElement.classList.contains(this.active)
          ? (i.style.backgroundColor = "orange")
          : (i.style.backgroundColor = "red");
      }
    },
    activeChild: function (classs) {
      this.active_child = classs;
      // this.align = document.querySelector("." + this.active_child).style.textAlign;
      for (let i of document.getElementsByClassName("dragable")) {
        for (let j of i.children) {
          j.classList.contains(classs)
            ? (j.style.backgroundColor = "rgba(255,255,0,.7)")
            : (j.style.backgroundColor = "");
        }
      }
    },
    showBorder: function () {
      this.sb == "red" ? (this.sb = "#eee") : (this.sb = "red");
      this.bdr == "" ? (this.bdr = "1px solid #000") : (this.bdr = "");
      // for (let i of document.querySelectorAll(".drag_zone")) {
      //     i.style.backgroundColor != 'transparent' ? i.style.backgroundColor = 'transparent' : i.style.backgroundColor = 'red';
      // }
    },
    divideText(ch) {
      let iter = 0,
        line =
          this.lines[this.active[this.active.length - 1]][
            this.active_child[this.active_child.length - 1]
          ],
        line2 =
          this.lines[this.active[this.active.length - 1]][
            Number(this.active_child[this.active_child.length - 1]) + 1
          ],
        words = [];

      if (ch > 0) line2; //!= undefined ? (line = line + " " + line2) : line;

      if (
        !this.lines[this.active[this.active.length - 1]][
          Number(this.active_child[this.active_child.length - 1]) + 1
        ]
      ) {
        this.lines[this.active[this.active.length - 1]][
          Number(this.active_child[this.active_child.length - 1]) + 1
        ] = "";
      }

      for (const i of line) {
        words[iter] == undefined ? (words[iter] = "" + i) : (words[iter] += i);
        if (i == " ") iter++;
      }
      // for (const i of line2) {
      //   words[iter] == undefined ? (words[iter] = "" + i) : (words[iter] += i);
      //   if (i == " ") iter++;
      // }

      console.log(words);

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

      let getTextSize = () => {
        return document.getElementsByClassName(this.active)[0].style.fontSize
          ? document.getElementsByClassName(this.active)[0].style.fontSize
          : "14px";
      };

      iter = 0;

      if (
        getTextWidth(line, `normal ${getTextSize()} serif`) <
        document
          .getElementsByClassName(this.active_child)[0]
          .style.width.replace(/[^+\d]/g, "")
      ) {
        this.lines[this.active[this.active.length - 1]][
          this.active_child[this.active_child.length - 1]
        ] = line;
      }

      line = "";
      line2 =
        this.lines[this.active[this.active.length - 1]][
          Number(this.active_child[this.active_child.length - 1]) + 1
        ];

      window.words = words;
      for (const i of words) {
        if (
          getTextWidth(line + i, `normal ${getTextSize()} serif`) <
          document
            .getElementsByClassName(this.active_child)[0]
            .style.width.replace(/[^+\d]/g, "")
        ) {
          line += i;
        } else {
          line2 = i + line2;
        }
      }
      this.lines[this.active[this.active.length - 1]][
        this.active_child[this.active_child.length - 1]
      ] = line;

      this.lines[this.active[this.active.length - 1]][
        Number(this.active_child[this.active_child.length - 1]) + 1
      ] = line2;
      window.line = line;
      this.lines = [...this.lines];
    },
    move: function (id, k) {
      for (let i of document.getElementsByClassName(this.active_child)) {
        if (k == "l") {
          if (i.style.left.replace(/[^+\d]/g, "") > 0) {
            i.style.left = `${
              Number(i.style.left.replace(/[^+\d]/g, "")) - 1
            }px`;
          }
        } else if (k == "r") {
          // if (!i.style.left) i.style.left = "0px";
          i.style.left = `${Number(i.style.left.replace(/[^+\d]/g, "")) + 1}px`;
        }
        // else if (k == "u") {
        //   console.log(i.style.left);
        //   i.style.left = `${Number(i.style.left.replace(/[^+\d]/g, "")) + 1}px`;
        // } else if (k == "d") {
        //   console.log(i.style.left);
        //   i.style.left = `${Number(i.style.left.replace(/[^+\d]/g, "")) + 1}px`;
        // }
      }
    },
    changeFontSize: function (c) {
      for (let i of document.getElementsByClassName(this.active)) {
        if (!document.getElementsByClassName(this.active)[0].style.fontSize) {
          i.style.fontSize = "14px";
        }
        i.style.fontSize = `${
          Number(i.style.fontSize.replace(/[^+\d]/g, "")) + c
        }px`;
      }
    },
    changeLineHeight: function (c) {
      for (let i of document.getElementsByClassName(this.active)) {
        if (!document.getElementsByClassName(this.active)[0].style.lineHeight) {
          i.style.lineHeight = "1";
        }
        i.style.lineHeight = `${Number(i.style.lineHeight) + c}`;
      }
    },
    changeAlign: function () {
      for (let i of document.getElementsByClassName(this.active)) {
        i.style.textAlign = this.align;
      }
    },
    changeWidth: function (c) {
      for (let i of document.getElementsByClassName(this.active_child)) {
        if (
          !document.getElementsByClassName(this.active_child)[0].style.width
        ) {
          i.style.width = i.offsetWidth + "px";
        }
        i.style.width = `${Number(i.style.width.replace(/[^+\d]/g, "")) + c}px`;
      }

      this.divideText(c);

      setTimeout(() => {
        for (let i of document.getElementsByClassName("dragable")) {
          for (let j of i.children) {
            if (!j.classList.contains("drag_zone")) {
              j.style.width = j.clientWidth + "px";
            }
          }
        }
      }, 100);
    },
    showTemplate: function (event) {
      for (const output of document.getElementsByClassName("container")) {
        output.style.backgroundImage = `url(${URL.createObjectURL(
          event.target.files[0]
        )})`;
        output.onload = function () {
          URL.revokeObjectURL(output.src); // free memory
        };
      }
    },
    saveConfig: function () {
      for (const i in this.students[0]) {
        let el = document.getElementsByClassName("drag" + i)[0].style;

        this.config["drag" + i] = {
          fontSize: el.fontSize,
          width: el.width,
          lineHeight: el.lineHeight,
          textAlign: el.textAlign,
          top: el.top,
          left: el.left,
        };
        console.log(this.config);
      }
      this.configjson = JSON.stringify(this.config);
      this.config = JSON.parse(this.configjson);
      console.log(this.configjson);

      let blob = new Blob([this.configjson], { type: "text/json" });

      document.getElementById("save_config").href = URL.createObjectURL(blob);
    },
    uploadConf(inp) {
      let reader = new FileReader();
      let file = inp.files[0];

      reader.readAsText(file);
      reader.onload = () => {
        let config = JSON.parse(reader.result);
        for (const i in config) {
          for (let j of document.getElementsByClassName(i)) {
            for (const k in config[i]) {
              j.style[k] = config[i][k];
            }
          }
        }
      };
    },
    print: function (inp) {
      let reader = new FileReader();
      let file = inp.files[0];
      this.bdr = "";

      // this.showBorder

      reader.readAsText(file);
      reader.onload = () => {
        this.students = JSON.parse(reader.result);
        console.log(this.students);
        let timerId = setInterval(() => {
          for (let i of document.getElementsByClassName("container")[0]
            .children) {
            console.log(document.getElementsByClassName(i.classList[2]));
            // for (let j of i.children) {
            for (let j of document.getElementsByClassName(i.classList[2])) {
              j.style.width = document.getElementsByClassName(
                i.classList[2]
              )[0].style.width;
              j.style.fontSize = document.getElementsByClassName(
                i.classList[2]
              )[0].style.fontSize;
              j.style.lineHeight = document.getElementsByClassName(
                i.classList[2]
              )[0].style.lineHeight;
              j.style.textAlign = document.getElementsByClassName(
                i.classList[2]
              )[0].style.textAlign;
              j.style.top = document.getElementsByClassName(
                i.classList[2]
              )[0].style.top;
              j.style.left = document.getElementsByClassName(
                i.classList[2]
              )[0].style.left;
            }
            // }
          }
          if (
            document.getElementById("print_area").children.length ==
            this.students.length
          ) {
            document.body.innerHTML =
              document.getElementById("print_area").innerHTML;
            window.print();
            document.location.reload();
            clearInterval(timerId);
          }
        }, 1000);
      };

      // document.activeElement = ''
    },
  },
};
</script>
