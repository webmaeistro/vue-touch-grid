<template>
  <div>
    <h1>{{title}}</h1>
    <p>
      See
      <a
        href="https://github.com/jbaysolutions/vue-grid-layout/issues/333"
      >Document how to "maximize" a GridItem... · Issue #333 · jbaysolutions/vue-grid-layout</a>
    </p>
    <button :disabled="maximizeID" @click.prevent="maximizeID=1">Maximize item 1</button>
    <button :disabled="!maximizeID" @click.prevent="maximizeID=null">Restore</button>
    Maximized: {{maximizeID || "none"}}
    <div class="gridContainer">
      <grid-layout
        :layout="layout"
        :col-num="12"
        :row-height="30"
        :is-draggable="! maximizeID"
        :is-resizable="! maximizeID"
        :is-mirrored="false"
        :vertical-compact="true"
        :margin="[10, 10]"
        :use-css-transforms="true"
        class="gridLayout"
        :class="{maximized: maximizeID}"
      >
        <grid-item
          v-for="item in layout"
          :x="item.x"
          :y="item.y"
          :w="item.w"
          :h="item.h"
          :i="item.i"
          class="gridItem"
          :class="{
                     maximized: maximizeID == item.i,
                     hidden: maximizeID && maximizeID != item.i
                  }"
        >{{item.i}}</grid-item>
      </grid-layout>
    </div>
  </div>
</template>

<script>
import { GridItem, GridLayout } from "vue-grid-layout";
import $ from "jquery";
import jqueryUi from "jquery.ui.touch";
export default {
  components: {
    GridLayout,
    GridItem
  },
  props: {
    value: {
      type: String,
      default: ""
    }
  },
  data() {
    return {
      title: "",
      layout: [
        { x: 0, y: 0, w: 2, h: 4, i: "0" },
        { x: 2, y: 0, w: 2, h: 4, i: "1" },
        { x: 4, y: 0, w: 2, h: 4, i: "2" }
      ],
      maximizeID: null
    };
  },
  computed: {
    listeners() {}
  },
  methods: {},
  mounted() {
    this.title = "GridLayout with Maximizable GridItem";
    $(".vue-grid-layout").addTouch();
  }
};
</script>

<style lang="scss" scoped>
@import "../variables.scss";

.input {
  width: 100%;
  padding: 8px 10px;
  border: 1px solid $vue-blue;
}
.gridContainer {
  background-color: #ccc;
  height: 250px;
}
.gridLayout {
  background-color: #eee;
}
.gridLayout.maximized {
  height: 100% !important;
}
.gridItem {
  background-color: gray;
  /* width is omitted from the properties in vue-grid-layout
      and that causes transitions to appear weirdly during maximize */
  -webkit-transition-property: left, top, right, width;
  transition-property: left, top, right, width;
}
.gridItem.hidden {
  display: none;
}
.gridItem.maximized {
  transform: inherit !important;
  height: 100% !important;
  width: 100% !important;
}
</style>
