<template>
    <div>
        <a-alert message="您可以在本功能页面建立区域、建筑、楼层的层级归属关联关系，实现对园区的层级划分和分区管理。本功能的层级可选类型为区域、建筑、楼层。建议从高到低逐层建立上下级的从属关系。根据实际管理需求可建立一至三级，最多支持建立三级的管理层级。" type="success" />
        <div style="margin-top:5px;"></div>
        <a-row :gutter="24">
          <a-col :span="8">
            <a-card title="区域" :bordered="false">
                <template class="ant-card-actions" slot="actions">
                    <span>新增</span>
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
                        <a-button v-else @click="onLoadMore">loading more</a-button>
                    </div>
                    <a-list-item slot="renderItem" slot-scope="item, index">
                        <a slot="actions">edit</a>
                        <a slot="actions">more</a>
                        <a-list-item-meta
                            description="区域描述"
                        >
                            <a slot="title" href="https://vuecomponent.github.io/ant-design-vue/">{{item.name.last}}</a>
                            <a-avatar slot="avatar" src="https://zos.alipayobjects.com/rmsportal/ODTLcjxAfvqbxHnVXCYX.png" />
                        </a-list-item-meta>
                    </a-list-item>
                </a-list>
            </a-card>
          </a-col>
          <a-col :span="8">
            <a-card title="建筑" :bordered="false">
                <template class="ant-card-actions" slot="actions">
                    <span>新增</span>
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
                        <a-button v-else @click="onLoadMore">loading more</a-button>
                    </div>
                    <a-list-item slot="renderItem" slot-scope="item, index">
                        <a slot="actions">edit</a>
                        <a slot="actions">more</a>
                        <a-list-item-meta
                            description="建筑描述"
                        >
                            <a slot="title" href="https://vuecomponent.github.io/ant-design-vue/">{{item.name.last}}</a>
                            <a-avatar slot="avatar" src="https://zos.alipayobjects.com/rmsportal/ODTLcjxAfvqbxHnVXCYX.png" />
                        </a-list-item-meta>
                    </a-list-item>
                </a-list>
            </a-card>
          </a-col>
          <a-col :span="8">
            <a-card title="楼层" :bordered="false">
                <template class="ant-card-actions" slot="actions">
                    <span>新增</span>
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
                        <a-button v-else @click="onLoadMore">loading more</a-button>
                    </div>
                    <a-list-item slot="renderItem" slot-scope="item, index">
                        <a slot="actions">edit</a>
                        <a slot="actions">more</a>
                        <a-list-item-meta
                            description="楼层描述"
                        >
                            <a slot="title" href="https://vuecomponent.github.io/ant-design-vue/">{{item.name.last}}</a>
                            <a-avatar slot="avatar" src="https://zos.alipayobjects.com/rmsportal/ODTLcjxAfvqbxHnVXCYX.png" />
                        </a-list-item-meta>
                    </a-list-item>
                </a-list>
            </a-card>
          </a-col>
        </a-row>
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
  },
}
</script>

<style>
.demo-loadmore-list {
  min-height: 350px;
}
</style>