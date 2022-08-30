## Dropdown-filter 表格筛选组件

对表格组件的表头进行动态控制显示与否

### 基本用法

dropdown-filter 点击弹出表格所属项，进行筛选。

:::demo 需要设置`list`属性，它接收`Array`。注意: 需要添加`.sync`才可使子组件自动更新父组件`props.list`传入的数据。使用的表格组件一定要附带`:key:index` 否则没办法实时响应修改后的数据

```html
  <template>
      <el-dropdown-filter :list.sync="tableColumn"></el-dropdown-filter>
      <el-table ref="tableRef" :data="tableData" stripe style="width: 100%">
        <template v-for="(item,index) in tableColumn">
          <el-table-column :prop="item.prop" :label="item.label" :key="index" v-if="item.show_column"></el-table-column>
        </template>
      </el-table>
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
      }
    }
  </script>
```

:::

### Attributes

| 参数      | 说明          | 类型      | 可选值                           | 默认值  |
|---------- |-------------- |---------- |--------------------------------  |-------- |
| list     | .sync 异步动态修改tableColumn数据   | array   | — | [] |
