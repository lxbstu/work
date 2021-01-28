<template>
  <div id="chat">
    <div class="chat-list">
      <div v-for="(item, index) in chatList" :key="index">
        <div class="q">
          <span>{{ item.question }}</span>
        </div>
        <div class="a">
          <span>{{ item.answer }}</span>
        </div>
      </div>
    </div>
    <div class="chat-form">
      <el-form :inline="true" class="demo-form-inline">
        <el-form-item>
          <el-input v-model="chatText" placeholder="请输入信息"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="sendMessage">发送</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
import md5 from "blueimp-md5";
export default {
  created() {
    this.sendMessage;
  },
  data() {
    return {
      chatText: "",
      chatList: [],
    };
  },
  methods: {
    getTimeStamp() {
      let timer = new Date();
      timer = Date.parse(timer);
      timer = timer.toString().substr(0, 10);
      timer = Number.parseInt(timer);
      return timer;
    },
    getNonceStr() {
      let str = Math.random();
      str = str.toString(16);
      str = str.substr(2);
      return str;
    },
    getSign(obj) {
      let arr = Object.keys(obj);
      arr = arr.sort();
      let str = "";
      arr.forEach((key) => {
        str += key + "=" + obj[key] + "&";
      });
      str = encodeURI(str);
      str += "app_key=8CPq9nvKc2ZDSwjL";
      str = md5(str).toUpperCase();
      return str;
    },
    sendMessage() {
      let baseUrl = "https://bird.ioliu.cn/v2?url=";
      let requestUrl = "https://api.ai.qq.com/fcgi-bin/nlp/nlp_textchat";
      let params = {
        app_id: 2155983479,
        time_stamp: this.getTimeStamp(),
        nonce_str: this.getNonceStr(),
        session: "10000",
        question: this.chatText,
      };
      params.sign = this.getSign(params);
      console.log(params);
      this.axios
        .get(baseUrl + requestUrl, { params })
        .then((res) => {
          console.log(res);
          this.chatList.push({
            question: this.chatText,
            answer: res.data.data.answer,
          });
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
};
</script>

<style lang="scss" scoped>
.chat-list {
  font-size: 0.5rem;
  .q {
    text-align: right;
    span {
      background: green;
    }
  }
  .a {
    text-align: left;
    span {
      background: red;
    }
  }
}
.chat-form {
  position: fixed;
  bottom: 1rem;
  width: 100%;
  text-align: center;
  background: grey;
}
</style>
