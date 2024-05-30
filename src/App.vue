<template>
  <div>
    <el-table :data="tableData" style="width: 100%">
      <el-table-column prop="date" label="Date" width="180" />
      <el-table-column prop="name" label="Name" width="180" />
      <el-table-column prop="address" label="Address" />
      <el-table-column prop="remak" label="数量">
        <template #default="scope">
          <el-input v-model="scope.row.remak" placeholder="请输入" />
        </template>
      </el-table-column>
    </el-table>
    <div style="text-align: right; margin-top: 10px">
      <span>总数量: {{ totalRemak }}</span>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref, computed } from "vue";

const tableData = ref([
  {
    date: "2016-05-03",
    name: "Tom",
    address: "No. 189, Grove St, Los Angeles",
    remak: 0, // 添加 remak 属性并初始化为0
  },
  {
    date: "2016-05-02",
    name: "Tom",
    address: "No. 189, Grove St, Los Angeles",
    remak: 0, // 添加 remak 属性并初始化为0
  },
  {
    date: "2016-05-04",
    name: "Tom",
    address: "No. 189, Grove St, Los Angeles",
    remak: 0, // 添加 remak 属性并初始化为0
  },
  {
    date: "2016-05-01",
    name: "Tom",
    address: "No. 189, Grove St, Los Angeles",
    remak: 0, // 添加 remak 属性并初始化为0
  },
]);

const totalRemak = computed(() => {
  return tableData.value.reduce((sum, row) => {
    return sum + Number(row.remak);
  }, 0);
});
</script>
