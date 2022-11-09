<template>
  <d2-container>

    <div>
        

         <el-breadcrumb separator-class="el-icon-arrow-right">

      <!-- 这里是， 点进来二级菜单之后 -->
        <el-breadcrumb-item :to="{ path: '/index' }">首页</el-breadcrumb-item>
            <el-breadcrumb-item>Device of statistics</el-breadcrumb-item>
         </el-breadcrumb>
         
    <el-divider></el-divider>
         
    <!-- 视图区 -->
    <el-card>
      <!-- 间隙 ,此处为按钮和搜索框间的距离-->
      <el-row :gutter="30">
        <!-- 列宽 -->
        <!-- <div style="width:20%"> -->
        <el-col :span="6">
          <!-- 搜索添加 v-model是将按钮提供数据绑定，@clear是将搜索栏清楚后，也要将搜出来的数据清楚 -->
            <el-input placeholder="请输入temp1 wlwtest hum1" v-model="queryInfo.query" clearable @clear="getDeviceList">
            <!-- <el-input placeholder="请输入搜索内容" > -->
              <el-button slot="append" icon="el-icon-search" @click="getDeviceList"></el-button>
            </el-input>
         </el-col>
        <!-- </div> -->
        
        <!-- 时间的选择 -->
        <el-col :span="7">
            <!-- <div class="block" > -->
                <el-date-picker
                v-model="queryInfo.date"
                value-format="yyyy-MM-dd HH:mm:ss"
                type="datetimerange"
                align="right"
                start-placeholder="开始日期"
                end-placeholder="结束日期"
                :default-time="['12:00:00', '08:00:00']">
                </el-date-picker>

        </el-col>

         <el-col :span="2">
            <el-button type="primary" icon="el-icon-search" @click="searchClick" round>search</el-button>
         </el-col>

         <el-col :span="2">
            <el-button type="info" icon="el-icon-refresh" @click="searchDeClick" round>clear</el-button>
         </el-col>

         <el-col :span="2">
            <el-button type="success" icon="el-icon-document-add" @click="exportExcel" round>saveCSV</el-button>
         </el-col>

      </el-row>



      <!-- 用户列表。用于改、查 , 其中，border是边框，stripe是隔行变色 -->
      <el-table :data="result" id="exportTable"  :summary-method="getSummaries" show-summary  border stripe>
        <!--索引列 -->
          <el-table-column type="index"></el-table-column>

          <!-- 注意顺序,select 的时候不要* 并且 -->
          <el-table-column label="Time" prop="at"></el-table-column>

          <el-table-column label="Value" prop="value"></el-table-column>



      </el-table>

      <!-- 分页组件 size-change 每页最大的变化 current-change 当前最大变化 layout功能组件 -->
        <div>
            <el-pagination
                  @size-change="handleSizeChange"
                  @current-change="handleCurrentChange"
                  :current-page="queryInfo.pageNum"
                  :page-sizes="[1, 2, 5, 100]"
                  :page-size="queryInfo.pageSize"
                  layout="total"
                  :total="total"
                ></el-pagination>
        </div>

    </el-card>


         
    </div>
  </d2-container>

</template>

<script>
import * as XLSX from 'xlsx'
import FileSaver from 'file-saver'
export default{
     created(){
      // this.getDeviceList();
    },
    data(){
        return {
             // 查询信息的实体
          queryInfo:{
              query:"",//查询信息
              pageNum:1,//当前页
              pageSize:5,//每页的最大数
              date:["2022-05-01 00:00:00","2022-11-21 08:00:00"],//"yyyy-MM-dd HH:mm:ss"
              // time:"",
          },


          deviceList:[],//用户列表
          Value:0,
          Time:"",
          Id:"",
          max:0,
          min:0,
          avg:0,
          total:0,//最大的记录的数量，
        //   addDialogVisible:false,//添加用户的对话框的显示或隐藏
        };
    },
    methods:{

        async getDeviceList(){

            // const {data:res}=await this.$http.get("getDeviceInfo",{params:this.queryInfo});
            console.log(this.queryInfo);
            
            const {data: res}=await this.$http.post("getValue",this.queryInfo);

            this.total=res.number;
            this.result=res.result;
            this.max=res.max;
            this.min=res.min;
            this.avg=res.avg;

            console.log("total   ",res.number);
            console.log("result",res.result);
        },

  // 当页面变化的时候
        handleSizeChange(newSize){
          this.queryInfo.pageSize=newSize;
          this.getDeviceList();
        },
        // pageNum 的触发动作
        handleCurrentChange(newPage){
            this.queryInfo.pageNum=newPage;
            this.getDeviceList();
        },

        async searchClick(){
          
            console.log(this.queryInfo);
            console.log("this.queryInfo"+this.queryInfo.query+"    queryInfo  "+this.queryInfo.date);
            console.log("-0-----------");
            const {data: res}=await this.$http.post("getValue",this.queryInfo);
            
            this.total=res.number;
            this.result=res.result;

            this.max=res.max;
            this.min=res.min;
            this.avg=res.avg;

            console.log("total   ",res.number);
            console.log("result",res.result);
            console.log("max ",res.max);

            if(res.number==0){
                  return this.$message.error("操作失败!!~");
            }
            this.$message.success("操作成功!!~");
            
            

        },

        searchDeClick(){
            this.queryInfo={
              query:"",
              pageNum:1,
              pageSize:5,
              date:'',
            },
            // this.getDeviceList();
            this.result=[];
            console.log("llllll");
        },

        async saveClick(){
          console.log("hhhhhhhhhhhhhhhhhh");
            const {data: res}=await this.$http.post("saveFile",this.queryInfo);
            this.total=res.number;
            this.result=res.result;
            console.log("total   ",res.number);
            console.log("result",res.result);

            if(res.number==0){
                  return this.$message.error("操作失败!!~");
            }
            this.$message.success("操作成功!!~");
        },
        
           exportExcel () {
            console.log(" i am here");
              /* generate workbook object from table */
              var xlsxParam = { raw: true } // 导出的内容只做解析，不进行格式转换
              console.log("1");
              var table = document.querySelector('#exportTable').cloneNode(true)
              console.log("12");
              
              // table.removeChild(table.querySelector('.el-table__fixed')) //这里是双下划线
              
              var wb = XLSX.utils.table_to_book(table, xlsxParam)
              console.log("123");

              /* get binary string as output */
              var wbout = XLSX.write(wb, { bookType: 'xlsx', bookSST: true, type: 'array' })
              try {
                  FileSaver.saveAs(new Blob([wbout], { type: 'application/octet-stream' }), this.queryInfo.query+'.xlsx')
                  console.log("hhhyeahhhhhheah");
              } catch (e) {
              if (typeof console !== 'undefined') {
                  console.log(e, wbout)
              }
              }
                return wbout
            },
            getSummaries(){

              const sums = [ ];
              sums[1]="value max: "+this.max+"  ";
              sums[1]+="    value min: "+this.min;
              sums[2]="value avg: "+this.avg;
              return sums;

            }
        
    },
}
</script>

<style  scoped>

</style>