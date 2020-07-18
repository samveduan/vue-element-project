<template>
  <div>
    <el-button type="primary" @click="downloadData">加载数据</el-button>
    <div id="tree" class="ztree" style="height: 300px;"></div>
  </div>
</template>

<script>
export default {
  methods: {
    onClick: function(event, treeId, treeNode) {
      console.log(treeNode);
    },
    downloadData(){
      $.fn.zTree.init($("#tree"), this.orgTreeSetting, this.znodes);
    }
  },
  data() {
    return {
      orgTreeSetting: {
        view: {
          showLine: true,
          showIcon: true,
          selectedMulti: true
        },
        data: {
          simpleData: {
            enable: true,
            idKey: "id",
            pIdKey: "pId"
          }
          /*key: {
					 name: "alias",
						 title: "name"
				} */
        },
        callback: {
          onClick: this.onClick
        }
      },
      znodes: [
        { id: 1, pId: 0, name: "随意勾选 1", open: true },
        { id: 11, pId: 1, name: "随意勾选 1-1" },
        { id: 12, pId: 1, name: "随意勾选  1-2", open: true },
        { id: 121, pId: 12, name: "随意勾选 1-2-1", checked: true },
        { id: 122, pId: 12, name: "随意勾选 1-2-2" },
        { id: 123, pId: 12, name: "随意勾选 1-2-3" },
        { id: 13, pId: 1, name: "随意勾选 1-3" },
        { id: 2, pId: 0, name: "随意勾选 2", open: true },
        { id: 21, pId: 2, name: "随意勾选 2-1" },
        { id: 22, pId: 2, name: "随意勾选 2-2", open: true },
        { id: 221, pId: 22, name: "随意勾选 2-2-1", checked: true },
        { id: 222, pId: 22, name: "随意勾选 2-2-2" },
        { id: 223, pId: 22, name: "随意勾选 2-2-3" },
        { id: 23, pId: 2, name: "随意勾选 2-3", checked: true }
      ]
    };
  },
  mounted() {

  }
};
</script>

<style scoped>
</style>