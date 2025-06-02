<template>
  <div class="posters">
    <div class="poster-item" :key="index" v-for="(i, index) in list" @click="goDetail(index)">
      <img class="poster" v-lazy="_checkImgUrl(i.video.cover.url_list[0])" alt="" />
      <template v-if="mode === 'normal'">
        <div class="num1"><span role="img" class="semi-icon" style="font-size: 20px;"><svg viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" focusable="false"><path d="M15.143 9.18076L7.26011 3.66272C6.59733 3.19877 5.68665 3.67293 5.68665 4.48195V15.518C5.68665 16.3271 6.59733 16.8012 7.26011 16.3373L15.143 10.8192C15.7117 10.4211 15.7117 9.57886 15.143 9.18076Z" stroke="currentColor" stroke-width="2.15" stroke-linejoin="round"></path></svg></span><span class="num2">{{ _formatNumber(i.statistics.digg_count) }}</span></div>
        <div class="top" v-if="i.is_top">置顶</div>
      </template>
      <div class="date" v-if="mode === 'date'">
        <div class="day">{{ getDay(i.create_time) }}</div>
        <div class="month">{{ getMonth(i.create_time) }}</div>
      </div>
      <template v-if="mode === 'music'">
        <div class="music" v-if="index === 0">首发</div>
      </template>
    </div>
  </div>
</template>

<script setup>
import { _checkImgUrl, _formatNumber } from '@/utils'
import { useBaseStore } from '@/store/pinia'
import { useRouter } from 'vue-router'
import { cloneDeep } from '@/utils'

const store = useBaseStore()
const nav = useRouter()
const props = defineProps({
  list: {
    type: [Array, Number],
    default: () => {
      return []
    }
  },
  mode: {
    type: String,
    default: 'normal' //date,music
  }
})

defineOptions({
  name: 'Posters'
})

function goDetail(index) {
  store.routeData = cloneDeep({ list: props.list, index })
  nav.push({ path: '/video-detail' })
}

function getDay(time) {
  let date = new Date(time * 1000)
  return date.getDate()
}

function getMonth(time) {
  let date = new Date(time * 1000)
  let month = date.getMonth() + 1
  switch (month) {
    case 1:
      return '一月'
    case 2:
      return '二月'
    case 3:
      return '三月'
    case 4:
      return '四月'
    case 5:
      return '五月'
    case 6:
      return '六月'
    case 7:
      return '七月'
    case 8:
      return '八月'
    case 9:
      return '九月'
    case 10:
      return '十月'
    case 11:
      return '十一月'
    case 12:
      return '十二月'
  }
}
</script>

<style scoped lang="less">
.posters {
  display: grid;
  grid-template-columns: 33.33% 33.33% 33.33%;
}

.poster-item {
  height: 200rem;
  max-height: calc(33.33vw * 1.3);
  border: 0.5px solid black;
  overflow: hidden;
  position: relative;

  .poster {
    width: 100%;
    height: 100%;
    display: block;
    object-fit: cover;
  }

  .top,
  .music {
    position: absolute;
    font-size: 12rem;
    background: gold;
    color: black;
    padding: 2rem 3rem;
    border-radius: 2rem;
    top: 7rem;
    left: 7rem;
  }

  .num {
    color: white;
    position: absolute;
    bottom: 5rem;
    left: 5rem;
    display: flex;
    align-items: center;
    font-size: 20rem;
    gap: 3rem;

    .love {
      width: 14rem;
      height: 14rem;
      margin-right: 5rem;
    }
  }

  .num1 {
    color: #fff;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    font-family: PingFang SC,DFPKingGothicGB-Medium,sans-serif;
    font-size: 14px;
    font-weight: 500;
    line-height: 22px;
    display: flex;
    position: absolute;
    bottom: 4px;
    left: 10px
}
  .semi-icon {
    text-align: center;
    text-transform: none;
    text-rendering: optimizelegibility;
    fill: currentColor;
    font-style: normal;
    line-height: 0;
    display: inline-block
}
.num2 {
    margin-left: 5px;
    display: inline-block
}
  .date {
    position: absolute;
    top: 5rem;
    left: 5rem;
    display: flex;
    align-items: center;
    flex-direction: column;
    font-size: 14rem;
    color: black;
    background: white;
    padding: 6rem;
    border-radius: 6rem;

    .day {
      font-weight: bold;
    }

    .month {
      font-size: 10rem;
    }
  }
}
</style>
