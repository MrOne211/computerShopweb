<template>
  <div >
    <el-row>
      <el-col :span="24">
        <el-card :body-style="{ padding: '0px' }" v-if="isLogin" style="margin:5px 0 5px 0;width: auto;height: 205px;background-color: #cecece">
          <img :src="src" class="image">
          <div style="padding: 14px; text-align: center">
            <div><span style="text-align: center;font-weight: bolder">当前餐桌:{{table}}</span></div>
            <span style="text-align: center;font-weight: bolder">点餐员:{{name}}</span>
            <div><span style="text-align: center;font-weight: bolder">职员评分:{{rate}}分</span></div>
            <div class="bottom clearfix">
              <time class="time">{{ currentDate }}</time>
            </div>
          </div>
        </el-card>
        <el-card :body-style="{ padding: '0px' }" v-if="!isLogin" style="margin:5px 0 5px 0;width: auto;height: 205px;background-color: #cecece">
          <el-image
            style="margin-left: 53px;width: 50%;height: 90px"
            src="/static/images/default/isNotLogin.png"
            fit="fill "></el-image>
        <div style="padding: 14px; text-align: center">
          <el-button type="primary" @click="goLogin" icon="el-icon-switch-button">请登录</el-button>
          <div class="bottom clearfix">
            <time class="time">{{ currentDate }}</time>
          </div>
        </div>
      </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
    export default {
        name: "UserMsg",
      data() {
        return {
          currentDate: new Date(),
          axiosParams:new Object(),
          src:'',
          name:'',
          rate:'',
          table:'无',
          isLogin:false
        };
      },
      methods:{
          ajaxCall(){
            this.$axios.post("/iorder/User/findUser")
            .then(res=>{
              if (res.data.user!=null){
                this.isLogin = true;
                this.src = "."+res.data.user.img;
                this.name = res.data.user.name;
                this.rate = res.data.user.rate;
              }else {
                this.isLogin = false;
              }
            })
          },
        goLogin(){
            this.$router.push("/");
        }
      },
      mounted() {
          this.ajaxCall();
          eventBus.$on('table',res=>{
            this.axiosParams = new Object();
            if (!isNaN(res)){
              this.axiosParams.id = res;
              this.$axios.post("/iorder/Table/findById",this.axiosParams)
                .then(res=>{
                  this.table = res.data.table.tableName;
                })
                .catch(()=>{})
            }

        })
      }
    }
</script>

<style scoped>
  .time {
    font-size: 13px;
    color: white;
  }

  .bottom {
    margin-top: 13px;
    line-height: 12px;
  }

  .image {
    width: 150px;
    height: 80px;
    margin: 0 auto;
    display: block;
  }

  .clearfix:before,
  .clearfix:after {
    display: table;
    content: "";
  }

  .clearfix:after {
    clear: both
  }
</style>
