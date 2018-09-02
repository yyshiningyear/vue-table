<template>
  <div id="app">
    <vue-table
      :columns='columns'
      :tableData='tableData'>
    </vue-table>
  </div>
</template>

<script>
import VueTable from './components/VueTable'

export default {
  name: 'App',
  components: {
    VueTable
  },
  data() {
    return {
      columns: [
        {
          key: 'no',
          name: 'No',
          width: 80,
        }, {
          key: 'col1',
          name: '第一列',
          width: 200,
        }, {
          key: 'col2',
          name: '第二列',
          width: 200,
        }, {
          key: 'col3',
          name: '第三列',
          width: 200,
        }, {
          key: 'col4',
          name: '第四列',
          width: 200,
        }, {
          key: 'col5',
          name: '第五列',
          width: 200,
        }, {
          key: 'col6',
          name: '第六列',
          width: 200,
        }, {
          key: 'col7',
          name: '第七列',
          width: 200,
        }
      ],
      tableData: []
    }
  },
  beforeMount() {
    this.initData();
  },
  methods: {
    initData() {
      const dataSize = 100000;
      const dataList = [];
      for (let i = 1; i <= dataSize; i++) {
        const data = {
          no: i
        };
        for (let j = 1; j <=7; j++) {
          data[`col${j}`] = `第${i}行第${j}列`
        }
        dataList.push(data);
      }

      this.tableData = dataList;
    }
  }
}
</script>

<style>
#app {
  width: 800px;
  height: 500px;
  margin: 0 auto;
}
</style>
