<template>
  <div class="app-container">
    <div style="margin: 0 0 10px 0;font-size: 16px;font-weight: bolder;text-align: center;">
      <span style="">论文发表情况</span>
      <div style="float: right;margin-right: 1.5rem"><el-button type="text" size="medium" @click="addClass">新增</el-button></div>
    </div>
    <el-table :data="paperData" style="width: 100%" stripe>
      <el-table-column prop="papername" label="论文名称" >
      </el-table-column>
      <el-table-column prop="isauthor" label="是否第一作者">
      </el-table-column>
      <el-table-column prop="publication" label="发表刊物" >
      </el-table-column>
      <el-table-column prop="press" label="出版社">
      </el-table-column>
      <el-table-column prop="pubtime" label="发表时间">
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
    <hr style="margin: 30px 0 30px 0"/>
    <div style="margin: 0 0 10px 0;font-size: 16px;font-weight: bolder;text-align: center;">
      <span style="">参加比赛情况</span>
      <div style="float: right;margin-right: 1.5rem"><el-button type="text" size="medium" @click="addMatch">新增</el-button></div>
    </div>
    <el-table :data="matchData" style="width: 100%" stripe>
      <el-table-column prop="matchname" label="比赛名称" >
      </el-table-column>
      <el-table-column prop="date" label="比赛时间">
      </el-table-column>
      <el-table-column prop="matchclass" label="比赛级别" >
      </el-table-column>
      <el-table-column prop="name" label="作品名称">
      </el-table-column>
      <el-table-column prop="class" label="获奖情况">
      </el-table-column>
      <el-table-column align="center" label="操作">
        <template slot-scope="scope">
          <el-button type="text" size="medium" @click="changematch">修改</el-button>
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
    <hr style="margin: 30px 0 30px 0"/>
    <div style="margin: 0 0 10px 0;font-size: 16px;font-weight: bolder;text-align: center;">
      <span style="">组织指导课外活动情况</span>
      <div style="float: right;margin-right: 1.5rem"><el-button type="text" size="medium" @click="addOrganization">新增</el-button></div>
    </div>
    <el-table :data="activeData" style="width: 100%" stripe>
      <el-table-column prop="bdate" label="起始时间" >
      </el-table-column>
      <el-table-column prop="fdate" label="终止时间">
      </el-table-column>
      <el-table-column prop="activename" label="组织名称" >
      </el-table-column>
      <el-table-column prop="peoplenum" label="参加人数">
      </el-table-column>
      <el-table-column prop="activenum" label="活动次数">
      </el-table-column>
      <el-table-column prop="activecontent" label="活动内容">
      </el-table-column>
      <el-table-column align="center" label="操作">
        <template slot-scope="scope">
          <el-button type="text" size="medium" @click="changcOrganization">修改</el-button>
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
    <hr style="margin: 30px 0 30px 0"/>
    <div style="margin: 0 0 10px 0;font-size: 16px;font-weight: bolder;text-align: center;">
      <span style="">参与进修教育情况</span>
      <div style="float: right;margin-right: 1.5rem"><el-button type="text" size="medium" @click="addFurther">新增</el-button></div>
    </div>
    <el-table :data="jinxiuData" style="width: 100%" stripe>
      <el-table-column prop="bdate" label="起始时间" >
      </el-table-column>
      <el-table-column prop="fdate" label="终止时间">
      </el-table-column>
      <el-table-column prop="name" label="进修单位名称" >
      </el-table-column>
      <el-table-column prop="content" label="进修内容">
      </el-table-column>
      <el-table-column prop="xingshi" label="进修形式">
      </el-table-column>
      <el-table-column prop="completenum" label="完成课时">
      </el-table-column>
      <el-table-column prop="cdate" label="结业时间">
      </el-table-column>
      <el-table-column align="center" label="操作">
        <template slot-scope="scope">
          <el-button type="text" size="medium" @click="changeFurtherinfo">修改</el-button>
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
      <el-dialog :visible.sync="addVisible" title="新增论文发表情况">
        <el-form ref="form" :model="form" label-width="100px">
          <el-form-item label="论文名称">
            <el-input v-model="form.papername"/>
          </el-form-item>
          <el-form-item label="是否第一作者">
            <el-select v-model="form.isauthor" style="width: 310px" placeholder="请选择">
              <el-option label="是" value="1"></el-option>
              <el-option label="否" value="0"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="发表刊物">
            <el-input v-model="form.publication"/>
          </el-form-item>
          <el-form-item label="出版社">
            <el-input v-model="form.press"/>
          </el-form-item>
          <el-form-item label="发表时间">
            <el-col :span="11">
              <el-date-picker v-model="form.pubtime" value-format=" yyyy-MM-dd " format="yyyy-MM-dd " type="date" placeholder="选择日期" style="width: 60%;"/>
            </el-col>
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="addVisible = false">取 消</el-button>
          <el-button type="primary" @click="addClassSuccess">确 定</el-button>
        </div>
      </el-dialog>
    </div>
    <div>
      <el-dialog :visible.sync="changeVisible" title="修改论文发表情况">
        <el-form ref="form" :model="form" label-width="100px">
          <el-form-item label="论文名称">
            <el-input v-model="form.papername" placeholder="大数据研究" />
          </el-form-item>
          <el-form-item label="是否第一作者">
            <el-select v-model="form.isauthor" style="width: 310px" placeholder="是">
              <el-option label="是" value="1"></el-option>
              <el-option label="否" value="0"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="发表刊物">
            <el-input v-model="form.publication" placeholder="大数据研究" />
          </el-form-item>
          <el-form-item label="出版社">
            <el-input v-model="form.press" placeholder="大数据研究" />
          </el-form-item>
          <el-form-item label="发表时间">
            <el-col :span="11">
              <el-date-picker v-model="form.pubtime" value-format=" yyyy-MM-dd " format="yyyy-MM-dd " type="date" placeholder="2019-7-4" style="width: 60%;"/>
            </el-col>
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="changeVisible = false">取 消</el-button>
          <el-button type="primary" @click="changeSuccess">确 定</el-button>
        </div>
      </el-dialog>
    </div>
    <div>
      <el-dialog :visible.sync="addMatchVisible" title="新增参加比赛情况">
        <el-form ref="form" :model="form" label-width="100px">
          <el-form-item label="比赛名称">
            <el-input v-model="form.papername"/>
          </el-form-item>
          <el-form-item label="比赛时间">
            <el-col :span="11">
              <el-date-picker v-model="form.pubtime" value-format=" yyyy-MM-dd " format="yyyy-MM-dd " type="date" placeholder="选择日期" style="width: 60%;"/>
            </el-col>
          </el-form-item>
          <el-form-item label="比赛级别">
            <el-input v-model="form.papername"/>
          </el-form-item>
          <el-form-item label="作品名称">
            <el-input v-model="form.publication"/>
          </el-form-item>
          <el-form-item label="获奖情况">
            <el-input v-model="form.press"/>
          </el-form-item>

        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="addMatchVisible = false">取 消</el-button>
          <el-button type="primary" @click="addMatchSuccess">确 定</el-button>
        </div>
      </el-dialog>
    </div>
    <div>
      <el-dialog :visible.sync="changematchVisible" title="修改比赛情况">
        <el-form ref="form" :model="form" label-width="100px">
          <el-form-item label="比赛名称">
            <el-input v-model="form.papername"/>
          </el-form-item>
          <el-form-item label="比赛时间">
            <el-col :span="11">
              <el-date-picker v-model="form.pubtime" value-format=" yyyy-MM-dd " format="yyyy-MM-dd " type="date" placeholder="选择日期" style="width: 60%;"/>
            </el-col>
          </el-form-item>
          <el-form-item label="比赛级别">
            <el-input v-model="form.papername"/>
          </el-form-item>
          <el-form-item label="作品名称">
            <el-input v-model="form.publication"/>
          </el-form-item>
          <el-form-item label="获奖情况">
            <el-input v-model="form.press"/>
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="changematchVisible = false">取 消</el-button>
          <el-button type="primary" @click="changematchSuccess">确 定</el-button>
        </div>
      </el-dialog>
    </div>
    <div>
      <el-dialog :visible.sync="organizationVisible" title="新增组织指导课外活动情况">
        <el-form ref="form" :model="form" label-width="100px">
          <el-form-item label="起始时间">
            <el-col :span="11">
              <el-date-picker v-model="form.pubtime" value-format=" yyyy-MM-dd " format="yyyy-MM-dd " type="date" placeholder="选择日期" style="width: 60%;"/>
            </el-col>
          </el-form-item>
          <el-form-item label="终止时间">
            <el-col :span="11">
              <el-date-picker v-model="form.pubtime" value-format=" yyyy-MM-dd " format="yyyy-MM-dd " type="date" placeholder="选择日期" style="width: 60%;"/>
            </el-col>
          </el-form-item>
          <el-form-item label="组织名称">
            <el-input v-model="form.papername"/>
          </el-form-item>
          <el-form-item label="参加人数">
            <el-input v-model="form.publication"/>
          </el-form-item>
          <el-form-item label="活动次数">
            <el-input v-model="form.press"/>
          </el-form-item>
          <el-form-item label="活动内容">
            <el-input v-model="form.press"/>
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="organizationVisible = false">取 消</el-button>
          <el-button type="primary" @click="addOrganizationSuccess">确 定</el-button>
        </div>
      </el-dialog>
    </div>
    <div>
      <el-dialog :visible.sync="corganizationVisible" title="修改组织指导课外活动情况">
        <el-form ref="form" :model="form" label-width="100px">
          <el-form-item label="起始时间">
            <el-col :span="11">
              <el-date-picker v-model="form.pubtime" value-format=" yyyy-MM-dd " format="yyyy-MM-dd " type="date" placeholder="选择日期" style="width: 60%;"/>
            </el-col>
          </el-form-item>
          <el-form-item label="终止时间">
            <el-col :span="11">
              <el-date-picker v-model="form.pubtime" value-format=" yyyy-MM-dd " format="yyyy-MM-dd " type="date" placeholder="选择日期" style="width: 60%;"/>
            </el-col>
          </el-form-item>
          <el-form-item label="组织名称">
            <el-input v-model="form.papername"/>
          </el-form-item>
          <el-form-item label="参加人数">
            <el-input v-model="form.publication"/>
          </el-form-item>
          <el-form-item label="活动次数">
            <el-input v-model="form.press"/>
          </el-form-item>
          <el-form-item label="活动内容">
            <el-input v-model="form.press"/>
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="corganizationVisible = false">取 消</el-button>
          <el-button type="primary" @click="changcOrganizationSuccess">确 定</el-button>
        </div>
      </el-dialog>
    </div>
    <div>
      <el-dialog :visible.sync="further" title="新增进修教育情况">
        <el-form ref="form" :model="form" label-width="100px">
          <el-form-item label="起始时间">
            <el-col :span="11">
              <el-date-picker v-model="form.pubtime" value-format=" yyyy-MM-dd " format="yyyy-MM-dd " type="date" placeholder="选择日期" style="width: 60%;"/>
            </el-col>
          </el-form-item>
          <el-form-item label="终止时间">
            <el-col :span="11">
              <el-date-picker v-model="form.pubtime" value-format=" yyyy-MM-dd " format="yyyy-MM-dd " type="date" placeholder="选择日期" style="width: 60%;"/>
            </el-col>
          </el-form-item>
          <el-form-item label="进修单位名称">
            <el-input v-model="form.papername"/>
          </el-form-item>
          <el-form-item label="进修内容">
            <el-input v-model="form.publication"/>
          </el-form-item>
          <el-form-item label="进修形式">
            <el-input v-model="form.press"/>
          </el-form-item>
          <el-form-item label="完成课时">
            <el-input v-model="form.press"/>
          </el-form-item>
          <el-form-item label="结业时间">
            <el-input v-model="form.press"/>
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="further = false">取 消</el-button>
          <el-button type="primary" @click="addFurtherSuccess">确 定</el-button>
        </div>
      </el-dialog>
    </div>
    <div>
      <el-dialog :visible.sync="changeFurther" title="修改进修教育情况">
        <el-form ref="form" :model="form" label-width="100px">
          <el-form-item label="起始时间">
            <el-col :span="11">
              <el-date-picker v-model="form.pubtime" value-format=" yyyy-MM-dd " format="yyyy-MM-dd " type="date" placeholder="选择日期" style="width: 60%;"/>
            </el-col>
          </el-form-item>
          <el-form-item label="终止时间">
            <el-col :span="11">
              <el-date-picker v-model="form.pubtime" value-format=" yyyy-MM-dd " format="yyyy-MM-dd " type="date" placeholder="选择日期" style="width: 60%;"/>
            </el-col>
          </el-form-item>
          <el-form-item label="进修单位名称">
            <el-input v-model="form.papername"/>
          </el-form-item>
          <el-form-item label="进修内容">
            <el-input v-model="form.publication"/>
          </el-form-item>
          <el-form-item label="进修形式">
            <el-input v-model="form.press"/>
          </el-form-item>
          <el-form-item label="完成课时">
            <el-input v-model="form.press"/>
          </el-form-item>
          <el-form-item label="结业时间">
            <el-input v-model="form.press"/>
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="changeFurther = false">取 消</el-button>
          <el-button type="primary" @click="changFurtherSuccess">确 定</el-button>
        </div>
      </el-dialog>
    </div>
  </div>
</template>

<script>
export default {
name: "scientificTable",
  data(){
    return{
      addVisible:false,
      changeVisible:false,
      addMatchVisible:false,
      changematchVisible:false,
      organizationVisible:false,
      corganizationVisible:false,
      further:false,
      changeFurther:false,
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
      jinxiuData:[{
        bdate:'2020-6-5',
        fdate:'2020-7-1',
        name:'百度',
        content:'人工智能',
        xingshi:'讲座',
        completenum:'10',
        cdate: '2020-7-1'
      },
        {
          bdate:'2020-3-5',
          fdate:'2020-4-1',
          name:'清华大学',
          content:'工程伦理',
          xingshi:'授课',
          completenum:'10',
          cdate:'2020-4-1'
        },
        {
          bdate:'2020-6-5',
          fdate:'2020-7-1',
          name:'北京大学',
          content:'自然语言处理',
          xingshi:'合作',
          completenum:'10',
          cdate:'2020-7-1'
        },

      ],
      activeData:[{
        bdate:'2020-6-5',
        fdate:'2020-7-1',
        activename:'校外公司培训',
        peoplenum:'20',
        activenum:'2',
        activecontent:'熟悉matlab软件'
      },
        {
          bdate:'2019-6-5',
          fdate:'2019-6-20',
          activename:'参观软件园',
          peoplenum:'20',
          activenum:'2',
          activecontent:'参观'
        },
        {
          bdate:'2019-9-5',
          fdate:'2019-9-20',
          activename:'大数据讲座',
          peoplenum:'10',
          activenum:'3',
          activecontent:'大数据刘俊明的讲座'
        }
      ],
      matchData: [{
        matchname:'计算机设计大赛',
        date:'2019-6-7',
        matchclass:'国家级',
        name:'智能问答系统',
        class:'北京市一等奖',
      },
        {
          matchname:'ACM国际大学生程序设计竞赛',
          date:'2019-9-1',
          matchclass:'国际级',
          name:'推荐系统',
          class:'一等奖',
        },
        {
          matchname:'计算机设计大赛',
          date:'2019-6-7',
          matchclass:'国家级',
          name:'大数据可视化',
          class:'省一等奖',
        },
        {
          matchname:'计算机设计大赛',
          date:'2019-6-7',
          matchclass:'国家级',
          name:'智能机器人',
          class:'国家二等奖',
        }
      ],
      paperData: [{
        papername: '大数据研究',
        isauthor: '是',
        publication: '软件学报',
        press:'北京出版社',
        pubtime:'2020-8-20',
      }, {
        papername: '自然语言问答系统',
        isauthor: '是',
        publication: '计算机学报',
        press:'北京出版社',
        pubtime:'2020-8-20',
      }, {
        papername: '供电系统研究',
        isauthor: '是',
        publication: '辅助设计',
        press:'北京出版社',
        pubtime:'2020-8-20',
      },{
        papername: '大数据下的可视化进展',
        isauthor: '是',
        publication: '计算机学报',
        press:'北京出版社',
        pubtime:'2020-8-20',
      },{
        papername: '大屏可视化',
        isauthor: '是',
        publication: '计算机学报',
        press:'北京出版社',
        pubtime:'2020-8-20',
      },{
        papername: '智慧校园建设',
        isauthor: '是',
        publication: '计算机学报',
        press:'北京出版社',
        pubtime:'2020-8-20',
      }
      ],
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
    addMatch(){
      this.addMatchVisible = true;
    },
    addMatchSuccess(){
      this.addMatchVisible = false;
      this.$message({
          type: 'success',
          message: '新增成功',
        }
      )
    },
    changematch(){
      this.changematchVisible = true;
    },
    changeSuccess(){
      this.changeVisible = false;
      this.$message({
        type: 'success',
        message: '修改成功',
      })
    },
    changematchSuccess(){
      this.changematchVisible = false;
      this.$message({
        type: 'success',
        message: '修改成功',
      })
    },
    addOrganization(){
      this.organizationVisible = true;
    },
    addOrganizationSuccess(){
      this.organizationVisible = false;
      this.$message({
          type: 'success',
          message: '新增成功',
        }
      )
    },
    changcOrganization(){
      this.corganizationVisible = true;
    },
    changcOrganizationSuccess(){
      this.corganizationVisible = false;
      this.$message({
          type: 'success',
          message: '修改成功',
        }
      )
    },
    addFurther(){
      this.further = true
    },
    changeFurtherinfo(){
      this.changeFurther = true
    },
    addFurtherSuccess(){
      this.further = false;
      this.$message({
          type: 'success',
          message: '新增成功',
        }
      )
    },
    changFurtherSuccess(){
      this.changeFurther = false;
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
