<template>
  <div id="app">
    <div v-if="savedCode">
      <h1>歡迎回來, {{ savedCode }}!</h1>
      <button @click="clearCode">清除用戶代碼</button>
    </div>
    <div v-else>
      <h1>請輸入您的用戶代碼</h1>
      <input v-model="userCode" placeholder="輸入代碼">
      <button @click="saveCode">儲存</button>
    </div>
    <div v-if="browser && os">
      <h2>您的設備資訊：</h2>
      <p>瀏覽器：{{ browser.name }} {{ browser.version }}</p>
      <p>作業系統：{{ os.name }} {{ os.version }}</p>
    </div>
  </div>
</template>

<script>
import { UAParser } from "ua-parser-js";

export default {
  name: "App",
  data() {
    return {
      userCode: "",
      savedCode: null,
      browser: null,
      os: null
    };
  },
  mounted() {
    this.savedCode = localStorage.getItem("userCode");
    const parser = new UAParser();
    const result = parser.getResult();
    this.browser = result.browser;
    this.os = result.os;
  },
  methods: {
    saveCode() {
      localStorage.setItem("userCode", this.userCode);
      this.savedCode = this.userCode;
      this.userCode = "";
    },
    clearCode() {
      localStorage.removeItem("userCode");
      this.savedCode = null;
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>