<template>
  <div>
    <el-dropdown-filter :list.sync="tableColumn"></el-dropdown-filter>
    <el-table :data="tableData" stripe style="width: 100%">
      <template v-for="(item, index) in resTableColumn">
        <el-table-column :prop="item.prop" :label="item.label" :key="index" v-if="item.show_column"></el-table-column>
      </template>
    </el-table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tableColumn: [
        {
          prop: 'date',
          label: '日期',
          show_column: true
        },
        {
          prop: 'name',
          label: '姓名',
          show_column: true
        },
        {
          prop: 'address',
          label: '地址',
          show_column: false
        }
      ],
      tableData: [
        {
          date: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        },
        {
          date: '2016-05-04',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1517 弄'
        },
        {
          date: '2016-05-01',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1519 弄'
        },
        {
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1516 弄'
        }
      ]
    };
  },
  computed: {
    resTableColumn() {
      console.log(this.tableColumn);
      return this.tableColumn;
    }
  }
};
</script>
