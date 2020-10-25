<template>
  <g
    v-if="tree"
    :transform="`translate(${objectWidth},${objectHeight})`"
    :id="`g-parent-${parentIndex}`"
    :ref="'parent' + parentIndex"
  >
    <svg :width="234" :height="76">
      <line
        :x1="computeLine1.x1"
        :y1="computeLine1.y1"
        :x2="computeLine1.x2"
        :y2="computeLine1.y2"
        stroke="#e74c3c"
      ></line>
      <rect
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
        {{ tree.company.name }}
      </text>
      <line
        :x1="computeLine2.x1"
        :y1="computeLine2.y1"
        :x2="computeLine2.x2"
        :y2="computeLine2.y2"
        stroke="#e74c3c"
      ></line>
    </svg>
  </g>
  <g
    v-if="tree.children"
    :ref="`children-${parentIndex}`"
    :id="`children-${parentIndex}`"
  >
    <tree-company
      v-for="(child, key, childIdx) in tree.children"
      :key="'children' + childIdx"
      :line1="line1"
      :line2="line2"
      :rect="rect"
      :tree="child"
      :index="childIdx++"
      :objectWidth="objectWidth + objectWidth"
      :objectHeight="createObjectHeight(objectHeight, childIdx)"
    />
  </g>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import TreeCompany from "@/components/TreeCompany.vue";

export default defineComponent({
  name: "TreeCompany",
  components: {
    TreeCompany
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
      default: null
    },
    parentIndex: {
      type: Number,
      default: null
    }
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
  },
  watch: {
    tree: {
      deep: true,
      immediate: true,
      handler(val) {
        console.log(val);
      }
    }
  },
  // async mounted(){
  //   await this.$nextTick(()=> {
  //     console.log(this.$refs[`children-${this.index}`])
  //     // .getBoundingClientRect()
  //   })
  // },
  methods: {
    createTranslate(objectWidth) {
      console.log(this.index);
      let width;
      if (this.index == 0) {
        width = objectWidth;
      } else {
        width = objectWidth + objectWidth;
      }
      return `translate(${width},0)`;
    },
    createObjectHeight(height: number, index: number) {
      console.log(index, height);
      const y = index * 100;
      return y;
    },
    getParentHeight(index, objectHeight) {
      if (index == 0 || !index) {
        return objectHeight;
      }
      const i = index - 1;
      console.log(i, "発火");
      const children = document.getElementById(`children-${i}`);
      console.log("this", index, children);
      const rect = children.getBoundingClientRect();
      console.log(rect, objectHeight + rect.x);
      return objectHeight + rect.x;
    }
  }
});
</script>
