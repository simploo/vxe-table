<template>
  <div>
    <p class="tip">调用 <table-api-link prop="insert"/>、<table-api-link prop="insertAt"/> 函数插入临时数据，还可以通过 <table-api-link prop="icon"/> 自定义编辑状态的图标<br><span class="red">注：原生的 select 只支持字符串的 value</span></p>

    <vxe-toolbar>
      <template v-slot:buttons>
        <vxe-button icon="fa fa-plus" @click="insertEvent()">新增</vxe-button>
        <vxe-button @click="insertEvent(tableData[2])">在第3行插入并激活 Sex 单元格</vxe-button>
        <vxe-button @click="insertEvent(-1)">在最后行插入</vxe-button>
        <vxe-button @click="$refs.xTable.removeSelecteds()">删除选中</vxe-button>
        <vxe-button icon="fa fa-save" @click="getInsertEvent">保存</vxe-button>
      </template>
    </vxe-toolbar>

    <vxe-table
      border
      show-overflow
      ref="xTable"
      max-height="400"
      :data="tableData"
      :edit-config="{trigger: 'click', mode: 'cell', icon: 'fa fa-pencil'}">
      <vxe-table-column type="checkbox" width="60"></vxe-table-column>
      <vxe-table-column type="index" width="60"></vxe-table-column>
      <vxe-table-column field="name" title="Name" sortable :edit-render="{name: 'input', defaultValue: '默认的名字'}"></vxe-table-column>
      <vxe-table-column field="sex" title="Sex" :edit-render="{name: 'select', options: sexList}"></vxe-table-column>
      <vxe-table-column field="age" title="Age" sortable :edit-render="{name: 'input', defaultValue: 18}"></vxe-table-column>
    </vxe-table>

    <p class="demo-code">{{ $t('app.body.button.showCode') }}</p>

    <pre>
      <code class="xml">{{ demoCodes[0] }}</code>
      <code class="javascript">{{ demoCodes[1] }}</code>
    </pre>
  </div>
</template>

<script>
import hljs from 'highlight.js'

export default {
  data () {
    return {
      tableData: [],
      sexList: [],
      demoCodes: [
        `
        <vxe-toolbar>
          <template v-slot:buttons>
            <vxe-button icon="fa fa-plus" @click="insertEvent()">新增</vxe-button>
            <vxe-button @click="insertEvent(tableData[2])">在第3行插入并激活 Sex 单元格</vxe-button>
            <vxe-button @click="insertEvent(-1)">在最后行插入</vxe-button>
            <vxe-button @click="$refs.xTable.removeSelecteds()">删除选中</vxe-button>
            <vxe-button icon="fa fa-save" @click="getInsertEvent">保存</vxe-button>
          </template>
        </vxe-toolbar>

        <vxe-table
          border
          show-overflow
          ref="xTable"
          max-height="400"
          :data="tableData"
          :edit-config="{trigger: 'click', mode: 'cell', icon: 'fa fa-pencil'}">
          <vxe-table-column type="checkbox" width="60"></vxe-table-column>
          <vxe-table-column type="index" width="60"></vxe-table-column>
          <vxe-table-column field="name" title="Name" sortable :edit-render="{name: 'input', defaultValue: '默认的名字'}"></vxe-table-column>
          <vxe-table-column field="sex" title="Sex" :edit-render="{name: 'input'}"></vxe-table-column>
          <vxe-table-column field="age" title="Age" sortable :edit-render="{name: 'input', defaultValue: 18}"></vxe-table-column>
        </vxe-table>
        `,
        `
        export default {
          data () {
            return {
              tableData: [],
              sexList: []
            }
          },
          created () {
            this.tableData = window.MOCK_DATA_LIST.slice(0, 4)
            this.findSexList()
          },
          methods: {
            async findSexList () {
              this.sexList = await this.$ajax.getJSON('/api/conf/sex/list')
            },
            async insertEvent (row) {
              let record = {
                sex: '1'
              }
              await this.$refs.xTable.insertAt(record, row)
              await this.$refs.xTable.setActiveCell(row, 'sex')
            },
            getInsertEvent () {
              let insertRecords = this.$refs.xTable.getInsertRecords()
              this.$XModal.alert(insertRecords.length)
            }
          }
        }
        `
      ]
    }
  },
  created () {
    this.tableData = window.MOCK_DATA_LIST.slice(0, 4)
    this.findSexList()
  },
  mounted () {
    Array.from(this.$el.querySelectorAll('pre code')).forEach((block) => {
      hljs.highlightBlock(block)
    })
  },
  methods: {
    async findSexList () {
      this.sexList = await this.$ajax.getJSON('/api/conf/sex/list')
    },
    async insertEvent (row) {
      let record = {
        sex: '1'
      }
      await this.$refs.xTable.insertAt(record, row)
      await this.$refs.xTable.setActiveCell(row, 'sex')
    },
    getInsertEvent () {
      let insertRecords = this.$refs.xTable.getInsertRecords()
      this.$XModal.alert(insertRecords.length)
    }
  }
}
</script>
