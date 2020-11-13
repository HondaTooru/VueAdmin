<template>
  <div class="phone flex flex-direction">
    <div :style="{ backgroundColor: page.backgroundColor }" :class="['top', { selected: page.selected }, page.textColor === 'white' ? 'white' : 'black']">
      <div class="title">{{ page.pageTitle }}</div>
    </div>
    <draggable
      v-model="list.component"
      class="drag-area"
      tag="transition-group"
      draggable=".item"
      :animation="200"
      @choose="handlerChoose"
      @end="$emit('input', [])"
    >
      <div v-for="(element, index) in list.component" :key="index" class="item">
        <div :class="['box',{ selected: element.selected }]"><component :is="element.componentName" :option="element.data" /></div>
        <div class="del"><el-button type="info" icon="el-icon-delete" size="mini" @click="delItem(index)">删除</el-button></div>
      </div>
    </draggable>
  </div>
</template>

<script>

import draggable from 'vuedraggable'
import { Cswiper, PicGroup, ShowCase, ShowVideo, AirtcleList, TouTiao, Search, Notice, Navigator, ShopPing, Coupon, PingShop, KanJia, MenDian, Kefu, Empty, Fuwenben, Fuzhuxian } from './child'
export default {
  components: {
    draggable,
    Cswiper,
    PicGroup,
    ShowCase,
    ShowVideo,
    AirtcleList,
    TouTiao,
    Search,
    Notice, Navigator, ShopPing, Coupon, PingShop, KanJia, MenDian, Kefu, Empty, Fuwenben, Fuzhuxian
  },
  props: {
    list: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      drag: false,
      clist: [],
      topHead: {},
      listCom: []
    }
  },
  computed: {
    page() {
      return this.list.page
    }
  },
  methods: {
    delItem(item) {
      this.$confirm('你确认删除吗？', '提示', {
        confirmButtonText: '确认',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(_ => {
        this.$emit('onDel', item)
      }).catch(_ => {})
    },
    handlerChoose(e) {
      this.list.component.forEach((item, index) => {
        if (index === e.oldIndex) {
          item.selected = !item.selected
        } else {
          item.selected = false
        }
      })
    }
  }
}
</script>

<style lang="scss">
.phone {
  width: 377px;
  border-radius: 3px;
  box-shadow: 0 3px 10px #dcdcdc;
  border: 1px solid #ddd;
  overflow: hidden;
  position: relative;
  z-index: 1;
  .top {
    position: relative;
    display: flex;
    height: 75px;
    padding-bottom: 15px;
    background:url(../../../assets/phone-top-black.png) center top/contain no-repeat;
    font-size: 14px;
    align-items: flex-end;
    justify-content: center;
    &.white {
      background: url(../../../assets/phone-top-white.png) center top/contain no-repeat;
        .title {
          color: white;
        }
    }
    .title {
      width: 70%;
      white-space: nowrap;
      text-overflow: ellipsis;
      overflow: hidden;
      text-align: center;
    }
    &.selected {
      &::after{
          content: '';
          top: 0;
          left: 0;
          position: absolute;
          width: 100%;
          height: 100%;
          border:2px dashed #3a8ee6;
          z-index: 99;
      }
    }
  }
  .drag-area {
    overflow-y: auto;
    height: 100%;
    min-height: 650px;
    max-height: 650px;
    width: 100%;
    list-style: none;
    margin: 0;
    padding: 0;
    &::-webkit-scrollbar {
      width: 5px;
      height: 1px;
    }
    &::-webkit-scrollbar-thumb {
      border-radius: 5px;
      box-shadow: inset 0 0 5px rgba(0, 0, 0, .5);
      background-color: #535353
    }
    .item {
      position: relative;
      .del {
        position: absolute;
        bottom: 10px;
        right: 10px;
        z-index: 99;
        display: none;
      }
      &:hover {
        .del {
          display: block
        }
      }
      &:hover>.box:not(.selected), &>.selected{
        &::after{
          content: '';
          top: 0;
          left: 0;
          cursor: move;
          position: absolute;
          width: 100%;
          height: 100%;
          border:2px dashed #3a8ee6;
          z-index: 99;
        }
      }
    }
  }
}
</style>
