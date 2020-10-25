<template>
  <div class="trees">
    <svg :width="width" :height="height">
      <prime-company
        :line1="initLine1"
        :line2="initLine2"
        :rect="initRect"
        :company="trees.company"
      />
      <tree-company
        v-for="(tree, key, index) in trees.children"
        class="tree-company"
        :key="'tree' + index"
        :line1="initLine1"
        :line2="initLine2"
        :rect="initRect"
        :tree="tree"
        :objectWidth="initRect.objectWidth"
        :objectHeight="createObjectHeight(initRect.objectHeight, index)"
        :parentIndex="index"
      />
    </svg>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import PrimeCompany from "@/components/PrimeCompany.vue";
import TreeCompany from "@/components/TreeCompany.vue";

interface Line {
  x1: number;
  y1: number;
  x2: number;
  y2: number;
}

interface Rect {
  width: number;
  height: number;
  objectWidth: number;
  objectHeight: number;
  x: number;
  y: number;
}

const initLine1: Line = {
  x1: 0,
  y1: 38,
  x2: 30,
  y2: 38
};

const initLine2: Line = {
  x1: 196,
  y1: 38,
  x2: 226,
  y2: 38
};

const initRect: Rect = {
  width: 166,
  height: 76,
  objectWidth: 166 + 30 + 30,
  objectHeight: 0,
  x: 30,
  y: 0
};

export default defineComponent({
  name: "Tree",
  components: {
    PrimeCompany,
    TreeCompany
  },
  data() {
    return {
      width: window.outerWidth,
      height: window.outerHeight,
      initLine1,
      initLine2,
      initRect,
      trees: {
        id: 1,
        company: {
          id: 1,
          name: "元請け"
        },
        children: {
          2: {
            id: 2,
            company: {
              id: 2,
              name: "テスト2"
            },
            children: {
              3: {
                id: 3,
                company: {
                  id: 3,
                  name: "テスト3"
                }
              },
              6: {
                id: 6,
                company: {
                  id: 6,
                  name: "テスト6"
                }
              },
              7: {
                id: 7,
                company: {
                  id: 7,
                  name: "テスト7"
                }
              }
            }
          },
          4: {
            id: 4,
            company: {
              id: 4,
              name: "テスト4"
            },
            children: {
              5: {
                id: 5,
                company: {
                  id: 5,
                  name: "テスト5"
                }
              }
            }
          }
        }
      }
    };
  },
  methods: {
    createInitLine(initLine1: Line, index: number) {
      console.log("line", initLine1, index);
      const childrenLength = index;
      const y = childrenLength * 100;
      console.log(y);
      return { ...initLine1 };
    },
    createObjectHeight(height: number, index: number) {
      console.log(index, height);
      const y = index * 100;
      return y;
    }
  }
});
</script>
