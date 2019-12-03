<template>
  <div class="left-panel">
    <div class="title">组件库</div>
    <div class="list">
      <div v-for="(item, index) in list" :key="index" class="item">
        <h4>{{ item.title }}</h4>
        <ul>
          <li v-for="(component, idx) in item.list" :key="idx" @click="$emit('addComponent', component)">
            <span :class="['iconfont', `icon-${component.icon}`]" />
            <span>{{ component.name }}</span>
          </li>
        </ul>
      </div>
    </div>
    <div class="footer">
      <el-button type="danger" size="mini" @click="saveSetting">保存页面</el-button>
    </div>
  </div>
</template>
<script>
export default {
  name: 'LeftControl',
  props: {
    list: {
      type: Array,
      default: () => []
    },
    saveData: {
      type: Object,
      required: true
    }
  },
  methods: {
    saveSetting() {
      const h = this.$createElement
      this.$msgbox({
        title: '保存数据',
        message: h('pre', null, JSON.stringify(this.saveData, null, 4)),
        showCancelButton: true,
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        beforeClose: (action, instance, done) => {
          if (action === 'confirm') {
            instance.confirmButtonLoading = true
            instance.confirmButtonText = '执行中...'
            setTimeout(() => {
              done()
              setTimeout(() => {
                instance.confirmButtonLoading = false
              }, 300)
            }, 3000)
          } else {
            done()
          }
        }
      }).then(action => {
        this.$message({
          type: 'info',
          message: `action:${action}`
        })
      })
    }
  }
}
</script>
<style lang="scss">
.left-panel {
    width: 290px;
    box-sizing: border-box;
    border: 1px solid #ddd;
    padding: 15px;
    margin-right: 100px;
    .title {
        border-bottom: 1px solid #eef1f5;
        font-size: 14px;
        position: relative;
        height: 30px;
        padding: 0 22px;
        line-height: 30px;
        color:#333;
        &::after {
            content: '';
            position: absolute;
            width: 4px;
            height: 13px;
            background: #00aeff;
            top: 8px;
            left: 9px;
        }
    }
    .footer {
      display: flex;
      justify-content: flex-end;
      padding: 10px 0 0;
      margin-top: 10px;
      border-top: 1px solid #eef1f5
    }
    .list {
        .item {
            h4 {
                font-size: 12px;
                font-weight: normal;
                color: #999;
                margin: 15px 0 10px;
            }
            ul {
                padding: 0;
                margin: 0;
                list-style: none;
                display: flex;
                flex-wrap: wrap;
                li {
                    border: 1px solid #dddddd;
                    margin: 5px;
                    display: flex;
                    flex-direction: column;
                    align-items: center;
                    justify-content: center;
                    width: 75px;
                    height: 70px;
                    background-color:#f4f4f4;
                    cursor: pointer;
                    transition: All 0.3s ease-in-out;
                    &:hover {
                        background: #fff;
                        border: 1px solid #00aeff;
                        color: #00aeff;
                        transition: All 0.4s;
                    }
                    &:active {
                        box-shadow: inset 1px 3px 5px rgba(0, 0, 0, 0.2);
                    }
                    span {
                        &:first-child {
                            font-size: 20px;
                            color:#999
                        }
                        &:last-of-type {
                            padding-top: 5px;
                            color: #424242;
                            font-size: 12px;
                        }
                    }
                }
            }
        }
    }
}
</style>
