<template>
  <div>
    <a-card title="搜索条件" :bordered="false">
      <div class="table-page-search-wrapper">
        <a-form>
          <a-row>
            <a-col class="ant-form-item-label" style="right:5px;" :span="2">园区名称：</a-col>
            <a-col :span="4">
              <a-form-item>
                <a-input placeholder="请输入园区名称"></a-input>
              </a-form-item>
            </a-col>
            <a-col class="ant-form-item-label" style="right:5px;" :span="3">负责人姓名：</a-col>
            <a-col :span="4">
              <a-form-item>
                <a-input placeholder="请输入负责人姓名"></a-input>
              </a-form-item>
            </a-col>
            <a-col class="ant-form-item-label" style="right:5px;" :span="3">使用状态：</a-col>
            <a-col :span="3">
              <a-form-item>
                <a-select v-model="queryParam.status" placeholder="请选择" default-value="0">
                  <a-select-option value="0">全部</a-select-option>
                  <a-select-option value="1">运行中</a-select-option>
                  <a-select-option value="2">关闭</a-select-option>
                </a-select>
              </a-form-item>
            </a-col>
          </a-row>
          <a-row>
            <a-col :span="24">
              <a-button  type="primary" @click="$refs.table.refresh()">查询</a-button>
              <a-button style="margin-left: 8px" @click="() => queryParam = {}">重置</a-button>
            </a-col>
          </a-row>
        </a-form>
      </div>
    </a-card>

    <div style="margin-bottom:10px;"></div>

    <a-card title="搜索列表">
      <a slot="extra">
        <a-button @click="openParkingDialog()" type="primary" icon="plus">新建</a-button>
        <a-dropdown v-if="selectedRowKeys.length > 0">
          <a-menu slot="overlay">
            <a-menu-item key="1"><a-icon type="delete" />删除</a-menu-item>
            <!-- lock | unlock -->
            <a-menu-item key="2"><a-icon type="lock" />锁定</a-menu-item>
          </a-menu>
          <a-button style="margin-left: 8px">
            批量操作 <a-icon type="down" />
          </a-button>
        </a-dropdown>
      </a>
      <s-table
        ref="table"
        size="default"
        :columns="columns"
        :data="loadData"
        :showAlertInfo="true"
        @onSelect="onChange"
      >
        <span slot="action" slot-scope="text, record">
          <template v-if="$auth('table.update')">
            <a @click="handleEdit(record)">编辑</a>
            <a-divider type="vertical" />
          </template>
          <a-dropdown>
            <a class="ant-dropdown-link">
              更多 <a-icon type="down" />
            </a>
            <a-menu slot="overlay">
              <a-menu-item>
                <a href="javascript:;">详情</a>
              </a-menu-item>
              <a-menu-item>
                <a href="javascript:;">禁用</a>
              </a-menu-item>
              <a-menu-item>
                <a href="javascript:;">删除</a>
              </a-menu-item>
            </a-menu>
          </a-dropdown>
        </span>
      </s-table>
    </a-card>

    <a-modal class="parkingDialog" :visible="parkingDialogVisible" title="园区" @ok="handleParkingDialog()" @cancel="handleParkingDialog()">

    </a-modal>
  </div>
</template>

<script>
  import STable from '@/components/table/'
  import ATextarea from 'ant-design-vue/es/input/TextArea'
  import AInput from 'ant-design-vue/es/input/Input'
  import moment from 'moment'

  import { getRoleList, getServiceList } from '@/api/manage'

  export default {
    name: 'TableList',
    components: {
      AInput,
      ATextarea,
      STable
    },
    data () {
      return {
        visible: false,
        parkingDialogVisible: false,
        labelCol: {
          xs: { span: 24 },
          sm: { span: 5 },
        },
        wrapperCol: {
          xs: { span: 24 },
          sm: { span: 12 },
        },
        form: null,
        mdl: {},

        // 高级搜索 展开/关闭
        advanced: true,
        // 查询参数
        queryParam: {},
        // 表头
        columns: [
          {
            title: '序号',
            dataIndex: 'no'
          },
          {
            title: '园区名称',
            dataIndex: 'description'
          },
          {
            title: '园区类型',
            dataIndex: 'callNo',
            sorter: true,
            customRender: (text) => text + ' 次'
          },
          {
            title: '园区地址',
            dataIndex: 'address'
          },
          {
            title: '负责人姓名',
            dataIndex: 'name',
            sorter: true
          },
          {
            title: '负责人手机',
            dataIndex: 'phone',
            sorter: true
          },
          {
            title: '状态',
            dataIndex: 'status',
            sorter: true
          },
          {
            title: '操作',
            dataIndex: 'action',
            width: '150px',
            scopedSlots: { customRender: 'action' },
          }
        ],
        // 加载数据方法 必须为 Promise 对象
        loadData: parameter => {
          return getServiceList(Object.assign(parameter, this.queryParam))
            .then(res => {
              return res.result
            })
        },

        selectedRowKeys: [],
        selectedRows: []
      }
    },
    created () {
      getRoleList({ t: new Date()})
    },
    methods: {
      handleEdit (record) {
        this.mdl = Object.assign({}, record)
        console.log(this.mdl)
        this.visible = true
      },
      handleOk () {

      },
      onChange (row) {
        this.selectedRowKeys = row.selectedRowKeys
        this.selectedRows = row.selectedRows

        console.log(this.$refs.table)
      },
      toggleAdvanced () {
        this.advanced = !this.advanced
      },

      resetSearchForm () {
        this.queryParam = {
          date: moment(new Date())
        }
      },
      openParkingDialog() {
        this.parkingDialogVisible = true
      },
      handleParkingDialog() {
        this.parkingDialogVisible = false
      }
    },
    watch: {
      /*
      'selectedRows': function (selectedRows) {
        this.needTotalList = this.needTotalList.map(item => {
          return {
            ...item,
            total: selectedRows.reduce( (sum, val) => {
              return sum + val[item.dataIndex]
            }, 0)
          }
        })
      }
      */
    }
  }
</script>

<style>
  .parkingDialog .ant-modal-footer {
    display: none
  }
</style>