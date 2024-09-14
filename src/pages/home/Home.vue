<template>
  <page-wraper>
    <view class="home">
      <wd-img-cropper v-model="show" :img-src="src" @confirm="handleConfirm" @cancel="handleCancel">
      </wd-img-cropper>
      <view class="profile">
        <view v-if="!imgSrc" class="img" @click="upload">
          <wd-icon name="fill-camera" custom-class="img-icon"></wd-icon>
        </view>
        <wd-img v-if="imgSrc" round width="200px" height="200px" :src="imgSrc" mode="aspectFit"
          custom-class="profile-img" @click="upload" />
        <view style="font-size: 14px;">点击上传头像</view>
      </view>
      <view style="margin-top: 100px;">
        <wd-input type="text" v-model="value" placeholder="请输入用户名" />
        <wd-input type="text" v-model="value1" placeholder="请输入用户名" />
        <wd-input type="text" v-model="value2" placeholder="请输入用户名" />
      </view>
    </view>
  </page-wraper>
</template>

<script lang="ts" setup>
import type KV from '@/model/KV'
import { ref, reactive } from 'vue'
import { fetchHome } from '../../services/home/home'
import { fetchGoodsList } from '../../services/good/fetchGoods'
import { useToast } from 'wot-design-uni'
const { show: showToast, loading: showLoading, close: hideLoading } = useToast()
const src = ref<string>('')
const imgSrc = ref<string>('')
const show = ref<boolean>(false)
const value = ref('dsauinmqjiownjdjoindasmk')
const value1 = ref('dsauinmqjiownjdjoindasmk')
const value2 = ref('dsauinmqjiownjdjoindasmk')

function upload() {
  uni.chooseImage({
    count: 1,
    success: (res) => {
      const tempFilePaths = res.tempFilePaths[0]
      src.value = tempFilePaths
      show.value = true
    }
  })
}
function handleConfirm(event) {
  const { tempFilePath } = event
  imgSrc.value = tempFilePath
}
function imgLoaderror(res) {
  console.log('加载失败', res)
}
function imgLoaded(res) {
  console.log('加载成功', res)
}
function handleCancel(event) {
  console.log('取消', event)
}
</script>

<style lang="scss" scoped>
.home {
  width: 100vw;
  box-sizing: border-box;
  background: #fff;

  .home-header {
    .search {
      margin-bottom: 20rpx;

      :deep(.custom-search) {
        padding: 0;
      }
    }
  }
}


.profile {
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  align-items: center;
  height: 300px;
}

:deep(.profile-img) {
  border: 1px solid rgba(0, 0, 0, 0.09);
}
.img {
  width: 200px;
  height: 200px;
  border-radius: 50%;
  background-color: rgba(0, 0, 0, 0.04);
  position: relative;
}
:deep(.img-icon) {
  font-size: 60px;
  color: #fff;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
</style>
