<template>
  <el-row>
    <el-col :span="12">
      <el-row>
        <el-form :model="form" label-width="120px">
          <el-form-item label="名字">
            <el-input v-model="form.name" placeholder="请输入食品名字" />
          </el-form-item>
          <el-form-item label="地点">
            <el-select
              v-model="form.address"
              placeholder="请选择地址"
              clearable
            >
              <el-option
                v-for="(item, index) in addressOptions"
                :key="index"
                :label="item"
                :value="item"
              />
            </el-select>
          </el-form-item>
          <!-- <el-form-item label="是否到店">
            <el-switch v-model="form.delivery" />
          </el-form-item> -->

          <el-form-item>
            <el-button type="primary" @click="search">搜索</el-button>
            <!-- <el-button>Cancel</el-button> -->
          </el-form-item>
        </el-form>
      </el-row>
      <el-row v-show="showFoodList">
        <el-col>
          <ul class="food-list-ul">
            <li v-for="(value, key) in foodArr" :key="key">
              <span>{{ key }}</span>
              <span> - </span>
              <span>{{ value.name }}</span>
            </li>
          </ul>
        </el-col>
      </el-row>
    </el-col>
    <el-col :span="12">
      <el-row>
        <el-col :span="12">
          {{ randomResult.name }}
        </el-col>
        <el-col :span="12">
          <el-button @click="randomFood">随机</el-button>
        </el-col>
      </el-row>
    </el-col>
  </el-row>
</template>

<script setup lang="ts">
import foodList from '../utils/foods.json';
import { ref, reactive } from 'vue';
/** 搜索条件 */
const form = reactive({
  address: '',
  name: '',
  delivery: false,
});
/** 食品列表 */
const foodArr = reactive([] as any);
/** 是否显示食品列表 */
const showFoodList = ref(false);
/** 地址options */
const addressOptions = ref([] as any);
/** 初始化address下拉列表 */
const initOptions = function () {
  let addressList = [] as String[];
  foodList.forEach((food) => {
    if (food.address) {
      addressList.push(...food.address);
    }
  });
  addressOptions.value.push(...Array.from(new Set(addressList)));
};
initOptions();
/** 搜索事件 */
const search = function () {
  // 1. 清空原始列表
  foodArr.splice(0, foodArr.length);
  // 2. 如果检索条件为空，则显示全部
  if (form.name === '' && form.address === '') {
    showFoodList.value = false;
    return;
  }
  showFoodList.value = true;
  // 3. 如果检索条件存在，则显示检索内容
  foodList.forEach((food) => {
    if (!!form.name && food.name.indexOf(form.name) < 0) {
      return;
    }
    if (!!form.address && !food.address?.includes(form.address)) {
      return;
    }
    foodArr.push(food);
  });
};

/** 随机结果 */
const randomResult = ref({} as any);
/** 随机食物 */
const randomFood = () => {
  let list = [] as any;
  if (form.name === '') {
    list = foodList;
  } else {
    list = foodArr;
  }
  let size = list.length;
  let randomNum = Math.trunc(Math.random() * size);
  randomResult.value = list[randomNum];
};
</script>

<style scoped>
.food-list-ul {
  text-align: left;
  list-style-type: none;
}
</style>
