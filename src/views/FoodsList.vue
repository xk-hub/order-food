<template>
  <el-row>
    <el-col :span="6">
      <el-input v-model="searchInput"></el-input>
    </el-col>
    <el-col :span="12">
      <el-button @click="search">搜索</el-button>
    </el-col>
  </el-row>
  <el-row>
    <el-col>
      <ul class="food-list-ul">
        <li v-for="(value, key) in foodArr" :key="key">
          <span>{{ value.name }}</span>
        </li>
      </ul>
    </el-col>
  </el-row>
</template>

<script setup lang="ts">
import foodList from '../utils/foods.json';
import { ref, reactive, onMounted } from 'vue';
/**
 * 搜索条件
 */
const searchInput = ref('');
/**
 * 食品列表
 */
const foodArr = reactive([...foodList] as any);
/**
 * 搜索事件
 */
const search = function () {
  // 1. 清空原始列表
  foodArr.splice(0, foodArr.length);
  // 2. 如果检索条件为空，则显示全部
  if (searchInput.value === '' || searchInput.value === undefined) {
    foodArr.push([...foodList]);
    return
  }
  // 3. 如果检索条件存在，则显示检索内容
  foodList.forEach((food) => {
    if (food.name.indexOf(searchInput.value) >= 0) {
      foodArr.push(food);
    }
  });
};
</script>

<style scoped>
.food-list-ul {
  text-align: left;
  list-style-type: none;
}
</style>
