<template>
  <g
    v-if="tree"
    :transform="`translate(${objectWidth},${height})`"
    :id="`g-parent-${parentIndex}`"
    :ref="'parent' + parentIndex"
  >
    <svg :width="234" :height="76">
      <line
        :x1="line1.x1"
        :y1="line1.y1"
        :x2="line1.x2"
        :y2="line1.y2"
        stroke="#e74c3c"
      ></line>
      <rect
        :width="rect.width"
        :height="rect.height"
        :x="rect.x"
        :y="rect.y"
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
        {{ tree.company.name }}
      </text>
      <line
        :x1="line2.x1"
        :y1="line2.y1"
        :x2="line2.x2"
        :y2="line2.y2"
        stroke="#e74c3c"
      ></line>
    </svg>
    <g
      v-if="tree.children"
      :ref="`children-${parentIndex}-${index}`"
      :id="`children-${parentIndex}-${index}`"
      :transform="`translate(${objectWidth},${height})`"
    >
      <tree-company
        v-for="(child, key, childIdx) in tree.children"
        :key="'children' + childIdx"
        :line1="line1"
        :line2="line2"
        :rect="rect"
        :tree="child"
        :index="childIdx + 1"
        :parentIndex="parentIndex + 1"
        :objectWidth="createObjectWidth(index, objectWidth)"
        :objectHeight="createObjectHeight(objectHeight, childIdx)"
      />
    </g>
  </g>
</template>

<script lang="ts">
import { defineComponent, HtmlHTMLAttributes } from "vue";
// import TreeCompany from "@/components/TreeCompany.vue";

export default defineComponent({
  name: "tree-company",
  components: {
    TreeCompany: () => import("@/components/TreeCompany.vue")
  },
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
    tree: {
      type: Object,
      default: null
    },
    company: {
      type: Object,
      default: null
    },
    objectWidth: {
      type: Number,
      default: null
    },
    objectHeight: {
      type: Number,
      default: null
    },
    index: {
      type: Number,
      default: 0
    },
    parentIndex: {
      type: Number,
      default: 0
    }
  },
  data() {
    return {
      height: 0,
      width: null
    };
  },
  watch: {
    "tree.children": {
      deep: true,
      immediate: true,
      async handler(val) {
        await this.$nextTick(() => {
          const parentIndex = this.parentIndex == 0 ? 0 : -1;
          const index = this.index;
          const prevChildren = document.getElementById(
            `children-${parentIndex}-${index}`
          );
          console.log(this.parentIndex);
          if (!prevChildren) {
            this.height = this.objectHeight;
            return;
          }
          const rect = prevChildren?.getBoundingClientRect();
          this.height = this.objectHeight;
        });
      }
    }
  },
  methods: {
    createTranslate(objectWidth) {
      console.log(this.index);
      let width;
      if (this.index == 0) {
        width = objectWidth;
      } else {
        console.log(objectWidth);
        width = objectWidth + 226;
      }
      return `translate(${width},0)`;
    },
    createObjectWidth(index: number, objectWidth: number) {
      if (index == 0) {
        return 0;
      }
      const width = objectWidth + 226;
      return width;
    },
    createObjectHeight(height: number, index: number) {
      console.log(index, height);
      const i = index - 1;
      const y = i * 100;
      return y;
    }
  }
});
</script>
