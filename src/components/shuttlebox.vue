<template>
    <div class="shuttlebox">
        <div class="main">
            <ul class="leftbox">
                <!-- 左侧穿梭框 -->
                <li class="choose-item" @click="setChooseAll(leftArr,0)">
                    <!-- 全选区域 -->
                    <input type="checkbox" v-model="checkedAll[0]"><span class="choose-label">全选</span>
                    <!-- 统计选中长度以及总共长度 -->
                    <span class="checked-length">{{ checked[0] }}/{{ leftArr.length }}</span>
                </li>
                <li v-for="(item, index) in leftArr" class="choose-item" @click="setChoose(leftArr, index,0)">
                    <!-- 循环渲染穿梭框选项 -->
                    <input type="checkbox" v-model="item.checked"><span class="choose-label">{{ item.label }}</span>
                </li>
            </ul>
            <div class="centerbox">
                <!-- 中间执行穿梭按钮 -->
                <button class="btnItem" v-for="(item, index) in btnList" @click="shuttle(index)">{{ item }}</button>
            </div>
            <ul class="rightbox">
                <!-- 右侧穿梭框 -->
                <li class="choose-item" @click="setChooseAll(rightArr,1)">
                    <input type="checkbox" v-model="checkedAll[1]"><span class="choose-label">全选</span>
                    <span class="checked-length">{{ checked[1] }}/{{ rightArr.length }}</span>
                </li>
                <li v-for="(item, index) in rightArr" class="choose-item" @click="setChoose(rightArr, index,1)">
                    <input type="checkbox" v-model="item.checked"><span class="choose-label">{{ item.label }}</span>
                </li>
            </ul>
        </div>
    </div>
</template>
<script lang="ts" setup>
import { computed, Ref, ref } from 'vue';
// 定义初始数据
const leftArr = ref([
    { label: "选项1", checked: true, id: 1 },
    { label: "选项2", checked: false, id: 2 },
    { label: "选项3", checked: false, id: 3 },
    { label: "选项4", checked: false, id: 4 }
]), rightArr = ref([
    { label: "选项5", checked: false, id: 5 },
    { label: "选项6", checked: false, id: 6 },
    { label: "选项7", checked: false, id: 7 },
    { label: "选项8", checked: false, id: 8 }
]), btnList = ['<', '>']
    , checkedAll = ref([false, false]);
const checked: Ref<any[]> = ref([]);
// 计算两侧数据选中的个数
checked.value[0] = computed(() => {
    return getCheckedLength(leftArr);
});
checked.value[1] = computed(() => {
    return getCheckedLength(rightArr);
})
// 设置当前点击的元素的checked
const setChoose = (arr: any[], idx: number,index:number) => {
    arr[idx].checked = !arr[idx].checked;
    // 设置全选的状态
    changeChooseAll(arr,index);
}
// 执行穿梭入口
const shuttle = (idx: number) => {
    idx === 1 ? changeArr(leftArr, rightArr) : changeArr(rightArr, leftArr);
    // 设置全选状态
    changeChooseAll(leftArr.value,0);
    changeChooseAll(rightArr.value,1);
}
// 执行穿梭功能
const changeArr = (reduceArr: Ref<{ label: string; checked: boolean; id: number; }[]>, addArr: Ref<{ label: string; checked: boolean; id: number; }[]>) => {
    const temp = [];
    for (let i = 0; i < reduceArr.value.length; i++) {
        if (reduceArr.value[i].checked) {
            reduceArr.value[i].checked = false;
            addArr.value.push(reduceArr.value[i]);
        } else {
            temp.push(reduceArr.value[i]);
        }
    }
    reduceArr.value = temp;
    // 添加数据的数组执行排序
    addArr.value.sort((a, b) => a.id - b.id);
}
// 执行全选菜操作
const setChooseAll = (arr: { label: string; checked: boolean; id: number; }[],index:number)=>{
    checkedAll.value[index] = !checkedAll.value[index];
    arr.forEach(item=>{
        item.checked = checkedAll.value[index];
    })
}
// 修改全选状态
const changeChooseAll = (arr:any[],index:number)=>{
    console.log('in');
    let time = 0;
    arr.forEach(item=>{
        item.checked ? time++ : '';
    })
    time == arr.length && time !=0 ? checkedAll.value[index] = true : checkedAll.value[index] = false;
}
// 获取选中个数
function getCheckedLength(arr: Ref<{ label: string; checked: boolean; id: number; }[]>) {
    let time = 0;
    arr.value.forEach(item => {
        if (item.checked) {
            time++;
        }
    })
    return time;
}
</script>
<style scoped>
.shuttlebox {
    padding: 50px 100px;
}

.main {
    background-color: #ccc;
    display: flex;
    align-items: center;
    justify-content: space-around;
}

.leftbox,
.rightbox {
    margin: 10px 0;
    border-radius: 4px;
    background-color: #fff;
}

.choose-item {
    padding: 10px 20px;
    display: flex;
    align-items: center;
}

.choose-item:hover {
    background-color: rgb(239, 239, 239);
}

.choose-label {
    padding-left: 5px;
    margin-top: -2px;
}

button {
    padding: 10px 50px;
    background-color: #1588f5;
    color: #fff;
    border-radius: 4px;
    border: none;
    margin-left: 10px;
}

button:hover {
    background-color: #1473cb;
}
.checked-length{
    margin-top: 2px;
    padding-left: 5px;
}
</style>