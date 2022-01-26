<template>
  <el-tree :data="menus" :props="defaultProps" @node-click="handleNodeClick" show-checkbox :expand-on-click-node="false">
    <span class="custom-tree-node" slot-scope="{ node, data }">
      <span>{{ node.label }}</span>
      <span>
        <el-button v-if="node.level <=2" type="text" size="mini" @click="() => append(data)">
          添加
        </el-button>
        <el-button v-if="node.childNodes.length==0" type="text" size="mini" @click="() => remove(node, data)">
          删除
        </el-button>
      </span>
    </span></el-tree
  >
</template>

<script>
export default {
  data() {
    return {
      menus: [],
      defaultProps: {
        children: "children",
        label: "name",
      },
    };
  },
  methods: {
    append(data) {
      const newChild = { id: id++, label: "testtest", children: [] };
      if (!data.children) {
        this.$set(data, "children", []);
      }
      data.children.push(newChild);
    },

    remove(node, data) {
      const parent = node.parent;
      const children = parent.data.children || parent.data;
      const index = children.findIndex((d) => d.id === data.id);
      children.splice(index, 1);
    },
    getMenus() {
      this.$http({
        url: this.$http.adornUrl("/product/category/list/tree"),
        method: "get",
      }).then(({ data }) => {
        this.menus = data.data;
        console.log(data);
      });
    },

    handleNodeClick(data) {
      console.log(data.data);
    },
  },
  created() {
    this.getMenus();
  },
};
</script>

<style>
</style>