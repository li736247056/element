<template>
  <transition name="fade">
    <el-popover placement="bottom" trigger="click">
      <template slot="reference">
        <template v-if="!this.$slots.content">
          <el-button icon="el-icon-s-grid" circle></el-button>
        </template>
        <slot name="content"></slot>
      </template>
      <template>
        <div class="outer-layer el-dropdown-filter" v-if="isEmptyTableRef">
          <div class="header">
            <el-checkbox v-model="wholeColumn" label="全部" :indeterminate="wholeColumnIndeterminate" @change="wholeColumnChange" size="large"/>
            <el-button type="text" @click="confirm">确定</el-button>
          </div>
          <div class="body" ref="bodyRef">
            <ul id="draggable">
              <li class="item" v-for="item in columnList" :key="item.prop" :data-id="item.prop">
                <el-checkbox :class="dragSort ? 'drag-sort--cursor' : ''" v-model="item.show_column" :label="item.label" class="checkbox-item " size="large" />
              </li>
            </ul>
          </div>
        </div>
        <div v-else class="error">
          <span class="title">错误</span>
          <br />
          <span class="content">无法连接到表格</span>
        </div>
      </template>
    </el-popover>
  </transition>
</template>

<script>
import ElPopover from '../../popover/src/main.vue';
import Sortable from '../Sortable.min';
export default {
  name: 'ElDropdownFilter',
  components: { ElPopover },
  props: {
    list: {
      type: Array,
      default: []
    },
    dragSort: {
      type: Boolean,
      default: true
    }
  },
  data() {
    return {
      isEmptyTableRef: true,
      wholeColumn: false,
      columnList: []
    };
  },
  methods: {
    confirm() {
      this.$emit('update:list', this.columnList);
    },
    wholeColumnChange(value) {
      if (value || this.wholeColumnIndeterminate) {
        this.columnList.forEach(item => {
          item.show_column = true;
        });
        return;
      } else {
        this.columnList.forEach(item => {
          item.show_column = false;
        });
        return;
      }
    },
    addDragSortEvent() {
      const that = this;
      this.$nextTick(() => {
        const el = document.querySelector('#draggable');
        if (!el) return;
        /* eslint-disable no-new */
        var sortable = new Sortable(el, {
          animation: 300,
          onEnd(evt) {
            var arr = sortable.toArray();
            let res = [];
            arr.forEach(item => {
              let obj = that.columnList.find(columnItem => {
                return columnItem.prop === item;
              });
              if (obj) res.push(obj);
            });
            that.columnList = res;
          }
        });
      });
    }
  },
  computed: {
    wholeColumnIndeterminate() {
      let columnList = this.columnList;
      let count = 0;
      columnList.forEach(item => {
        if (item.show_column) count++;
      });
      if (count === columnList.length) {
        this.wholeColumn = true;
        return false;
      } else if (count > 0) {
        this.wholeColumn = false;
        return true;
      }
      return false;
    }
  },
  watch: {
    list: {
      immediate: true,
      deep: true,
      handler(value) {
        this.columnList = [];
        this.isEmptyTableRef = false;
        if (value && value.length > 0) {
          this.columnList = JSON.parse(JSON.stringify(value));
          this.isEmptyTableRef = true;
          return;
        }
      }
    }
  },
  mounted() {
    if (this.dragSort) {
      this.addDragSortEvent();
    }
  }
};
</script>