<template>
<!-- 
  <div class="d2-page-cover">
    <div class="d2-page-cover__logo">
      <slot/>
    </div>
    <p class="d2-page-cover__sub-title">NB-IOT展示--作者：22班 17组 李方正 学号：2019213710</p>
    <p class="d2-page-cover__build-time">FINAL BUILD TIME {{$buildTime}}</p>
  </div> -->
  <div>
    <el-card > 
      <!-- <el-carousel :interval="4000" type="card" height="200px">
      <el-carousel-item v-for="item in 6" :key="item">
        <h3 class="medium">{{ item }}</h3>
      </el-carousel-item>
    </el-carousel> -->

    <el-carousel trigger="click" height="200px" :interval="3000" arrow="always"  type="card">
      <el-carousel-item v-for="item in imgList" :key="item.name">
        <img :src="item.src" style="height:100%;width:100%;" alt="图片丢失了" :title="item.title" />
      </el-carousel-item>
    </el-carousel>

      <el-row :gutter="25">
        <el-col :span="24">

      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <span>Humidity</span>
          <el-button style="float: right; padding: 3px 0" type="text" @click="refresh">refresh</el-button>
        </div>
        <div class="text item">
          {{ 'max humidity:'+ this.maxHum}}
          {{'now humidity:'+ this.nowHum}}
          {{'min humidity:'+ this.minHum}}

        </div>
        
        <!-- <div class="text item">
          {{ 'max temperature:'+ this.maxTemp}}
          {{'now temperature:'+ this.nowTemp}}
          {{'min temperature:'+ this.minTemp}}
        </div>
          <div class="text item">
          {{ 'now RFID:'+ this.nowRFID}}
          </div> -->
      </el-card>
    <el-divider></el-divider>


      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <span>Temperature</span>
          <el-button style="float: right; padding: 3px 0" type="text" @click="refresh">refresh</el-button>
        </div>
        
        <div class="text item">
         <div> {{ 'max temperature:'+ this.maxTemp}}</div>
         <div>  {{'now temperature:'+ this.nowTemp}}</div>
         <div>  {{'min temperature:'+ this.minTemp}}</div>
        </div>
      </el-card>
    <el-divider></el-divider>
      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <span>RFID</span>
          <el-button style="float: right; padding: 3px 0" type="text" @click="refresh">refresh</el-button>
        </div>
          <div class="text item">
          {{ 'now RFID:'+ this.nowRFID}}
          </div>
      </el-card>
        </el-col>




    
    </el-row>
    </el-card>
  
  </div>
</template>

<script>
// import { defineComponent } from '@vue/composition-api'

export default{
created(){
   this.getHumList();
   this.getTempList();
   this.getRFID();  
  },
mounted(){
  this.timer=setInterval(()=>{
    this.getHumList();
   this.getTempList();
   this.getRFID();  

  },3000);
},
    data(){
        return {

          imgList:[
              {
                  name: "lj",
                  src: require("E:\\物联网工程实践\\大作业\\frontend\\UI\\src\\assets\\svg-icons\\figure\\fig1.png"),
                  title: "这是lj.png"
              },
              {
                  name: "lj",
                  src: require("E:\\物联网工程实践\\大作业\\frontend\\UI\\src\\assets\\svg-icons\\figure\\fig2.png"),

                  // src: require("E:\物联网工程实践\大作业\frontend\UI\src\assets\svg-icons\figure\fig2.png"),
                  title: "这是lj.png"
              },
              {
                  name: "lj",
               
                  src: require("E:\\物联网工程实践\\大作业\\frontend\\UI\\src\\assets\\svg-icons\\figure\\fig3.png"),
               // src: require("E:\物联网工程实践\大作业\frontend\UI\src\assets\svg-icons\figure\fig3.png"),
                  title: "这是lj.png"
              },
              // {
              //     // name: "lj",
              //     // src: require("@/assets/lj.png"),
              //     // title: "这是lj.png"
              // }
          ],

          timer:null,
             // 查询信息的实体
          queryInfo:{
              query:"temp1",//查询信息
              pageNum:1,//当前页
              pageSize:5,//每页的最大数
              date:["2022-09-15 00:00:00","2022-12-21 08:00:00"],//"yyyy-MM-dd HH:mm:ss"
              // time:"",
          },

          queryInfohum:{
              query:"hum1",//查询信息
              pageNum:1,//当前页
              pageSize:5,//每页的最大数
              date:["2022-09-15 00:00:00","2022-12-21 08:00:00"],//"yyyy-MM-dd HH:mm:ss"
              // time:"",
          },
          
          queryInfoRFID:{
              query:"RFID",//查询信息
              pageNum:1,//当前页
              pageSize:5,//每页的最大数
              date:["2022-09-15 00:00:00","2022-12-21 08:00:00"],//"yyyy-MM-dd HH:mm:ss"
              // time:"",
          },

          deviceList:[],//用户列表
          Value:0,
          Time:"",
          Id:"",
          maxHum:0,
          minHum:0,
          avgHum:0,
          nowHum:0,
          maxTemp:0,
          minTemp:0,
          avgTemp:0,
          nowTemp:0,
          nowRFID:0,
          total:0,//最大的记录的数量，
        //   addDialogVisible:false,//添加用户的对话框的显示或隐藏
        };
    },
    methods:{
        refresh(){
          this.$router.go(0);
          this.getHumList();
          this.getTempList();
          this.getRFID();
        },
        async getHumList(){
            console.log("hhhhhhhhhh");
            // this.queryInfohum.query="hum1";
            // const {data:res}=await this.$http.get("getDeviceInfo",{params:this.queryInfo});
            // console.log(this.queryInfo.query);
            
            const {data: res}=await this.$http.post("getValue",this.queryInfohum);

            this.total=res.number;
            this.result=res.result;
            this.maxHum=res.max;
            this.minHum=res.min;
            this.avgHum=res.avg;
            this.nowHum=res.result[0]['value']
            // console.log("total   ",res.number);
            // console.log("result",res.result);
            console.log(res.result[0]['value'])

        },
        async getTempList(){
            console.log("hhhhhxxxxxxxxxxxxhhhhh");

            // this.queryInfo.query="temp1";
            // const {data:res}=await this.$http.get("getDeviceInfo",{params:this.queryInfo});
            console.log(this.queryInfo.query);
            
            const {data: res}=await this.$http.post("getValue2",this.queryInfo);

            this.total=res.number;
            this.result=res.result;
            this.maxTemp=res.max;
            this.minTemp=res.min;
            this.avgTemp=res.avg;
            this.nowTemp=res.result[0]['value']
            // console.log("total   ",res.number);
            // console.log("result",res.result);
            console.log("temp "+this.nowTemp);
        },
        async  getRFID(){

            // this.queryInfo.query="RFID";
            // const {data:res}=await this.$http.get("getDeviceInfo",{params:this.queryInfo});
            // console.log(this.queryInfo.query);
            
            const {data: res}=await this.$http.post("getValue3",this.queryInfoRFID);

            this.total=res.number;
            this.result=res.result;
            // this.min=res.min;
            // this.max=res.max;
            // this.avg=res.avg;
            this.nowRFID=res.result[0]['value']
            // console.log("total   ",res.number);
            console.log("result",res.result);
            console.log("now "+this.nowRFID);

        },
  }
}
</script>


<style lang="scss" scoped>
.d2-page-cover {
  @extend %full;
  @extend %unable-select;
  display: flex;
  flex-flow: column nowrap;
  justify-content: center;
  align-items: center;
  .d2-page-cover__logo {
    img {
      width: 200px;
    }
  }
  .d2-page-cover__title {
    margin: 0px;
    margin-bottom: 20px;
    font-weight: bold;
    color: $color-text-main;
  }
  .d2-page-cover__sub-title {
    margin: 0px;
    margin-bottom: 5px;
    color: $color-text-normal;
  }
  .d2-page-cover__build-time {
    margin: 0px;
    margin-bottom: 10px;
    font-size: 12px;
    color: $color-text-placehoder;
  }
}
  .text {
    font-size: 14px;
  }

  .item {
    margin-bottom: 18px;
  }

  .clearfix:before,
  .clearfix:after {
    display: table;
    content: "";
  }
  .clearfix:after {
    clear: both
  }

  .box-card {
    width: 400px;
  }





  .el-carousel__item h3 {
    color: #475669;
    font-size: 14px;
    opacity: 0.75;
    line-height: 200px;
    margin: 0;
  }
  
  .el-carousel__item:nth-child(2n) {
    background-color: #99a9bf;
  }
  
  .el-carousel__item:nth-child(2n+1) {
    background-color: #d3dce6;
  }
</style>
