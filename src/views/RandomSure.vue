<template>
  <div class="RandomSure">
    <div class="centerLogin">
      <p class="loginPwd">登录界面密码验证</p>
      <el-form ref="form" :model="form" label-width="80px" :rules="rules">
        <!--      用户名-->
        <el-form-item prop="username" style="line-height: 0px" label="用户名">
          <el-input v-model="form.username" placeholder="用户名"></el-input>
        </el-form-item>
        <!--      密码-->
        <el-form-item prop="password" style="line-height: 0px" label="密 码">
          <el-input v-model="form.password" placeholder="密码"></el-input>
        </el-form-item>
        <!-- 随机验证码 -->
        <el-form-item prop="verified" style="line-height:0px;" label="验证码">
          <el-input v-model="form.verified" ref="verified" placeholder="验证码" class="identifying"
          ></el-input>
          <div class="identify" @click="refreshCode">
            <sidentify :identifyCode="identifyCode"></sidentify>
          </div>
        </el-form-item>
      </el-form>
      <div class="login">
        <el-button type="warning" size="medium" @click="login('form')">登 录</el-button>
      </div>
    </div>

  </div>
</template>

<script>
import sidentify from "@/components/sidentify";

export default {
  components: {
    sidentify
  },
  data() {
    return {
      form: {
        verified: '',
        username: '',
        password: '',
      },
      rules: {
        username: [
          {
            required: true,
            message: '请输入用户名',
            trigger: 'blur'
          },
        ],
        password: [
          {
            required: true,
            message: '请输入密码',
            trigger: 'blur'
          },
        ],
        verified: [
          {
            required: true,
            message: '请输入验证码',
            trigger: 'blur'
          },
        ],
      },
      identifyCodes: "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ", //验证码的数字库
      identifyCode: "",
    }
  },
  methods: {
    //验证码----start
    randomNum(min, max) {
      return Math.floor(Math.random() * (max - min) + min);
    },
    refreshCode() {
      this.identifyCode = "";
      this.makeCode(this.identifyCodes, 4);
    },
    makeCode(o, l) {
      for (let i = 0; i < l; i++) {
        this.identifyCode += this.identifyCodes[
            this.randomNum(0, this.identifyCodes.length)
            ];
      }
      // console.log("this.identifyCode:", this.identifyCode);
    },
    //验证码----end
    login(form) {
      this.$refs[form].validate((valid) => {
        if (valid) {
          if (this.identifyCode == this.form.verified) {
            alert('登录成功!');
          } else {
            alert("验证码错误")
          }
        } else {
          return false;
        }
      });
    }
  },

  created() {
    this.refreshCode(); //初始化验证码
  },

  mounted() {
    // 验证码初始化
    this.identifyCode = '';
    this.makeCode(this.identifyCodes, 4);

  },
}

</script>

<style scoped>
.RandomSure {
  margin: 0;
  padding: 0;
  width: 100vw;
  min-height: 100vh;
  background-color: #995d19;
  display: flex;
  justify-content: center;
  position: relative;
}

.identify {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.loginPwd {
  text-align: center;
  font-size: 20px;
  font-weight: bold;
  color: cornflowerblue;
}

.centerLogin {
  width: 500px;
  height: 350px;
  position: absolute;
  top: 13%;
  background-color: white;
  border-radius: 15px;
}

.login {
  display: flex;
  justify-content: center;
}

/deep/ .el-form-item__content {
  display: flex;
  width: 400px;
  justify-content: center;
  align-items: center;
}
</style>