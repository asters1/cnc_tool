<template>
  <view class="wrap">
    <button type="primary" @click="showModal = true">
      显示20条数据
    </button>

    <view v-if="showModal" class="mask" @click="showModal = false">
      <view class="panel" @click.stop>
        <view class="list" :style="{height:'60vh'}">
          <view
            v-for="(item, idx) in history_data"
            :key="idx"
            class="item"
            @click="select(idx, item)"
          >
            {{ item }}
          </view>
        </view>
        <view class="cancel" @click="showModal = false">取消</view>
      </view>
    </view>
  </view>
</template>

<script setup>
import { ref } from 'vue'

const showModal = ref(false)

// 纯数组，不是 ref
const history_data = [
  '数据01','数据02','数据03','数据04','数据05',
  '数据06','数据07','数据08','数据09','数据10',
  '数据11','数据12','数据13','数据14','数据15',
  '数据16','数据17','数据18','数据19','数据20'
]

function select(index, item) {
  console.log('选中:', index, item)
  uni.showToast({
    title: '已选择：' + item,
    icon: 'none'
  })
  showModal.value = false
}
</script>

<style scoped>
.wrap {
  padding: 40rpx;
}
.mask {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 999;
}
.panel {
  width: 88%;
  background: #fff;
  border-radius: 20rpx;
  overflow: hidden;
}
.list {
  padding: 20rpx;
  overflow-y: auto;
}
.item {
  padding: 24rpx;
  text-align: center;
  border-bottom: 1rpx solid #eee;
}
.cancel {
  padding: 24rpx;
  text-align: center;
  background: #f8f8f8;
  font-weight: 500;
}
</style>