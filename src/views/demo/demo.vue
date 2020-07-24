<template>
  <div>
    <el-cascader
      v-model="value"
      :options="options"
      @change="handleChange"
      :props="defaultParams"
    ></el-cascader>
  </div>
</template>
<script>
export default {
  name: "",
  data() {
    return {
      value: [],
      options: [],
      defaultParams: {
        checkStrictly: true,
        label: "name",
        value: "id",
        children: "list",
      },
      
    };
  },
  components: {},
  computed: {},
  beforeMount() {},
  created() {},
  mounted() {
    this.getList();
  },
  methods: {
    getList() {
      this.$http({
        url: this.$http.adornUrl("/admin/category/selecttree"),
        method: "get",
        params: this.$http.adornParams(),
      }).then(({ data }) => {
        console.log(data);
        this.options = this.getTreeData(data.categoryList);
      });
    },
    getTreeData(data) {
      // 循环遍历json数据
      for (var i = 0; i < data.length; i++) {
        if (data[i].list == null || data[i].list.length < 1) {
          // children若为空数组，则将children设为undefined
          data[i].list = undefined;
        } else {
          // children若不为空数组，则继续 递归调用 本方法
          this.getTreeData(data[i].list);
        }
      }
      return data;
    },

    handleChange(value) {
      console.log(value);
    },
  },
  watch: {},
};
</script>
<style scoped>
</style>
