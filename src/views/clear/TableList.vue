<template>
  <a-card :bordered="false">
    <a-tabs type="card">
      <a-tab-pane tab="普通文本" key="1">普通文本</a-tab-pane>
      <a-tab-pane tab="用户资料文本" key="2">用户资料文本</a-tab-pane>
      <a-tab-pane tab="普通图片" key="3">普通图片</a-tab-pane>
      <a-tab-pane tab="用户资料图片" key="4">用户资料图片</a-tab-pane>
    </a-tabs>
    <div class="table-page-search-wrapper filter-search-wrap">
      <a-form layout="inline">
        <a-row :gutter="48">
          <a-col :md="8" :sm="24">
            <a-form-item label="业务名称">
              <a-select v-model="queryParam.name" placeholder="请选择">
                <a-select-option value="0">普通文本</a-select-option>
              </a-select>
            </a-form-item>
          </a-col>
          <a-col :md="8" :sm="24">
            <a-form-item label="回调状态">
              <a-select v-model="queryParam.cb" placeholder="请选择">
                <a-select-option value="0">所有</a-select-option>
                <a-select-option value="1">不需要回调</a-select-option>
                <a-select-option value="2">等待回调</a-select-option>
                <a-select-option value="3">已回调</a-select-option>
              </a-select>
            </a-form-item>
          </a-col>
          <a-col :md="8" :sm="24">
            <a-form-item label="数据状态">
              <a-select v-model="queryParam.data" placeholder="请选择">
                <a-select-option value="0">所有</a-select-option>
                <a-select-option value="1">通过</a-select-option>
                <a-select-option value="2">嫌疑</a-select-option>
                <a-select-option value="3">不通过</a-select-option>
              </a-select>
            </a-form-item>
          </a-col>
          <a-col :md="8" :sm="24">
            <a-form-item label="命中原因">
              <a-select v-model="queryParam.hit" placeholder="请选择">
                <a-select-option value="0">所有</a-select-option>
                <a-select-option value="1">正常</a-select-option>
                <a-select-option value="2">色情</a-select-option>
                <a-select-option value="3">广告</a-select-option>
                <a-select-option value="4">暴恐</a-select-option>
                <a-select-option value="5">违禁</a-select-option>
                <a-select-option value="6">涉政</a-select-option>
                <a-select-option value="7">谩骂</a-select-option>
                <a-select-option value="8">灌水</a-select-option>
                <a-select-option value="9">用户账号黑名单</a-select-option>
                <a-select-option value="10">用户账号白名单</a-select-option>
                <a-select-option value="11">IP黑名单</a-select-option>
                <a-select-option value="12">IP白名单</a-select-option>
                <a-select-option value="13">IP地区限制</a-select-option>
                <a-select-option value="14">设备黑名单</a-select-option>
                <a-select-option value="15">IP地区限制</a-select-option>
                <a-select-option value="16">设备白名单</a-select-option>
                <a-select-option value="17">URL黑名单</a-select-option>
              </a-select>
            </a-form-item>
          </a-col>
          <a-col :md="8" :sm="24">
            <a-form-item label="内容检索">
              <a-input v-model="queryParam.retrieval" placeholder="请输入内容检索"/>
            </a-form-item>
          </a-col>
          
          <template v-if="advanced">
            <a-col :md="8" :sm="24">
              <a-form-item label="用户标识">
                <a-input v-model="queryParam.usermark" placeholder="请输入用户标识"/>
              </a-form-item>
            </a-col>
            <a-col :md="8" :sm="24">
              <a-form-item label="数据标识">
                <a-input v-model="queryParam.datamark" placeholder="请输入数据标识"/>
              </a-form-item>
            </a-col>
            <a-col :md="8" :sm="24">
              <a-form-item label="taskID">
                <a-input v-model="queryParam.taskid" placeholder="请输入taskID"/>
              </a-form-item>
            </a-col>
            <a-col :md="8" :sm="24">
              <a-form-item label="IP地址">
                <a-input v-model="queryParam.ipaddress" placeholder="请输入IP地址"/>
              </a-form-item>
            </a-col>
            <a-col :md="8" :sm="24">
              <a-form-item label="数据类型">
                <a-select v-model="queryParam.datatype" placeholder="请选择">
                  <a-select-option value="0">所有</a-select-option>
                </a-select>
              </a-form-item>
            </a-col>
            <a-col :md="12" :sm="24">
              <a-form-item label="时间范围">
                <a-date-picker
                  :disabledDate="disabledStartDate"
                  showTime
                  format="YYYY-MM-DD HH:mm:ss"
                  v-model="queryParam.startValue"
                  @openChange="handleStartOpenChange"
                />
                -
                <a-date-picker
                  :disabledDate="disabledEndDate"
                  showTime
                  format="YYYY-MM-DD HH:mm:ss"
                  v-model="queryParam.endValue"
                  :open="endOpen"
                  @openChange="handleEndOpenChange"
                />
              </a-form-item>
            </a-col>
          </template>
          <a-col :md="!advanced && 24 || 24" :sm="24">
            <span class="table-page-search-submitButtons" :style="advanced && { float: 'left', overflow: 'hidden' } || {} ">
              <a @click="toggleAdvanced" style="margin-left: 8px">
                {{ advanced ? '收起' : '展开' }}
                <a-icon :type="advanced ? 'up' : 'down'"/>
              </a>
            </span>
          </a-col>
          <a-col :md="!advanced && 24 || 24" :sm="24">
            <span class="table-page-search-submitButtons" :style="advanced && { float: 'left', overflow: 'hidden' } || {} ">
              <a-button type="primary" @click="$refs.table.refresh(true)">查询</a-button>
            </span>
          </a-col>
        </a-row>
      </a-form>
    </div>

    <s-table
      ref="table"
      size="default"
      rowKey="key"
      :columns="columns"
      :data="loadData"
      :alert="options.alert"
      :rowSelection="options.rowSelection"
    >
      <span slot="serial" slot-scope="text, record, index">
        {{ index + 1 }}
      </span>
      <span slot="status" slot-scope="text">
        <a-badge :status="text | statusTypeFilter" :text="text | statusFilter" />
      </span>

      <span slot="action" slot-scope="text, record">
        <template>
          <a @click="handleEdit(record)">配置</a>
          <a-divider type="vertical" />
          <a @click="handleSub(record)">订阅报警</a>
        </template>
      </span>
    </s-table>
    <create-form ref="createModal" @ok="handleOk" />
    <step-by-step-modal ref="modal" @ok="handleOk"/>
  </a-card>
</template>

<script>
import moment from 'moment'
import { STable } from '@/components'
import StepByStepModal from './modules/StepByStepModal'
import CreateForm from './modules/CreateForm'
import { getRoleList, getServiceList } from '@/api/manage'
const statusMap = {
  0: {
    status: 'default',
    text: '关闭'
  },
  1: {
    status: 'processing',
    text: '运行中'
  },
  2: {
    status: 'success',
    text: '已上线'
  },
  3: {
    status: 'error',
    text: '异常'
  }
}

export default {
  name: 'TableList',
  components: {
    STable,
    CreateForm,
    StepByStepModal
  },
  data () {
    return {
      rangeConfig: {
        rules: [{ type: 'array', required: true, message: 'Please select time!' }],
      },
      mdl: {},
      // 高级搜索 展开/关闭
      advanced: false,
      endOpen: false,
      // 查询参数
      queryParam: {
        name: '0',
        cb: '0',
        data: '0',
        hit: '0',
        datatype: '0',
        startValue: moment(moment().format('YYYY-MM-DD 00:00:00')),
        endValue: moment(moment().format('YYYY-MM-DD 23:59:59'))
      },
      // 表头
      columns: [
        {
          title: '#',
          scopedSlots: { customRender: 'serial' }
        },
        {
          title: '规则编号',
          dataIndex: 'no'
        },
        {
          title: '描述',
          dataIndex: 'description'
        },
        {
          title: '服务调用次数',
          dataIndex: 'callNo',
          sorter: true,
          needTotal: true,
          customRender: (text) => text + ' 次'
        },
        {
          title: '状态',
          dataIndex: 'status',
          scopedSlots: { customRender: 'status' }
        },
        {
          title: '更新时间',
          dataIndex: 'updatedAt',
          sorter: true
        },
        {
          title: '操作',
          dataIndex: 'action',
          width: '150px',
          scopedSlots: { customRender: 'action' }
        }
      ],
      // 加载数据方法 必须为 Promise 对象
      loadData: parameter => {
        console.log('loadData.parameter', parameter)
        return getServiceList(Object.assign(parameter, this.queryParam))
          .then(res => {
            return res.result
          })
      },
      selectedRowKeys: [],
      selectedRows: [],

      // custom table alert & rowSelection
      options: {
        // alert: { show: true, clear: () => { this.selectedRowKeys = [] } },
        rowSelection: {
          selectedRowKeys: this.selectedRowKeys,
          onChange: this.onSelectChange
        }
      },
      // optionAlertShow: false
    }
  },
  watch: {
    startValue(val) {
      console.log('startValue', val)
    },
    endValue(val) {
      console.log('endValue', val)
    }
  },
  filters: {
    statusFilter (type) {
      return statusMap[type].text
    },
    statusTypeFilter (type) {
      return statusMap[type].status
    }
  },
  created () {
    this.tableOption()
    getRoleList({ t: new Date() })
  },
  methods: {
    tableOption () {
      if (!this.optionAlertShow) {
        this.options = {
          // alert: { show: true, clear: () => { this.selectedRowKeys = [] } },
          rowSelection: {
            selectedRowKeys: this.selectedRowKeys,
            onChange: this.onSelectChange
          }
        }
        // this.optionAlertShow = true
      } else {
        this.options = {
          alert: false,
          rowSelection: null
        }
        // this.optionAlertShow = false
      }
    },
    handleEdit (record) {
      console.log(record)
      this.$refs.modal.edit(record)
    },
    handleSub (record) {
      if (record.status !== 0) {
        this.$message.info(`${record.no} 订阅成功`)
      } else {
        this.$message.error(`${record.no} 订阅失败，规则已关闭`)
      }
    },
    handleOk () {
      this.$refs.table.refresh()
    },
    onSelectChange (selectedRowKeys, selectedRows) {
      this.selectedRowKeys = selectedRowKeys
      this.selectedRows = selectedRows
    },
    toggleAdvanced () {
      this.advanced = !this.advanced
    },
    resetSearchForm () {
      this.queryParam = {
        date: moment(new Date())
      }
    },
    disabledStartDate (startValue) {
      const endValue = this.endValue;
      if (!startValue || !endValue) {
        return false;
      }
      return startValue.valueOf() > endValue.valueOf();
    },
    disabledEndDate (endValue) {
      const startValue = this.startValue;
      if (!endValue || !startValue) {
        return false;
      }
      return startValue.valueOf() >= endValue.valueOf();
    },
    handleStartOpenChange (open) {
      if (!open) {
        this.endOpen = true;
      }
    },
    handleEndOpenChange (open) {
      this.endOpen = open;
    }
  }
}
</script>
<style lang="less" scoped>
.table-page-search-wrapper.filter-search-wrap /deep/ .ant-form-inline .ant-form-item,
.table-page-search-wrapper.filter-search-wrap /deep/ .table-page-search-submitButtons{
  margin-bottom: 10px;
}
</style>
