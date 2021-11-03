<template>
  <div class="node">
    <div class="card">
      {{nodeData.title}}
      <div @click="add" class="add">+</div>
    </div>
    <template v-if="nodeData.children">
      <div class="connect">
        <svg :width="nodeWidth" height="100" version="1.1" class="line">
          <defs>
            <linearGradient id="greenGradient">
              <stop offset="0" stop-color="#b2ed9d" />
              <stop offset="100%" stop-color="#569b3d" />
            </linearGradient>
          </defs>
          <path v-for="(child,ind) in nodeData.children" :key="'path_'+child.id+'_'+ind"
            :d="`M${nodeWidth/2-10} 0 C  ${nodeWidth/2-10} 40, ${child.left||0} 60, ${child.left||0} 100`" fill="none"
            stroke="#b2ed9d" stroke-width="10" />
        </svg>
      </div>
      <TreeNode @tree-updated="onTreeUpdated" :ref="child.id" v-for="child in nodeData.children" :nodeData="child"
        :key="child.id"></TreeNode>
    </template>
  </div>
</template>

<script>
import TreeNode from "./TreeNode";

export default {
  name: "TreeNode",
  props: ["nodeData"],
  components: { TreeNode },
  data() {
    return {
      nodeWidth: 0,
    };
  },
  methods: {
    add() {
      if (!this.nodeData.children) {
        this.$set(this.nodeData, "children", []);
      }
      this.nodeData.children.push({
        id: Date.now(),
        title: "步骤xx",
      });
      //本级组件更新之后,通知父组件重新计算位置
      this.$nextTick(() => {
        this.$emit("tree-updated");
      });
    },
    computePosition() {
      if (this.$el.clientWidth && this.nodeData.children) {
        this.nodeWidth = this.$el.clientWidth;
        this.nodeData.children.forEach((child) => {
          const childEl = this.$refs[child.id][0].$el;
          const offsetLeft =
            childEl.offsetLeft -
            childEl.parentNode.offsetLeft +
            childEl.offsetWidth / 2;
          this.$set(child, "left", offsetLeft);
        });
      }
    },
    onTreeUpdated() {
      this.computePosition();
      this.$emit("tree-updated");
    },
  },
  updated() {
    this.computePosition();
  },
  mounted() {
    this.computePosition();
  },
};
</script>

<style lang="scss" scoped>
.node {
  display: inline-block;
  vertical-align: top;
  text-align: center;
  white-space: nowrap;
  .row {
    text-align: center;
    white-space: nowrap;
    font-size: 0;
  }
  .connect {
    height: 100px;
  }
  .card {
    position: relative;
    display: inline-block;
    font-size: 20px;
    height: 150px;
    line-height: 150px;
    width: 250px;
    text-align: center;
    background: #efefef;
    border-radius: 10px;
    box-shadow: 0 0 5px rgba(0, 0, 0, 0.3);
    margin-right: 20px;
  }
  .add {
    position: absolute;
    bottom: 10px;
    left: 50%;
    width: 50px;
    height: 50px;
    line-height: 50px;
    text-align: center;
    background: #ccc;
    margin-left: -25px;
    border-radius: 50%;
    font-size: 20px;
    box-shadow: 0 0 2px rgba(0, 0, 0, 0.3);
    transition: all 0.3s;
    cursor: pointer;
    &:hover {
      box-shadow: 0 0 5px rgba(0, 0, 0, 0.3);
    }
  }
}
</style>