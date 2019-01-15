<template>
    <div>
        <a-affix :offsetTop="10">
            <a-alert type="warning" showIcon description="您可以在本功能页面建立区域、建筑、楼层的层级归属关联关系，实现对园区的层级划分和分区管理。本功能的层级可选类型为区域、建筑、楼层。建议从高到低逐层建立上下级的从属关系。根据实际管理需求可建立一至三级，最多支持建立三级的管理层级。" message="温馨提醒" />
            <div style="margin-top:5px;"></div>
            <a-alert showIcon="" message="已选区域：XX楼XX栋XX层" type="info"></a-alert>
        </a-affix>
        <div style="margin-top:5px;"></div>
        <a-row :gutter="24">
          <a-col :span="6">
            <a-card title="区域" :bordered="false">
                <a slot="extra">
                    <a-avatar src="https://zos.alipayobjects.com/rmsportal/ODTLcjxAfvqbxHnVXCYX.png"></a-avatar>
                </a>
                <a-input-search
                    placeholder="请输入区域名"
                    @search="onSearch"
                    enterButton
                />
                <template class="ant-card-actions" slot="actions">
                    <span @click="openDialog('area')">新增</span>
                    <span>清空</span>
                </template>
                <a-list
                    class="demo-loadmore-list"
                    :loading="loading"
                    itemLayout="horizontal"
                    :dataSource="data"
                >
                    <div v-if="showLoadingMore" slot="loadMore" :style="{ textAlign: 'center', marginTop: '12px', height: '32px', lineHeight: '32px' }">
                        <a-spin v-if="loadingMore" />
                        <a-button v-else @click="onLoadMore">加载更多</a-button>
                    </div>
                    <a-list-item slot="renderItem" slot-scope="item, index">
                        <a slot="actions"><a-icon @click="openDialog('area')" type="eye"></a-icon></a>
                        <a slot="actions"><a-icon @click="openDialog('area')" type="edit"></a-icon></a>
                        <a slot="actions"><a-icon @click="showDeleteDialog" type="delete"></a-icon></a>
                        <a-list-item-meta
                            description="区域描述"
                        >
                            <a slot="title" href="https://vuecomponent.github.io/ant-design-vue/">{{item.name.last}}</a>
                        </a-list-item-meta>
                    </a-list-item>
                </a-list>
            </a-card>
          </a-col>
          <a-col :span="6">
            <a-card title="建筑" :bordered="false">
                <a slot="extra">
                    <a-avatar src="https://zos.alipayobjects.com/rmsportal/ODTLcjxAfvqbxHnVXCYX.png"></a-avatar>
                </a>
                <a-input-search
                    placeholder="请输入建筑名"
                    @search="onSearch"
                    enterButton
                />
                <template class="ant-card-actions" slot="actions">
                    <span @click="openDialog('area')">新增</span>
                    <span>清空</span>
                </template>
                <a-list
                    class="demo-loadmore-list"
                    :loading="loading"
                    itemLayout="horizontal"
                    :dataSource="data"
                >
                    <div v-if="showLoadingMore" slot="loadMore" :style="{ textAlign: 'center', marginTop: '12px', height: '32px', lineHeight: '32px' }">
                        <a-spin v-if="loadingMore" />
                        <a-button v-else @click="onLoadMore">加载更多</a-button>
                    </div>
                    <a-list-item slot="renderItem" slot-scope="item, index">
                        <a slot="actions"><a-icon @click="openDialog('area')" type="eye"></a-icon></a>
                        <a slot="actions"><a-icon @click="openDialog('area')" type="edit"></a-icon></a>
                        <a slot="actions"><a-icon @click="showDeleteDialog" type="delete"></a-icon></a>
                        <a-list-item-meta
                            description="建筑描述"
                        >
                            <a slot="title" href="https://vuecomponent.github.io/ant-design-vue/">{{item.name.last}}</a>
                        </a-list-item-meta>
                    </a-list-item>
                </a-list>
            </a-card>
          </a-col>
          <a-col :span="6">
            <a-card title="楼层" :bordered="false">
                <a slot="extra">
                    <a-avatar src="https://zos.alipayobjects.com/rmsportal/ODTLcjxAfvqbxHnVXCYX.png"></a-avatar>
                </a>
                <a-input-search
                    placeholder="请输入楼层"
                    @search="onSearch"
                    enterButton
                />
                <template class="ant-card-actions" slot="actions">
                    <span @click="openDialog('area')">新增</span>
                    <span>清空</span>
                </template>
                <a-list
                    class="demo-loadmore-list"
                    :loading="loading"
                    itemLayout="horizontal"
                    :dataSource="data"
                >
                    <div v-if="showLoadingMore" slot="loadMore" :style="{ textAlign: 'center', marginTop: '12px', height: '32px', lineHeight: '32px' }">
                        <a-spin v-if="loadingMore" />
                        <a-button v-else @click="onLoadMore">加载更多</a-button>
                    </div>
                    <a-list-item slot="renderItem" slot-scope="item, index">
                        <a slot="actions"><a-icon @click="openDialog('area')" type="eye"></a-icon></a>
                        <a slot="actions"><a-icon @click="openDialog('area')" type="edit"></a-icon></a>
                        <a slot="actions"><a-icon @click="showDeleteDialog" type="delete"></a-icon></a>
                        <a-list-item-meta
                            description="楼层描述"
                        >
                            <a slot="title" href="https://vuecomponent.github.io/ant-design-vue/">{{item.name.last}}</a>
                        </a-list-item-meta>
                    </a-list-item>
                </a-list>
            </a-card>
          </a-col>
          <a-col :span="6">
            <a-card title="房间" :bordered="false">
                <a slot="extra">
                    <a-avatar src="https://zos.alipayobjects.com/rmsportal/ODTLcjxAfvqbxHnVXCYX.png"></a-avatar>
                </a>
                <a-input-search
                    placeholder="请输入房间名"
                    @search="onSearch"
                    enterButton
                />
                <template class="ant-card-actions" slot="actions">
                    <span @click="openDialog('area')">新增</span>
                    <span>清空</span>
                </template>
                <a-list
                    class="demo-loadmore-list"
                    :loading="loading"
                    itemLayout="horizontal"
                    :dataSource="data"
                >
                    <div v-if="showLoadingMore" slot="loadMore" :style="{ textAlign: 'center', marginTop: '12px', height: '32px', lineHeight: '32px' }">
                        <a-spin v-if="loadingMore" />
                        <a-button v-else @click="onLoadMore">加载更多</a-button>
                    </div>
                    <a-list-item slot="renderItem" slot-scope="item, index">
                        <a slot="actions"><a-icon @click="openDialog('area')" type="eye"></a-icon></a>
                        <a slot="actions"><a-icon @click="openDialog('area')" type="edit"></a-icon></a>
                        <a slot="actions"><a-icon @click="showDeleteDialog" type="delete"></a-icon></a>
                        <a-list-item-meta
                            description="房间描述"
                        >
                            <a slot="title" href="https://vuecomponent.github.io/ant-design-vue/">{{item.name.last}}</a>
                        </a-list-item-meta>
                    </a-list-item>
                </a-list>
            </a-card>
          </a-col>
        </a-row>
        <a-modal :visible="dialogVisible" :title="dialogTitle" width="650px" @ok="handleDialog()" @cancel="handleDialog()">
            <a-alert style="margin-bottom:10px;" message="京基大厦" type="success"></a-alert>
            <a-form>
              <a-row>
                <a-col class="ant-form-item-label" style="right:5px;" :span="4">区域名称：</a-col>
                <a-col :span="20">
                    <a-form-item>
                        <a-input placeholder="请填写名称"></a-input>
                    </a-form-item>
                </a-col>
                <a-col class="ant-form-item-label" style="right:5px;" :span="4">区域类型：</a-col>
                <a-col :span="8">
                    <a-form-item>
                        <a-select style="width:100%;" placeholder="请选择区域类型">
                          <a-select-option key="1" value="1">区域类型1</a-select-option>
                          <a-select-option key="2" value="2">区域类型2</a-select-option>
                        </a-select>
                    </a-form-item>
                </a-col>
                <a-col class="ant-form-item-label" style="right:5px;" :span="4">区域状态：</a-col>
                <a-col :span="8">
                    <a-form-item>
                        <a-select style="width:100%;" placeholder="请选择区域状态">
                          <a-select-option key="1" value="1">区域状态1</a-select-option>
                          <a-select-option key="2" value="2">区域状态2</a-select-option>
                        </a-select>
                    </a-form-item>
                </a-col>
                <a-col class="ant-form-item-label" style="right:5px;" :span="4">负责人姓名：</a-col>
                <a-col :span="8">
                    <a-form-item>
                        <a-input placeholder="请填写负责人姓名"></a-input>
                    </a-form-item>
                </a-col>
                <a-col class="ant-form-item-label" style="right:5px;" :span="4">负责人电话：</a-col>
                <a-col :span="8">
                    <a-form-item>
                        <a-input placeholder="请填写负责人电话"></a-input>
                    </a-form-item>
                </a-col>
                <a-col class="ant-form-item-label" style="right:5px;" :span="4">区域说明：</a-col>
                <a-col :span="20">
                    <a-form-item>
                        <a-textarea placeholder="请填写区域说明" :autosize="{minRows: 6, maxRows: 6}"></a-textarea>
                    </a-form-item>
                </a-col>
              </a-row>
            </a-form>
        </a-modal>
    </div>
</template>

<script>
import reqwest from 'reqwest'

const fakeDataUrl = 'https://randomuser.me/api/?results=5&inc=name,gender,email,nat&noinfo'

export default {
  data () {
    return {
      loading: true,
      loadingMore: false,
      showLoadingMore: true,
      data: [],
      dialogVisible: false,
      dialogTitle: '',
      area:{
          parentName: '一号楼'
      }
    }
  },
  mounted () {
    this.getData((res) => {
      this.loading = false
      this.data = res.results
    })
  },
  methods: {
    getData  (callback) {
      reqwest({
        url: fakeDataUrl,
        type: 'json',
        method: 'get',
        contentType: 'application/json',
        success: (res) => {
          callback(res)
        },
      })
    },
    openDialog(title) {
        if(title === 'area') this.dialogTitle = '区域'
        this.dialogVisible = true
    },
    handleDialog() {
        this.dialogVisible = false
    },
    onLoadMore () {
      this.loadingMore = true
      this.getData((res) => {
        this.data = this.data.concat(res.results)
        this.loadingMore = false
        this.$nextTick(() => {
          window.dispatchEvent(new Event('resize'))
        })
      })
    },
    onSearch() {
        console.log(1)
    },
    showDeleteDialog() {
      console.log(1)
      this.$confirm({
        title: '你希望删除此数据吗?',
        onOk() {
          return new Promise((resolve, reject) => {
            setTimeout(Math.random() > 0.5 ? resolve : reject, 1000);
          }).catch(() => console.log('Oops errors!'));
        },
        onCancel() {},
      });
    }
  },
}
</script>

<style>
.demo-loadmore-list {
  min-height: 350px;
}
</style>