<template>
  <div class="right-panel">
    <div class="title">{{ selected.name }}</div>
    <div class="setting">
      <el-form label-width="80px" class="page">
        <template v-if="selected.componentName === void 0">
          <el-form-item label="页面名称">
            <div class="icon">
              <el-input v-model="selected.pageName" placeholder="请输入页面名称" />
              <el-tooltip class="item" effect="dark" content="页面名称仅用于后台查找" placement="top-end">
                <span class="el-icon-question" />
              </el-tooltip>
            </div>
          </el-form-item>
          <el-form-item label="页面标题">
            <div class="icon">
              <el-input v-model="selected.pageTitle" placeholder="请输入页面标题" />
              <el-tooltip class="item" effect="dark" content="小程序端顶部显示的标题" placement="top-end">
                <span class="el-icon-question" />
              </el-tooltip>
            </div>
          </el-form-item>
          <el-form-item label="分享标题">
            <div class="icon">
              <el-input v-model="selected.shareTitle" placeholder="请输入分享标题" />
              <el-tooltip class="item" effect="dark" content="小程序端转发时显示的标题" placement="top-end">
                <span class="el-icon-question" />
              </el-tooltip>
            </div>
          </el-form-item>
          <el-form-item label="文字颜色">
            <el-radio-group v-model="selected.textColor">
              <el-radio label="black">黑色</el-radio>
              <el-radio label="white">白色</el-radio>
            </el-radio-group>
          </el-form-item>
          <el-form-item label="背景颜色">
            <el-color-picker v-model="selected.backgroundColor" @active-change="selected.backgroundColor = $event" />
          </el-form-item>
        </template>
        <template v-if="selected.data !== void 0">
          <template v-if="selected.data.padlf !== void 0">
            <el-form-item label="右边边距">
              <el-slider v-model="selected.data.padlf" :min="0" :max="50" />
            </el-form-item>
          </template>
          <template v-if="selected.data.padtb !== void 0">
            <el-form-item label="上下边距">
              <el-slider v-model="selected.data.padtb" :min="0" :max="50" />
            </el-form-item>
          </template>
          <template v-if="selected.data.backgroundColor !== void 0">
            <el-form-item label="背景颜色">
              <el-color-picker v-model="selected.data.backgroundColor" @active-change="selected.data.backgroundColor = $event" />
            </el-form-item>
          </template>
          <template v-if="selected.data.duration !== void 0">
            <el-form-item label="切换时间">
              <el-input v-model="selected.data.duration" placeholder="轮播图自动切换的间隔时间，单位：毫秒" />
            </el-form-item>
          </template>
          <template v-if="selected.data.video !== void 0">
            <el-form-item label="视频封面">
              <el-image :src="selected.data.video.cover || require('@/assets/cover.png')" fit="cover" />
              <el-upload
                action="https://jsonplaceholder.typicode.com/posts/"
                :limit="1"
              >
                <el-button icon="el-icon-upload" size="mini" plain type="primary">上传封面</el-button>
              </el-upload>
            </el-form-item>
            <el-form-item label="视频地址">
              <el-input />
            </el-form-item>
            <el-form-item label="自动播放">
              <el-radio-group v-model="selected.data.video.auto">
                <el-radio-button :label="true">是</el-radio-button>
                <el-radio-button :label="false">否</el-radio-button>
              </el-radio-group>
            </el-form-item>
          </template>
          <template v-if="selected.data.layout !== void 0">
            <el-form-item :label="selected.data.layout.name">
              <el-radio-group v-model="selected.data.layout.type">
                <el-radio-button v-for="(item, cvx) in selected.data.layout.list" :key="cvx" :label="item.value">{{ item.name }}</el-radio-button>
              </el-radio-group>
            </el-form-item>
          </template>
          <template v-if="selected.data.list !== void 0">
            <el-form-item label-width="0">
              <el-card v-for="(obj, v) in selected.data.list" :key="v" shadow="hover" class="margin-bottom-xs">
                <div slot="header" class="flex justify-between align-center">
                  <el-upload
                    action="https://jsonplaceholder.typicode.com/posts/"
                    :limit="1"
                  >
                    <el-button icon="el-icon-upload" circle size="mini" plain type="primary" />
                  </el-upload>
                  <el-button v-if="selected.data.list.length !== 1" type="danger" icon="el-icon-delete" circle size="mini" plain @click="$emit('on-del-list', selected, v)" />
                </div>
                <el-form-item label-width="60px" label="图片"><el-image :src="obj.image | img(v, selected.componentName)" class="cover" fit="cover" /></el-form-item>
                <el-form-item label-width="60px" label="地址">
                  <el-input v-model="obj.link" placeholder="请输入地址" />
                </el-form-item>
              </el-card>
            </el-form-item>
            <el-form-item label-width="0">
              <div class="flex"><el-button class="flex-sub" size="medium" type="danger" icon="el-icon-circle-plus-outline" @click="$emit('on-add-list', selected)">添加一个</el-button></div>
            </el-form-item>
          </template>
        </template>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Rightpanel',
  filters: {
    img(image, index, type) {
      const idx = index < 4 ? index + 1 : !(index % 4) ? 1 : index % 4 + 1
      return image || type === 'ShowCase' ? require(`@/assets/0${idx}.jpg`) : require(`@/assets/cover.png`)
    }
  },
  props: {
    list: {
      type: Object,
      required: true
    }
  },
  computed: {
    selected() {
      return this.list.component.find(item => item.selected) ? this.list.component.find(item => item.selected) : this.list.page
    },
    imgList() {
      if (this.selected.data) {
        if (this.selected.data.list instanceof Array) {
          return this.selected.data.list.map(item => item.image)
        }
        return []
      }
      return []
    }
  }
}
</script>

<style lang="scss">
.right-panel {
    width: 400px;
    box-sizing: border-box;
    border: 1px solid #ddd;
    padding: 15px;
    margin-right: 100px;
    position: relative;
    z-index: 1;
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
    .setting {
      padding: 15px 0;
      .page {
        .icon {
          display: flex;
          justify-content: center;
          align-items: center;
          span {
            font-size:16px;
            margin-left: 10px;
            color: #909090
          }
        }
        .imgCard {
          padding: 5px 20px;
        }
        .cover {
          width: 80px;
          height: 80px;
          display: block;
          margin-bottom: 10px;
        }
      }
    }
}
</style>
