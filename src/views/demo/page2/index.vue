<template>
  <d2-container>
             <el-breadcrumb separator-class="el-icon-arrow-right">

      <!-- 这里是， 点进来二级菜单之后 -->
        <el-breadcrumb-item :to="{ path: '/index' }">首页</el-breadcrumb-item>
            <el-breadcrumb-item>Statistics show</el-breadcrumb-item>
         </el-breadcrumb>
    <el-divider></el-divider>

    <el-card>
      <el-row :gutter="30">
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
      </el-row>
    </el-card>
    <el-card>
      <my-bread level1="statistics"></my-bread>
      <el-row>
          <div class="mainChartPart">
            <div id="mainChart" style="width: 90%; height: 350%"></div>
          </div>
      </el-row>
      <el-row>

          <div class="mainChartPart">
            <div id="myChart" style="width: 80%; height: 300%"></div>
          </div>
      </el-row>
    </el-card>
  </d2-container>
</template>

<script>
var echarts = require("echarts");

export default {
  name: "page2",
  created(){

  },
  data(){
    return{
          queryInfo:{
              query:"",//查询信息
              pageNum:1,//当前页
              pageSize:5,//每页的最大数
              date:["2022-05-01 00:00:00","2022-11-21 08:00:00"],//"yyyy-MM-dd HH:mm:ss"
              // time:"",
          },
          numberOfEach:[],

    }
  },
  mounted() {
    this.useEchart();
    this.createChart();
  },
  methods: {
    async createChart(){
        var mainChart=echarts.init(document.getElementById("myChart"));
        var option = {
        // title: {
        //         text: 'Proportion of all data stream',
        //         // subtext: 'What users care most about voice assistants around the world',
        //         left: 'center',
        //         textStyle: { color: "#000" },

        //     },
        tooltip: {
          // trigger: 'item'
        },
        legend: {
          top: "5%",
          left: "center",
        },
        series: [
          {
            name: "访问来源",
            type: "pie",
            radius: ["20%", "70%"],
            avoidLabelOverlap: false,
            itemStyle: {
              borderRadius: 10,
              borderColor: "#000",
              borderWidth: 2,
            },
            label: {
              show: false,
              position: "center",
            },
            emphasis: {
              label: {
                show: true,
                fontSize: "30",
                fontWeight: "bold",
              },
            },
            labelLine: {
              show: false,
            },
            data: [
              { value: this.numberOfEach.hum1, name: "hum1" },
              { value: this.numberOfEach.temp1, name: "temp1" },
              { value: this.numberOfEach.wlwtest, name: "wlwtest" },
              // { value: 2, name: "hum1" },
              // { value:3, name: "temp1" },
              // { value: 4, name: "wlwtest" },
            ],
          },
        ],
      };
      mainChart.setOption(option);
    },
    async useEchart() {
      var mainChart = echarts.init(document.getElementById("mainChart"));
      var option = {
        title: {
                text: 'Proportion of all data stream',
                // subtext: 'What users care most about voice assistants around the world',
                left: 'left',
                textStyle: { color: "#000" },

            },
        legend: {},
        tooltip: {
          trigger: "axis",
          showContent: false,
        },
        dataset: {
          source: [
            ["product", "1", "2", "3", "4", "5", "6","7"],
            ["hum1", 0.15,0.1,0,0,0,0,0],
            ["temp1", 0.17,0,0.2,0.32,0.3,0.4,0],
            ["wlwtest",0.1,0.23,0.10,0,0.45,0,0,0],
          ],
        },
        xAxis: { type: "category" },
        yAxis: { gridIndex: 0 },
        grid: { top: "55%" },
        series: [
          {
            type: "line",
            smooth: true,
            seriesLayoutBy: "row",
            emphasis: { focus: "series" },
          },
          {
            type: "line",
            smooth: true,
            seriesLayoutBy: "row",
            emphasis: { focus: "series" },
          },
          {
            type: "line",
            smooth: true,
            seriesLayoutBy: "row",
            emphasis: { focus: "series" },
          },
          {
            type: "line",
            smooth: true,
            seriesLayoutBy: "row",
            emphasis: { focus: "series" },
          },
          {
            type: "pie",
            id: "pie",
            radius: "30%",
            center: ["50%", "25%"],
            emphasis: {
              focus: "self",
            },
            label: {
              formatter: "{b}: {@2012} ({d}%)",
            },
            encode: {
              itemName: "product",
              value: "1",
              tooltip: "1",
            },
          },
        ],
        
      };
// var option = {
//         title: {
//                 text: 'Proportion of all data stream',
//                 // subtext: 'What users care most about voice assistants around the world',
//                 left: 'center',
//                 textStyle: { color: "#000" },

//             },
//         tooltip: {
//           // trigger: 'item'
//         },
//         legend: {
//           top: "5%",
//           left: "center",
//         },
//         series: [
//           {
//             name: "访问来源",
//             type: "pie",
//             radius: ["0%", "70%"],
//             avoidLabelOverlap: false,
//             itemStyle: {
//               borderRadius: 10,
//               borderColor: "#000",
//               borderWidth: 2,
//             },
//             label: {
//               show: false,
//               position: "center",
//             },
//             emphasis: {
//               label: {
//                 show: true,
//                 fontSize: "30",
//                 fontWeight: "bold",
//               },
//             },
//             labelLine: {
//               show: false,
//             },
//             data: [
//               { value: this.numberOfEach.hum1, name: "hum1" },
//               { value: this.numberOfEach.temp1, name: "temp1" },
//               { value: this.numberOfEach.wlwtest, name: "wlwtest" },
//               // { value: 2, name: "hum1" },
//               // { value:3, name: "temp1" },
//               // { value: 4, name: "wlwtest" },
//             ],
//           },
//         ],
//       };

      mainChart.setOption(option);
    },
    async searchClick(){
          
            console.log(this.queryInfo);
            console.log("this.queryInfo"+this.queryInfo.query+"    queryInfo  "+this.queryInfo.date);
            console.log("-0---------!!!!--");
            const {data: res}=await this.$http.post("getAll",this.queryInfo);
            this.numberOfEach=res.numberOfEach;
            console.log(this.numberOfEach);
            console.log(typeof this.numberOfEach);
            console.log(this.numberOfEach.hum1);
            console.log(typeof this.numberOfEach.hum1);

            // for(var i=0;i<this.numberOfEach.length;i++){
            //   console.log(this.numberOfEach[i]);
            //   console.log(i);
            // }

            if(res.numberOfEach==null){
                  return this.$message.error("操作失败!!~");
            }
            this.$message.success("操作成功!!~");
            
            this.useEchart();
            this.createChart();
            

        },
  },
};
</script>
<style  scoped>
.pie1,
.pie3,
.pie6,
.mainChartPart {
  background-color: #fff;
}
</style>