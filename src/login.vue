<template>
  <div id="app">
    <div style="width: 600px;margin: 200px auto 0 auto;border: 1px solid #e3e3e3;border-radius: 3px; background: white;">
      <el-row style="margin:30px;">
        <div>
          <el-row :gutter="20">
            <el-col :span="24">
              <img src="./assets/home_logo.png" style="width:170px;height: 50px">
              <span class="sm-title">
                <small style="color: gray;font-size:20px">电商管理系统</small>
                &nbsp;&nbsp;<small style="font-size:20px">用户登录</small>
              </span>
            </el-col>
          </el-row>
        </div>
        <div>
          <el-row>
            <el-col :span="2">
            <img src="./assets/icon_uname.png" style="vertical-align: middle;margin-top: 30px;">
            </el-col>
            <el-col :span="22">
             <el-input :placeholder="isPhoneLogin?'请输入手机号':'请输入账号'" v-model="from.userName" style='margin-top: 30px;'></el-input>
            </el-col>
          </el-row>
          <el-row>
            <el-col :span=2>
            <img src="./assets/icon_pswd.png" style="vertical-align: middle;margin-top: 30px;">
            </el-col>
            <el-col v-if="!isPhoneLogin" :span="22" >
             <el-input placeholder="请输入密码" type='password' style='margin-top: 30px;' v-model="from.password" @keyup.enter.native="keyup13()"></el-input>
            </el-col>
            <el-col v-if="isPhoneLogin" :span="15" >
              <el-input placeholder="请输入手机验证码" style='margin-top: 30px;' v-model="from.code" @keyup.enter.native="keyup13()"></el-input>
            </el-col>
            <el-col v-if="isPhoneLogin" :span="7" >
              <el-button style="margin-top: 30px; margin-left: 20px;float: right" type="primary" :disabled="is_disabled" @click="checkcode()">{{code_txt}}</el-button>
            </el-col>
          </el-row>
           <el-row>
            <el-col :span=2>
            <img src="./assets/icon_pswd.png" style="vertical-align: middle;margin-top: 30px;">
            </el-col>
            <el-col :span="15" >
              <el-input placeholder="请输入验证码" style='margin-top: 30px;' v-model="from.vcode" @keyup.enter.native="keyup13()"></el-input>
            </el-col>
            <el-col :span="7" >
              <img width='130px' style="margin-top:30px;" :src="vcodePath" :click="getVcodePath"/>
            </el-col>
          </el-row>
          <el-row>
            <el-col :span="12">
             <el-button type="primary" style="margin-top: 30px;width: 200px;padding: 10px;" @click="handleSubmit()">登录</el-button>
            </el-col>
            <el-col :span="12">
               <a href="javascript:;" @click="isPhoneLogin = !isPhoneLogin" style="color: #1d90e6;font-size: 0.8em;float: right;margin-left: 20px;margin-top: 50px;">
                 {{isPhoneLogin?'账号登录':'短信快捷登录'}}
               </a>
              <a style="color: #a0a0a0;font-size: 0.8em;float: right;margin-left: 20px;margin-top: 50px;">
                 <img src="./assets/icon_info.png" style="vertical-align: middle;"> 阅读服务协议
               </a>
            </el-col>
          </el-row>
         </div>
      </el-row>
    </div>
    <div style="width: 600px;margin: 10px auto 0 auto;">
      <el-row>
        <el-col :span="14" style="color: #c0c0c0;font-size: 0.8em;">
          粤ICP备05071255号-2 | Copyright &copy;2016
        </el-col>
         <el-col :span="10" style="color: #c0c0c0;font-size: 0.8em;text-align:right;">
        广东通驿高速公路服务区有限公司
        </el-col>
      </el-row>
    </div>
  </div>

</template>
<script>
module.exports = {
  data: function () {
    return {
       from: {
          userName: '',
          password:'',
          code: '',
          codeToken: '',
          vcode: '',
          vcodeToken: ''
        },
        vcodePath: '',
        code_txt: '获取手机验证码',
        is_disabled: false,
        tip:false,
        isPhoneLogin: false
    }
  },
  methods: {
     getVcodePath() {
       var me = this;
        	me.$get2('pc/validateCode/getValidateCode', {}).then2(function(result){
                me.vcodePath = result.data.path;
                me.from.vcodeToken = result.data.vcodeToken;
            });
        },
    checkcode() {
      var me = this;
            var phone = me.from.userName;
            var phone_pattern =/^[1][3,4,5,7,8][0-9]{9}$/;

            if (phone == '' || phone == null) {
                this.$notify({
                    title: '警告',
                    message: '手机号不能为空',
                    type: 'warning'
                  });
                return;
            }
            if (!phone_pattern.test(phone)) {
               this.$notify({
                    title: '警告',
                    message: '手机号码格式不正确',
                    type: 'warning'
                  });
                return;
            }

            me.$get2('admin/login/getCode', {phone: phone}).then2(function(response){
              if(response.data.error == 0){
                    me.is_disabled = true;
                    me.from.codeToken=response.data.codeToken;
                    me.from.phone = response.data.phone;
                    var time = 60;
                    var init = setInterval(function(){
                        if (time >= 0) {
                          me.code_txt = time + '秒后重新发送';
                        } else {
                          me.code_txt = '获取验证码';
                          me.is_disabled = false;
                          clearInterval(init);
                        }
                        time--;
                    }, 1000);
                } else {
                  me.$notify({
                    title: '警告',
                    message: '发送短信失败',
                    type: 'warning'
                  });
                  return;
                }
            },function(response){
              me.$notify({
                title: '警告',
                message: '发送短信失败',
                type: 'warning'
              });
          });
    },
    handleSubmit() {
      if(this.isPhoneLogin) {
        if(!this.from.userName){
          this.$notify({
              title: '警告',
              message: '手机号不可为空！',
              type: 'warning'
            });
          return;
        }

        if(!this.from.code){
          this.$notify({
              title: '警告',
              message: '手机验证码不可为空！',
              type: 'warning'
            });
          return;
        }
        var me = this;
        me.$get2('admin/login/phoneLogin', this.from)
        .then2(function(response){
	        if(response.data.error != 0){
	          me.$notify({
	            title: '警告',
	            message: response.data.errmsg,
	            type: 'warning'
            });
            me.getVcodePath();
	          return;
	        }
          me.$notify({
	          title: '登录成功',
	          message: '',
	          type: 'success'
	        });
	        localStorage.token=response.data.adminToken;
	        localStorage.userName=response.data.userName;
          window.location.href='#/home';
          location.reload();
        },function(response){
          me.$notify({
            title: '警告',
            message: '登录失败,用户名或密码错误!',
            type: 'warning'
          });
          me.getVcodePath();
          localStorage.token=""
        });
      } else {
        if(!this.from.userName){
          this.$notify({
              title: '警告',
              message: '用户名不可为空！',
              type: 'warning'
            });
          return;
        }

        if(!this.from.password){
          this.$notify({
              title: '警告',
              message: '用户密码不可为空！',
              type: 'warning'
            });
          return;
        }

      var me = this;
      me.$get2('admin/login/token',this.from)
        .then2(function(response){
	        if(response.data.error != 0){
	          me.$notify({
	            title: '警告',
	            message: response.data.errmsg || '登录失败,用户名或密码错误!',
	            type: 'warning'
            });
            me.getVcodePath();
	          return;
	        }
          me.$notify({
	          title: '登录成功',
	          message: '',
	          type: 'success'
	        });
	        localStorage.token=response.data.adminToken;
	        localStorage.userName=response.data.userName;
          window.location.href='#/home';
          location.reload();
        },function(response){
          me.$notify({
            title: '警告',
            message: '登录失败,用户名或密码错误!',
            type: 'warning'
          });
          me.getVcodePath();
          localStorage.token=""
        });
      }
    },
    keyup13:function () {
        this.handleSubmit();
    }
  },
  mounted() {
    this.getVcodePath();
  }
}
</script>
