<template>
  <div class="padding-sm flex justify-between align-start">
    <LeftPanel :list="allComponent" :save-data="list" @add-component="addComponent" />
    <CenterPanel :list="list" @onDel="delComponent" />
    <RightPanel :list="list" @on-del-list="delListItem" @on-add-list="addListItem" />
  </div>
</template>

<script>
import { RightPanel, LeftPanel, CenterPanel } from './components'

/*
  name: '组件名称'
  componentName: 加载的组件名称，
  icon: '图标
  data: {
    duration: 设置切换时间
    list: 图片列表 ， 比如组件 轮播图 图片橱窗 单图组
    padtb: 上下边距
    padlf: 左右边距
    backgroundColor: 背景颜色
    layout: {
      type: 1, 布局类型
      name: 显示名称
      list: 布局列表
    }
  }
*/
const listComponent = [ // 左边组件库
  { title: '媒体组件', list: [
    { name: '轮播图', componentName: 'Cswiper', icon: 'tupianlunbo', data: { duration: 5000, list: [{ link: '', image: '' }, { link: '', image: '' }] }},
    { name: '单图组', componentName: 'PicGroup', icon: 'tupian', data: { list: [{ link: '', image: '' }], padtb: 0, padlf: 0, backgroundColor: '#fff' }},
    { name: '图片橱窗', componentName: 'ShowCase', icon: 'newbilayout', data: { list: [{ link: '', image: '' }, { link: '', image: '' }, { link: '', image: '' }, { link: '', image: '' }], padtb: 0, padlf: 0, backgroundColor: '#fff', layout: { type: 1, name: '布局方式', list: [{ name: '两列', value: 1 }, { name: '三列', value: 2 }, { name: '四列', value: 3 }, { name: '橱窗', value: 4 }] }}},
    { name: '视频', componentName: 'ShowVideo', icon: 'shipin', data: { padtb: 0, padlf: 0, type: 1, video: { cover: '', src: '', auto: true, loop: true }}},
    { name: '文章组', componentName: 'AirtcleList', icon: 'zixun', data: { padtb: 0, padlf: 0, backgroundColor: '#fff' }},
    { name: '头条快报', componentName: 'TouTiao', icon: 'toutiao', data: { padtb: 0, padlf: 0, backgroundColor: '#fff' }}
  ] },
  { title: '商城组件', list: [
    { name: '搜索框', componentName: 'Search', icon: 'wxbsousuotuiguang', data: {}},
    { name: '公告组', componentName: 'Notice', icon: 'icon100', data: {}},
    { name: '导航组', componentName: 'Navigator', icon: 'daohang', data: {}},
    { name: '商品组', componentName: 'ShopPing', icon: 'shangpin', data: {}},
    { name: '优惠券组', componentName: 'Coupon', icon: 'yhq', data: {}},
    { name: '拼团商品', componentName: 'PingShop', icon: 'shangpin', data: {}},
    { name: '砍价商品', componentName: 'KanJia', icon: 'kanjia_', data: {}},
    { name: '线下门店', componentName: 'MenDian', icon: 'mendian', data: {}}
  ] },
  { title: '工具组件', list: [
    { name: '在线客服', componentName: 'Kefu', icon: 'kefu', data: {}},
    { name: '富文本', componentName: 'Empty', icon: 'wenbenbianji', data: {}},
    { name: '辅助空白', componentName: 'Fuwenben', icon: 'kongbai', data: {}},
    { name: '辅组线', componentName: 'Fuzhuxian', icon: 'fengexian', data: {}}
  ] }
]
const component = {
  page: { // 页面配置
    name: '页面设置',
    pageName: '页面名称',
    pageTitle: '页面标题',
    shareTitle: '分享标题',
    textColor: 'black',
    backgroundColor: '#fff'
  },
  component: []
}

export default {
  name: 'Mini',
  components: { RightPanel, CenterPanel, LeftPanel },
  data() {
    return {
      list: component, // 页面展示组件
      allComponent: listComponent // 组件库
    }
  },
  methods: {
    addComponent(component) { // 添加展示组件
      const obj = JSON.parse(JSON.stringify(component))
      const element = { ...obj, selected: false }
      this.list.component.push(element)
    },
    addListItem(component) { // 新增组件中的图片
      this.list.component = this.list.component.map(item => {
        if (item.componentName === component.componentName && item.selected) {
          item.data.list.push({ image: '', link: '' })
        }
        return item
      })
    },
    delListItem(component, index) { // 删除组件中图片
      this.list.component = this.list.component.map(item => {
        if (item.componentName === component.componentName && item.selected) {
          item.data.list = item.data.list.filter((_, idx) => idx !== index)
        }
        return item
      })
    },
    delComponent(idx) { // 删除展示组件
      this.list.component = this.list.component.filter((_, index) => index !== idx)
      this.$message.success('删除成功')
    }
  }
}
</script>

