<template>
  <!-- wrapper用于触发滚动条 -->
  <div ref="wrapper" class="vue-table-wrapper" @scroll="handleScroll">
    <div v-if="columns.length > 0" class="vue-table" :style="{width: tableWidth+'px'}">
        <div class="vue-table-header-wrapper">
          <table border="0" class="vue-table-fixed-header">
            <colgroup>
              <col :width="columns[0].width">
            </colgroup>
            <tr :style="{height: headerHeight + 'px'}">
              <th>
                <span>{{columns[0].name}}</span>
              </th>
            </tr>
          </table>
          <table border="0" class="vue-table-header">
            <colgroup>
              <col v-for="col in columns" :key="col.key" :width="col.width">
            </colgroup>
            <tr :style="{height: headerHeight + 'px'}">
              <th v-for="col in columns" :key="col.key">
                <span>{{col.name}}</span>
              </th>
            </tr>
          </table>
        </div>
        <div class="vue-table-content">
          <div :style="{height: topMarginHeight+'px'}"></div>
          <div :style="{width: tableWidth+'px', position: 'relative'}">
            <table border="0" class="vue-table-fixed-table">
              <colgroup>
                <col :width="columns[0].width">
              </colgroup>
              <tbody>
                <tr v-for="(data, index) in showTableData"
                  :key="'fixedCol' + index"
                  :style="{height: rowHeight + 'px'}">
                  <td>
                    <div class="vue-table-cell">{{data[columns[0].key]}}</div>
                  </td>
                </tr>
              </tbody>
            </table>
            <table border="0" class="vue-table-content-table">
              <colgroup>
                <col v-for="col in columns" :key="col.key" :width="col.width">
              </colgroup>
              <tbody>
                <tr v-for="(data, index) in showTableData"
                  :key="'data' + index"
                  :style="{height: rowHeight + 'px'}">
                  <td v-for="col in columns" :key="'data' + index + col.key">
                    <div class="vue-table-cell">{{data[col.key]}}</div>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
          <div :style="{height: bottomMarginHeight + 'px'}"></div>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "VueTable",
  props: {
    columns: {
      type: Array,
      required: true,
      default: []
    },
    tableData: {
      type: Array,
      required: true,
      default: []
    }
  },
  data() {
    return {
      headerHeight: 50,
      rowHeight: 48, // TODO 先写死行高
      showHeight: 500, // 可视的宽高 先写死
      showWidth: 800,
      scrollTop: 0,
      shouldHandleScroll: true // 防止频繁触发滚动
    };
  },
  computed: {
    tableWidth() {
      let width = 0;
      this.columns.forEach(col => (width += col.width));
      return width;
    },
    tableHeight() {
      return this.tableData.length * this.rowHeight;
    },
    // 可视范围内的数据
    startIndex() {
      const start = Math.floor(this.scrollTop / this.rowHeight) - 5;
      return start < 0 ? 0 : start;
    },
    endIndex() {
      const end = this.startIndex + Math.ceil(this.showHeight / this.rowHeight) + 5;
      return end > this.tableData.length ? this.tableData.length : end;
    },
    showTableData() {
      return this.tableData.slice(this.startIndex, this.endIndex);
    },
    topMarginHeight() {
      return this.startIndex * this.rowHeight;
    },
    bottomMarginHeight() {
      return this.tableHeight - this.endIndex * this.rowHeight;
    }
  },
  watch: {
    scrollTop() {
      this.shouldHandleScroll = false;
      this.$nextTick(() => {
        this.shouldHandleScroll = true;
        this.$refs["wrapper"].scrollTop = this.scrollTop;
      });
    }
  },
  methods: {
    handleScroll(e) {
      if (!this.shouldHandleScroll) {
        return;
      }
      let ele = e.srcElement || e.target;
      this.scrollTop = ele.scrollTop;
    }
  }
};
</script>

<style lang="less" scoped>
@import "./table.less";
</style>
