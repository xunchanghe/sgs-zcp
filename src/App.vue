<template>
  <div style="width: 100%;height: 100%;padding: 50px 200px;box-sizing: border-box">
    <h1>张昌蒲计算器</h1>
    <div style="color:#EE0000">注10请用x代替</div>
    <div style="display: flex;">
      <el-input v-model="num" @input="inputNum"/>
      <div>
        <el-button type="primary" @click="search">搜索</el-button>
      </div>
    </div>
    <div v-for="(item,index) in root" :key="index" style="font-size: 28px;font-weight: bolder">{{
        item.first
      }}-----{{ item.last }}
    </div>
  </div>
</template>

<script lang="ts" setup>
import {reactive, ref} from "vue";

const num = ref('')
const formatNumList = reactive<any>([])
const root = reactive<any>([])

function inputNum(e: string) {
  num.value = e.replace(/[^.axjqk[2-9]/g, '')
  const n = num.value.split("")
  formatNumList.length = 0
  formatNumList.push(...n.map((e: any) => formatStr(e)))
}

function search() {
  root.length = 0
  combinationList.length = 0
  match(formatNumList)
}

function formatStr(str: string) {
  return {
    'a': 1,
    '2': 2,
    '3': 3,
    '4': 4,
    '5': 5,
    '6': 6,
    '7': 7,
    '8': 8,
    '9': 9,
    'x': 10,
    'j': 11,
    'q': 12,
    'k': 13,
  }[str]
}

function reverseFormatStr(num: number) {
  return {
    1: 'a',
    2: '2',
    3: '3',
    4: '4',
    5: '5',
    6: '6',
    7: '7',
    8: '8',
    9: '9',
    10: '10',
    11: 'j',
    12: 'q',
    13: 'k',
  }[num]
}


/*
* 倒序循环 取数组的排列组合
*
* */
function match(arr: Array<number> = []) {
  for (let k = arr.length - 1; k > 0; k--) {
    const halfLength = arr.slice(0, k).length
    combination(arr, halfLength, [])
    for (let i = 0; i < combinationList.length; i++) {
      for (let j = i + 1; j < combinationList.length; j++) {
        if (combinationList[i].reduce((a: number, b: number) => a + b) === combinationList[j].reduce((a: number, b: number) => a + b) && !combinationList[i].some((e: any) => combinationList[j].includes(e))) {
          root.push({
            first: combinationList[i].map((e: any) => reverseFormatStr(e)),
            last: combinationList[j].map((e: any) => reverseFormatStr(e))
          })
          // console.log('数组之和相等', combinationList[i].map((e: any) => reverseFormatStr(e)), '另一个', combinationList[j].map((e: any) => reverseFormatStr(e)))
        }
      }
    }
  }
}

const combinationList: any = []

// 排列组合 不排列无重复
/*
*  arr 需要排列组合的数组
*  remainNum 多少位数排列组合
*  currentArr 默认传空数组[]
* */
function combination(arr: Array<number>, remainNum: number, currentArr: Array<number>) {
  if (remainNum === 0) {
    combinationList.push([...currentArr])
    return
  }
  for (let i = 0; i < arr.length + 1 - remainNum; i++) {
    currentArr.push(arr[i])
    combination(arr.slice(i + 1), remainNum - 1, currentArr)
    currentArr.pop()
  }
}

match()
</script>

<style lang="scss">

/*定义滚动条高宽及背景 高宽分别对应横竖滚动条的尺寸*/
::-webkit-scrollbar {
  width: 5px;
  height: 5px;
}

/*定义滚动条轨道 内阴影+圆角*/
::-webkit-scrollbar-track {
  background-color: transparent;
}

/*定义滑块 内阴影+圆角*/
::-webkit-scrollbar-thumb {
  border-radius: 10px;
  background-color: rgba(0, 0, 0, .2);
}


html, body {
  background-color: #f3f4f6;
  width: 100%;
  height: 100%;
  font-size: 12px;
  font-family: -apple-system, BlinkMacSystemFont, "Helvetica Neue", Helvetica, Arial, "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", sans-serif;
  -webkit-font-smoothing: antialiased
}

body > div {
  width: 100%;
  height: 100%;
}

ul, li {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>
