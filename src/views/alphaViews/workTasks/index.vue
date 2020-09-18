<template>
  <div class="app-container">
    <div style="background-color: white;padding: 20px">
    <el-row>
      <span style="font-size: 16px;font-weight: bolder">选择工作任务类型：</span>
      <el-select v-model="type_select_work" placeholder="请选择您的工作任务类型" >
        <el-option label="基本任务" value="basic"/>
        <el-option label="绩效任务" value="scientific"/>
      </el-select>
    </el-row>
      <el-divider/>
    <el-row v-if="type_select_work === 'basic'">
      <basic-table/>
    </el-row>
    <el-row v-if="type_select_work === 'scientific'">
      <scientific-table/>
    </el-row>
    </div>
  </div>
</template>

<script>
import basicTable from "./workTable/basicTable";
import scientificTable from "./workTable/scientificTable";
export default {
  name: 'Index',
  components:{basicTable,scientificTable},
  data(){
    return{
      type_select_work: 'basic',
      value: ''
    }
  }
}
</script>

<style scoped>

</style>
