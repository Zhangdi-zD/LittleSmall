<template>
  <div class="fullScreen">
    <div class="CenterBox">
      <div class="CenterHeader">
        <p>随机密码生成器</p>
      </div>
      <div class="centerTop">
        <span>密码长度:</span>
        <el-input-number v-model="input" controls-position="right" :min="1"
                         :max="20" size="small"></el-input-number>
      </div>
      <div class="centerTop">
        <span>密&nbsp;&nbsp;&nbsp;&nbsp;码:</span>
        <el-input v-model="inputValue" placeholder="密码" size="small" style="width: 130px"></el-input>
      </div>
      <div class="feetBottom">
        <el-button type="primary" @click="doIt">生成密码</el-button>
        <el-button type="success" @click="copyIt" :disabled="canCopy">复制密码</el-button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      canCopy: true,
      none: '啥也不是',
      input: 8,
      chars: "0123456789abcdefghijklmnopqrstuvwxyz!@#$%^&*()ABCDEFGHIJKLMNOPQRSTUVWXYZ",
      inputValue: ''
    }
  },
  methods: {
    copyIt() {
      const oInput = document.createElement("input");
      oInput.value = this.inputValue;
      document.body.appendChild(oInput);
      oInput.select(); // 选择对象
      document.execCommand("Copy"); // 执行浏览器复制命令
      oInput.className = "oInput";
      oInput.style.display = "none";
      this.$message("复制成功");
    },
    doIt: function () {
      this.inputValue = ''
      for (let i = 0; i <= this.input - 1; i++) {
        const randomNumber = Math.floor(Math.random() * this.chars.length);
        this.inputValue += this.chars.substring(randomNumber, randomNumber + 1);
      }

      this.canCopy = this.inputValue == '';
    }
  }
}
</script>
<style scoped>
.fullScreen {
  margin: 0;
  padding: 0;
  width: 100vw;
  min-height: 100vh;
  background-color: #42b983;
  display: flex;
  justify-content: center;
  position: relative;
}

.CenterBox {
  position: absolute;
  top: 20%;
  width: 480px;
  height: 400px;
  background-color: white;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
}

.CenterHeader p {
  text-align: center;
  font-size: 20px;
  font-weight: bold;
  color: cornflowerblue;
}

.centerTop {
  display: flex;
  justify-content: space-evenly;
}

.centerTop span {
  font-size: 18px;
  font-weight: bold;
}

.feetBottom {
  display: flex;
  justify-content: space-around;
}
</style>