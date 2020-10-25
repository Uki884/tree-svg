<template>
  <svg :width="rect.totalWidth" :height="rect.height">
    <line
      :x1="computeLine1.x1"
      :y1="computeLine1.y1"
      :x2="computeLine1.x2"
      :y2="computeLine1.y2"
      stroke="#e74c3c"
    ></line>
    <rect
      ref="rect"
      :width="computeRect.width"
      :height="computeRect.height"
      :x="computeRect.x"
      :y="computeRect.y"
      fill="white"
      stroke="black"
    ></rect>
    <text
      ref="text"
      x="110"
      y="40"
      font-size="h"
      id="text"
      text-anchor="middle"
    >
      {{ company.name }}
    </text>
    <line
      :x1="computeLine2.x1"
      :y1="computeLine2.y1"
      :x2="computeLine2.x2"
      :y2="computeLine2.y2"
      stroke="#e74c3c"
    ></line>
  </svg>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "PrimeCompany",
  props: {
    line1: {
      type: Object,
      default: null
    },
    line2: {
      type: Object,
      default: null
    },
    rect: {
      type: Object,
      default: null
    },
    company: {
      type: Object,
      default: null
    }
  },
  mounted() {
    const box = (this as any).$refs.text.getBBox();
    const b = (this as any).$refs.rect;
    console.log(box);
    // b.setAttribute("x",box.x);
    // b.setAttribute("y",box.y);
    // b.setAttribute("width",box.width);
    // b.setAttribute("height",box.height);
  },
  computed: {
    computeLine1(): any {
      const { x1, x2, y1, y2 } = this.line1;
      return {
        x1,
        x2,
        y1,
        y2
      };
    },
    computeLine2(): any {
      const { x1, x2, y1, y2 } = this.line2;
      return {
        x1,
        x2,
        y1,
        y2
      };
    },
    computeRect(): any {
      const { width, height, x, y } = this.rect;
      return {
        width,
        height,
        x,
        y
      };
    }
  }
});
</script>
