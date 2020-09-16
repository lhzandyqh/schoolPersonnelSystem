<template>
  <div class="app-container">
    <div style="margin: 0 0 10px 0;font-size: 16px;font-weight: bolder;text-align: center;">
      <span style="">教学工作情况</span>
      <div style="float: right;margin-right: 1.5rem"><el-button type="text" size="medium" @click="addClass">新增</el-button></div>
    </div>
    <el-table :data="peopleData" style="width: 100%" stripe>
      <el-table-column prop="name" label="课程名称" >
      </el-table-column>
      <el-table-column prop="class" label="任教年级">
      </el-table-column>
      <el-table-column prop="department" label="任教学院" >
      </el-table-column>
      <el-table-column prop="bdate" label="课程开始时间">
      </el-table-column>
      <el-table-column prop="fdate" label="课程结束时间">
      </el-table-column>
      <el-table-column prop="weeknum" label="每周课时" width="100px">
      </el-table-column>
      <el-table-column prop="sumnum" label="总课时" width="100px">
      </el-table-column>
      <el-table-column label="学科状态">
        <template slot-scope="scope">
          <el-tag  v-if="scope.row.approval==='未开始'" type="danger" >未开始</el-tag>
          <el-tag  v-if="scope.row.approval==='进行中'" >进行中</el-tag>
          <el-tag  v-if="scope.row.approval==='已结课'" type="success">已结课</el-tag>
        </template>
      </el-table-column>
      <el-table-column align="center" label="操作">
        <template slot-scope="scope">
          <el-button type="text" size="medium" @click="changeinfo">修改</el-button>
        </template>
      </el-table-column>
    </el-table>
    <div style="text-align: center; margin-top: 10px;">
      <el-pagination
        @current-change="handleCurrentChange"
        :current-page="currentPage"
        :page-size="pagesize"
        :page-sizes="[5, 10]"
        :total="peopleData.length"
        layout="total, sizes, prev, pager, next, jumper"
      />
    </div>
    <div>
      <el-dialog :visible.sync="addVisible" title="新增教学工作">
        <el-form ref="form" :inline="true" :model="form" label-width="100px">
          <el-form-item label="课程名称">
            <el-input v-model="form.classname"/>
          </el-form-item>
          <el-form-item label="任教年级">
            <el-input v-model="form.class"/>
          </el-form-item>
          <el-form-item label="任教学院">
            <el-input v-model="form.department"/>
          </el-form-item>
          <el-form-item label="课程时间">
            <el-col :span="9">
              <el-date-picker type="date" placeholder="开课日期" v-model="form.bdate" style="width: 90%;"></el-date-picker>
            </el-col>
            <el-col :span="1">-</el-col>
            <el-col :span="9">
              <el-date-picker type="date" placeholder="结课日期" v-model="form.fdate" style="width: 90%;"></el-date-picker>
            </el-col>
          </el-form-item>
          <el-form-item label="每周课时">
            <el-input v-model="form.weeknum"/>
          </el-form-item>
          <el-form-item label="总课时">
            <el-input v-model="form.sumnum"/>
          </el-form-item>
          <el-form-item label="学科状态">
            <el-select v-model="form.isend" style="width: 185px" placeholder="请选择">
              <el-option label="未开始" value="0"></el-option>
              <el-option label="进行中" value="1"></el-option>
              <el-option label="已结课" value="2"></el-option>
            </el-select>
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="addVisible = false">取 消</el-button>
          <el-button type="primary" @click="addClassSuccess">确 定</el-button>
        </div>
      </el-dialog>
    </div>
    <div>
      <el-dialog :visible.sync="changeVisible" title="修改教学工作">
        <el-form ref="form" :inline="true" :model="form" label-width="100px">
          <el-form-item label="课程名称">
            <el-input v-model="form.classname" placeholder="编译原理" />
          </el-form-item>
          <el-form-item label="任教年级">
            <el-input v-model="form.class" placeholder="大二" />
          </el-form-item>
          <el-form-item label="任教学院">
            <el-input v-model="form.department" placeholder="计算机学院" />
          </el-form-item>
          <el-form-item label="课程时间">
            <el-col :span="9">
              <el-date-picker type="date" placeholder="2019-9-1" v-model="form.bdate" style="width: 90%;"></el-date-picker>
            </el-col>
            <el-col :span="1">-</el-col>
            <el-col :span="9">
              <el-date-picker type="date" placeholder="2019-12-10" v-model="form.fdate" style="width: 90%;" ></el-date-picker>
            </el-col>
          </el-form-item>
          <el-form-item label="每周课时">
            <el-input v-model="form.weeknum" placeholder="2" />
          </el-form-item>
          <el-form-item label="总课时">
            <el-input v-model="form.sumnum" placeholder="32" />
          </el-form-item>
          <el-form-item label="学科状态">
            <el-select v-model="form.isend" style="width: 185px" placeholder="已结课">
              <el-option label="未开始" value="0"></el-option>
              <el-option label="进行中" value="1"></el-option>
              <el-option label="已结课" value="2"></el-option>
            </el-select>
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="changeVisible = false">取 消</el-button>
          <el-button type="primary" @click="changeSuccess">确 定</el-button>
        </div>
      </el-dialog>
    </div>
    <!--    <el-dialog :visible.sync="hexinVisible" title="审核详情" style="width: 110%">-->
    <!--      <el-row :gutter="20" style="padding-top: 10px">-->
    <!--        <el-col :span="8">-->
    <!--          <div class="single">-->
    <!--            <div class="biaoqian">-->
    <!--              <span style="font-weight: bolder">文章题目：</span>-->
    <!--              <span>高点探测智能分析</span>-->
    <!--            </div>-->
    <!--          </div>-->
    <!--        </el-col>-->
    <!--        <el-col :span="8">-->
    <!--          <div class="single">-->
    <!--            <div class="biaoqian">-->
    <!--              <span style="font-weight: bolder">发表时间：</span>-->
    <!--            </div>-->
    <!--          </div>-->
    <!--        </el-col>-->
    <!--        <el-col :span="8">-->
    <!--          <div class="single">-->
    <!--            <div class="biaoqian">-->
    <!--              <span style="font-weight: bolder">是否第一作者：</span>-->
    <!--              <span>是</span>-->
    <!--            </div>-->
    <!--          </div>-->
    <!--        </el-col>-->
    <!--      </el-row>-->
    <!--      <el-row :gutter="20" style="padding-top: 10px">-->
    <!--        <el-col :span="8">-->
    <!--          <div class="single">-->
    <!--            <div class="biaoqian">-->
    <!--              <span style="font-weight: bolder">发表刊物：</span>-->
    <!--            </div>-->
    <!--          </div>-->
    <!--        </el-col>-->
    <!--        <el-col :span="8">-->
    <!--          <div class="single">-->
    <!--            <div class="biaoqian">-->
    <!--              <span style="font-weight: bolder">出版社：</span>-->
    <!--            </div>-->
    <!--          </div>-->
    <!--        </el-col>-->
    <!--        <el-col :span="8">-->
    <!--          <div class="single">-->
    <!--            <div class="biaoqian">-->
    <!--              <span style="font-weight: bolder">提交时间：</span>-->
    <!--              <span>2019.4.3</span>-->
    <!--            </div>-->
    <!--          </div>-->
    <!--        </el-col>-->
    <!--      </el-row>-->
    <!--      <el-row :gutter="20" style="padding-top: 10px">-->
    <!--        <el-col :span="8">-->
    <!--          <div class="single">-->
    <!--            <div class="biaoqian">-->
    <!--              <span style="font-weight: bolder">照片证明：</span>-->
    <!--              <el-button type="text">查看图片</el-button>-->
    <!--            </div>-->
    <!--          </div>-->
    <!--        </el-col>-->
    <!--      </el-row>-->
    <!--      <el-divider/>-->
    <!--      <h4>部门意见</h4>-->
    <!--      <div>-->
    <!--        <el-row>-->
    <!--          <el-col :span="6">-->
    <!--            项目具有可行性，审核通过-->
    <!--          </el-col>-->
    <!--          <el-col :span="6">-->
    <!--            <span>审核时间：</span>-->
    <!--            <span>2019-12-04</span>-->
    <!--          </el-col>-->
    <!--          <el-col :span="6">-->
    <!--            <span>审核人：</span>-->
    <!--            <span>刘明至</span>-->
    <!--          </el-col>-->
    <!--          <el-col :span="5">-->
    <!--            <span>审核状态</span>-->
    <!--            <el-tag type="success" size="small">通过</el-tag>-->
    <!--          </el-col>-->
    <!--        </el-row>-->
    <!--      </div>-->
    <!--      <el-divider />-->
    <!--      <h4>系部意见</h4>-->
    <!--      <div>-->
    <!--        <el-row>-->
    <!--          <el-col :span="6">-->
    <!--            项目具有可行性，建议加快进度-->
    <!--          </el-col>-->
    <!--          <el-col :span="6">-->
    <!--            <span>审核时间：</span>-->
    <!--            <span>2019-12-04</span>-->
    <!--          </el-col>-->
    <!--          <el-col :span="6">-->
    <!--            <span>审核人：</span>-->
    <!--            <span>郭志</span>-->
    <!--          </el-col>-->
    <!--          <el-col :span="5">-->
    <!--            <span>审核状态</span>-->
    <!--            <el-tag type="success" size="small">通过</el-tag>-->
    <!--          </el-col>-->
    <!--        </el-row>-->
    <!--      </div>-->
    <!--      <el-divider />-->
    <!--      <div>-->
    <!--        <el-row style="padding-top: 10px">-->
    <!--          <span style="font-weight: bolder">科研处意见</span>-->
    <!--        </el-row>-->
    <!--        <el-row style="padding-top: 10px">-->
    <!--          <el-input-->
    <!--            :rows="4"-->
    <!--            v-model="AuditingReason"-->
    <!--            type="textarea"-->
    <!--            placeholder="请输入内容"/>-->
    <!--        </el-row>-->
    <!--      </div>-->
    <!--      <div class="foot">-->
    <!--        <span slot="footer" class="dialog-footer">-->
    <!--          <el-button type="success" size="small" plain @click="pass">审核通过</el-button>-->
    <!--          <el-button type="danger" size="small" plain @click="pass">审核未通过</el-button>-->
    <!--          <el-button type="primary" @click="zhuanyeVisible = false"  size="small" plain>关闭</el-button>-->
    <!--        </span>-->
    <!--      </div>-->
    <!--    </el-dialog>-->
  </div>
</template>

<script>
export default {
  name: "basicTable",
  data(){
    return{
      addVisible:false,
      changeVisible:false,
      form: {
        classname:'',
        class:'',
        department:'',
        bdate:'',
        fdate:'',
        weeknum:'',
        sumnum:'',
        isend:''
      },
      peopleData:[
        {
          name: '编译原理',
          class:'大二',
          department:'计算机学院',
          bdate:'2019-9-1',
          fdate:'2019-12-13',
          weeknum:'2',
          sumnum:'32',
          approval:'已结课',
        },
        {
          name: 'C语言程序设计',
          class:'大一',
          department:'机械学院',
          bdate:'2020-3-1',
          fdate:'2020-6-30',
          weeknum:'1',
          sumnum:'16',
          approval:'已结课',
        },
        {
          name: '计算机控制系统',
          class:'大二',
          department:'计算机学院',
          bdate:'2020-9-1',
          fdate:'2020-12-30',
          weeknum:'1',
          sumnum:'16',
          approval:'进行中',
        },
        {
          name: '计算机实训',
          class:'大二',
          department:'计算机学院',
          bdate:'2020-10-1',
          fdate:'2020-10-10',
          weeknum:'8',
          sumnum:'8',
          approval:'未开始',
        },
        {
          name: '离散数学',
          class:'大二',
          department:'计算机学院',
          bdate:'2020-9-1',
          fdate:'2020-11-10',
          weeknum:'2',
          sumnum:'24',
          approval:'进行中',
        },
        {
          name: '嵌入式系统',
          class:'大三',
          department:'计算机学院',
          bdate:'2020-10-1',
          fdate:'2020-12-10',
          weeknum:'1',
          sumnum:'9',
          approval:'未开始',
        },
        {
          name: 'VHDL实训',
          class:'大三',
          department:'计算机学院',
          bdate:'2020-9-1',
          fdate:'2020-9-10',
          weeknum:'8',
          sumnum:'8',
          approval:'已结课',
        },
        {
          name: '计算机图形学基础',
          class:'大二',
          department:'计算机学院',
          bdate:'2020-3-1',
          fdate:'2020-6-10',
          weeknum:'2',
          sumnum:'24',
          approval:'已结课',
        },

      ]
    }
  },
  methods: {
    addClass(){
      this.addVisible = true;
    },
    changeinfo(){
      this.changeVisible = true;
    },
    addClassSuccess(){
      this.addVisible = false;
      this.$message({
          type: 'success',
          message: '新增成功',
        }
      )
    },
    changeSuccess(){
      this.changeVisible = false;
      this.$message({
        type: 'success',
        message: '修改成功',
    })
  }
  }
}
</script>

<style scoped>

</style>
