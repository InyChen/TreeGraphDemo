<template>
  <div class="home">
    <TreeNode :nodeData="nodeData"></TreeNode>
  </div>
</template>

<script>
import TreeNode from "./TreeNode.vue";

export default {
  components: { TreeNode },
  data() {
    return {
      nodeData: {
        id: 1,
        title: "步骤一",
        children: [
          {
            id: 2,
            title: "步骤二",
          },
          {
            id: 3,
            title: "步骤三",
            children: [
              {
                id: 4,
                title: "步骤四",
              },
              {
                id: 5,
                title: "步骤五",
              },
            ],
          },
        ],
      },
    };
  },
  methods: {
    add(node) {
      node.child = "";
    },
  },
};
</script>


<style lang="scss" scoped>
.home {
  padding: 20px;
  text-align: center;
}
</style>