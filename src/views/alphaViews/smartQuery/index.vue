<template>
  <div class="app-container">
    <el-card class="box-card">
      <el-row gutter="20" style="margin-top: 1vh">
        <span style="font-size: 1rem;font-weight: bolder;margin-left: 1vw;margin-right: 15px">请选择查询类型:</span>
        <el-select v-model="projectclass" placeholder="选择查询类型" style="width: 18vw">
          <el-option
            v-for="item in options2"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
        <span style="font-size: 1rem;font-weight: bolder;margin-left: 3vw;margin-right: 15px">请输入关键词(选填):</span>
        <el-input v-model="input" placeholder="姓名/等" style="width: 18vw" />
        <el-button type="primary" round plain style="margin-left: 3vw" icon="el-icon-search" @click="searchmsg">点击查询</el-button>
      </el-row>
      <!--        <el-row  ">-->
      <div style="margin-top: 3vh;overflow: hidden;width: 100%">
        <span style="font-size: 1rem;font-weight: bolder;float:left;margin-left: 3vh">设置筛选条件:</span>
        <span style="font-size: 0.95rem;font-weight: bolder;float:left;margin-left:15px;margin-right: 10px">学院</span>
        <el-checkbox v-model="checked1" style="float:left;margin-right: 3vh">不限</el-checkbox>
        <el-checkbox-group v-model="college" style="width: 70%;float:left">
          <el-checkbox
            v-for="(item,index) in collegeList"
            :key="index"
            :label="item.label"
            @change="change1"
          />
        </el-checkbox-group>
      </div>
      <!--        </el-row>-->
      <!--        <HR width=1100 color=#dcdfe6 SIZE=1 align="right" />-->
      <el-row style="margin-top: 3vh">
        <span style="font-size: 0.95rem;font-weight: bolder;float:left;margin-right: 10px;margin-left: 135.5px">部门</span>
        <el-checkbox v-model="checked2" style="float:left;margin-right: 3vh">不限</el-checkbox>
        <el-checkbox-group v-model="department" style="width: 70%;float:left">
          <el-checkbox
            v-for="(item,index) in departmentList"
            :key="index"
            :label="item.label"
            @change="change2"
          />
        </el-checkbox-group>
      </el-row>
      <el-row style="margin-top: 3vh">
        <span style="font-size: 0.95rem;font-weight: bolder;float:left;margin-right: 10px;margin-left: 135.5px">职称</span>
        <el-checkbox v-model="checked3" style="float:left;margin-right: 3vh">不限</el-checkbox>
        <el-checkbox-group v-model="projectclass" style="width: 70%;float:left">
          <el-checkbox
            v-for="(item,index) in classList"
            :key="index"
            :label="item.label"
            @change="change3"
          />
        </el-checkbox-group>
      </el-row>
      <el-row style="margin-top: 3vh">
        <span style="font-size: 0.95rem;font-weight: bolder;float:left;margin-right: 10px;margin-left: 135.5px">状态</span>
        <el-checkbox v-model="checked4" style="float:left;margin-right: 3vh">不限</el-checkbox>
        <el-checkbox-group v-model="num" style="width: 70%;float:left">
          <el-checkbox
            v-for="(item,index) in numList"
            :key="index"
            :label="item.label"
            @change="change4"
          />
        </el-checkbox-group>
      </el-row>
      <el-row style="margin-top: 3vh">
        <span style="font-size: 0.95rem;font-weight: bolder;float:left;margin-right: 10px;margin-left: 135.5px">年龄分布</span>
        <el-checkbox v-model="checked5" style="float:left;margin-right: 3vh">不限</el-checkbox>
        <el-checkbox-group v-model="money" style="width: 70%;float:left">
          <el-checkbox
            v-for="(item,index) in moneyList"
            :key="index"
            :label="item.label"
            @change="change5"
          />
        </el-checkbox-group>
      </el-row>
      <el-divider />
      <div v-if="viewimg" class="img">
        <img src="../../../assets/zanwu.png">
        <div style="text-align: center">
          <span style="font-size: 15px;font-weight: bold;color: #999999">暂无内容</span>
        </div>
      </div>
      <div v-if="viewtable">
        <el-table
          v-if="viewtable"
          ref="multipleTable"
          :data="tableData"
          tooltip-effect="dark"
          style="width: 100%"
          @selection-change="handleSelectionChange"
        >
          <el-table-column
            v-if="piliangFlag"
            type="selection"
            width="55"
          />
          <!--      <el-table-column-->
          <!--        label="编号">-->
          <!--        <template slot-scope="scope">{{ scope.row.id }}</template>-->
          <!--      </el-table-column>-->
          <el-table-column
            prop="id_num"
            label="成员编号"
            width="100px"
          />
          <el-table-column
            prop="tDept"
            label="教师姓名"
          />
          <el-table-column
            prop="gender"
            label="性别"
          />
          <el-table-column
            prop="dep"
            width="150px"
            label="部门"
          />
          <el-table-column
            prop="zhicheng"
            label="职称"
          />
          <el-table-column
            prop="zhiwu"
            label="职务"
          />
          <el-table-column
            prop="age"
            label="年龄"
          />
          <el-table-column
            prop="status"
            label="工作状态"
          />
          <el-table-column
            prop="fdate"
            label="入校时间"
            width="100px"
          />
          <el-table-column align="center" label="操作" width="200">
            <template slot-scope="scope">
              <el-button type="text" style="margin-top: 10px" @click="handleClick(scope.row)">查看合同</el-button>
              <el-button type="text" style="margin-top: 10px">个人信息</el-button>
            </template>
          </el-table-column>
        </el-table>
        <div style="text-align: center">
          <el-pagination
            :current-page="currentPage"
            :page-sizes="[10, 20, 30]"
            :page-size="10"
            :total="tableData.length"
            style="margin-top:20px;"
            layout="total, sizes, prev, pager, next, jumper"
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
          />
        </div>
      </div>
    </el-card>
  </div>
</template>

<script>
export default {
  name: 'Index',
  data() {
    return {
      tableData: [
        {
          id_num: 'IM567382',
          tDept: '李老师',
          gender: '男',
          dep: '计算机工程学院',
          zhicheng: '教授',
          zhiwu: '院长',
          age: '55',
          status: '在职',
          fdate: '2003-8-9'
        },
        {
          id_num: 'IM564382',
          tDept: '何老师',
          gender: '男',
          dep: '计算机工程学院',
          zhicheng: '教授',
          zhiwu: '副院长',
          age: '53',
          status: '在职',
          fdate: '2005-8-9'
        },
        {
          id_num: 'IM598542',
          tDept: '杨老师',
          gender: '女',
          dep: '计算机工程学院',
          zhicheng: '副教授',
          zhiwu: '副院长',
          age: '50',
          status: '在职',
          fdate: '2008-12-9'
        },
        {
          id_num: 'IM508u36',
          tDept: '陈老师',
          gender: '男',
          dep: '计算机工程学院',
          zhicheng: '讲师',
          zhiwu: '教师',
          age: '34',
          status: '在职',
          fdate: '2012-12-9'
        },
        {
          id_num: 'IM567382',
          tDept: '邓老师',
          gender: '男',
          dep: '计算机工程学院',
          zhicheng: '讲师',
          zhiwu: '教师',
          age: '43',
          status: '在职',
          fdate: '2015-9-9'
        },
        {
          id_num: 'IM567382',
          tDept: '王老师',
          gender: '女',
          dep: '计算机工程学院',
          zhicheng: '讲师',
          zhiwu: '教师',
          age: '27',
          status: '在职',
          fdate: '2019-9-9'
        },
        {
          id_num: 'IM567382',
          tDept: '郭老师',
          gender: '男',
          dep: '计算机工程学院',
          zhicheng: '讲师',
          zhiwu: '教师',
          age: '44',
          status: '在职',
          fdate: '2016-9-9'
        },
        {
          id_num: 'IM567382',
          tDept: '安老师',
          gender: '女',
          dep: '计算机工程学院',
          zhicheng: '讲师',
          zhiwu: '教师',
          age: '47',
          status: '在职',
          fdate: '2012-9-12'
        }, {
          id_num: 'IM567382',
          tDept: '孙老师',
          gender: '男',
          dep: '计算机工程学院',
          zhicheng: '讲师',
          zhiwu: '教师',
          age: '50',
          status: '在职',
          fdate: '2010-8-21'
        }, {
          id_num: 'IM567382',
          tDept: '钱老师',
          gender: '男',
          dep: '计算机工程学院',
          zhicheng: '讲师',
          zhiwu: '教师',
          age: '39',
          status: '在职',
          fdate: '2014-9-1'
        }
      ],
      viewtable: false,
      viewimg: true,
      college: [],
      department: [],
      checked1: true,
      checked2: true,
      checked3: true,
      checked4: true,
      checked5: true,
      input: '',
      projectsub: '',
      projectclass: [],
      inputsearch: '',
      selectWord1: '',
      selectWord3: '',
      selectWord4: '',
      options1: [
        {
          value: '个人项目',
          label: '个人项目'
        },
        {
          value: '集体项目',
          label: '集体项目'
        }
      ],
      selectWord2: '',
      options2: [
        {
          value: '教师个人信息',
          label: '教师个人信息'
        }
      ],
      collegeList: [
        { label: '电子电器工程学院', value: '电子电器工程学院' },
        { label: '理学院', value: '理学院 ' },
        { label: '管理学院', value: '管理学院' },
        { label: '机械工程学院', value: '机械工程学院' },
        { label: '电子信息学院', value: '电子信息学院' },
        { label: '计算机工程学院', value: '计算机工程学院' },
        { label: '经贸管理学院', value: '经贸管理学院' },
        { label: '人文社科学院', value: '人文社科学院' }
      ],
      departmentList: [
        { label: '党委办公室', value: '1' },
        { label: '党委组织部', value: '2 ' },
        { label: '学工部', value: '3' },
        { label: '纪检监察室', value: '4' },
        { label: '人武部', value: '5' },
        { label: '团委', value: '6' }
      ],
      classList: [
        { label: '助教', value: '1' },
        { label: '讲师', value: '2 ' },
        { label: '副教授', value: '3' },
        { label: '教授', value: '4' }
      ],
      numList: [
        { label: '在职', value: '1' },
        { label: '离职', value: '2 ' },
        { label: '退休', value: '3' },
        { label: '转岗', value: '4' },
        { label: '借调', value: '5' }
      ],
      moneyList: [
        { label: '30岁以下', value: '1' },
        { label: '30岁~40岁', value: '2 ' },
        { label: '40岁~50岁', value: '3' },
        { label: '50岁~65岁', value: '4' }

      ],
      num: [],
      money: [],
      checkList: [], // 选中展示的字段
      uncheckList: {} // 控制隐藏的字段
    }
  },
  methods: {
    searchmsg() {
      this.$message({
        type: 'success',
        message: '查询成功'
      })
      this.viewimg = false
      this.viewtable = true
    },
    change1() {
      this.checked1 = false
    },
    change2() {
      this.checked2 = false
    },
    change3() {
      this.checked3 = false
    },
    change4() {
      this.checked4 = false
    },
    change5() {
      this.checked5 = false
    }

  }
}
</script>

<style scoped>
  .box-card {
    width: 100%;
    height: 100%;

  }
  /*/deep/ .el-card__body{*/
  /*  height: 100vh;*/
  /*}*/
  .img{
    text-align: center;
  }
</style>
