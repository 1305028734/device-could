<template>
  <div class="login">
    <div class="loginBox" v-show="isshow">
      <h1>长虹设备云</h1>
      <p>
        <el-input placeholder="用户名/手机号" v-model="userName"></el-input>
      </p>
      <p>
        <el-input type="password" placeholder="密码" v-model="password"></el-input>
      </p>
      <p class="proving">
        <el-input type="text" placeholder="验证码"></el-input>
        <span class="provingImg">图片</span>
        <el-button>点击刷新</el-button>
      </p>
      <p>
        <el-button type="primary" round @click="login()">登录</el-button>
      </p>
      <p class="registerSkip">
        <span>忘记密码</span>
        <span v-on:click="function(){ 
          isshow=!isshow 
          ishide=!ishide
          }">企业注册</span>
        <span v-on:click="function(){ 
          adminshow=!adminshow 
          isshow=!isshow
          }">
          管理员登录
        </span>
      </p>
    </div>
    <div class="adminLogin" v-show="adminshow">
      <h1>长虹设备云</h1>
      <p>
        <el-input placeholder="管理员账号" v-model="userName"></el-input>
      </p>
      <p>
        <el-input type="password" placeholder="管理员密码" v-model="password"></el-input>
      </p>
      <p class="proving">
        <el-input type="text" placeholder="操作员手机号"></el-input>
        <el-input type="text" placeholder="短信验证码"></el-input>
        <el-button type="primary" plain>手机验证</el-button>
      </p>
      <p>
        <el-button type="primary" round @click="login()">登录</el-button>
      </p>
      <p class="registerSkip">
        <span>忘记密码</span>
        <span v-on:click="function(){ 
          isshow=!isshow 
          ishide=!ishide
          }">企业注册</span>
        <span v-on:click="function(){ 
          adminshow=!adminshow 
          isshow=!isshow
          }">
          员工登录
        </span>
      </p>
    </div>
    <div class="register" v-show="ishide">
      <div v-show="backshow">
        <h2>企业注册</h2>
        <div class="titleText">（企业注册信息填写）</div>
        <ul>
          <li>
            <label for="">企业名称：</label>
            <el-input size="small" v-model="company.name"></el-input>
          </li>
          <li>
            <label for="">企业地址：</label>
            <el-input size="small" v-model="company.address"></el-input>
          </li>
          <li>
            <label for="">联系电话：</label>
            <el-input size="small" v-model="company.phone"></el-input>
          </li>
          <li>
            <label for="">法人代表：</label>
            <el-input size="small" v-model="company.corporation"></el-input>
          </li>
          <li>
            <label for="">营业执照注册号：</label>
            <el-input size="small" v-model="company.companyID"></el-input>
          </li>
          <li>
            <label for="">营业执照：</label>
            <el-upload class="upload-demo" action="https://jsonplaceholder.typicode.com/posts/" :on-preview="handlePreview" :on-remove="handleRemove" :file-list="fileList2" list-type="picture">
              <el-button size="small" type="primary">点击上传</el-button>
              <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过500kb</div>
            </el-upload>
          </li>
        </ul>
        <div class="next">
          <el-button type="primary" size="small" round class="registerBtn" v-on:click="function(){nextshow=!nextshow
           backshow=!backshow}">下一步</el-button>
        </div>
      </div>
      <div v-show="nextshow" style="margin-top:20px">
        <div class="titleText">（管理员信息登记）</div>
        <ul>
          <li>
            <label for="">用户名：</label>
            <el-input size="small" v-model="manager.userName"></el-input>
          </li>
          <li>
            <label for="">手机号：</label>
            <el-input size="small" v-model="manager.phone"></el-input>
          </li>
          <li>
            <label for="">验证码：</label>
            <el-input class="validate" size="small" v-model="manager.validate"></el-input>
            <el-button type="primary" round size="small">获取验证码</el-button>
          </li>
          <li>
            <el-checkbox v-model="checked">您已阅读<a href="">《长虹设备云用户注册协议》</a></el-checkbox>
          </li>
          <li>
            <el-button type="primary" size="small" round class="registerBtn" v-on:click="function(){nextshow=!nextshow
           backshow=!backshow}">上一步</el-button>
            <el-button type="primary" size="small" round class="registerBtn">注册</el-button>
          </li>
        </ul>
      </div>

      <div class="loginSkip" v-on:click="function(){
          isshow=!isshow 
          ishide=!ishide
          }">已有账号，直接登录</div>
    </div>
  </div>
</template>
<script>
import md5 from "js-md5/src/md5.js";
export default {
  name: "Login",
  data() {
    return {
      fileList2: [
        {
          name: "food.jpeg",
          url:
            "https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100"
        },
        {
          name: "food2.jpeg",
          url:
            "https://fuss10.elemecdn.com/3/63/4e7f3a15429bfda99bce42a18cdd1jpeg.jpeg?imageMogr2/thumbnail/360x360/format/webp/quality/100"
        }
      ],

      input: "",
      userName: "",
      password: "",
      isshow: true,
      ishide: false,
      adminshow: false,
      nextshow: false,
      backshow: true,
      fileList: [
        {
          name: "",
          url: ""
        }
      ],
      company: {
        name: "",
        address: "",
        phone: "",
        corporation: "",
        companyID: ""
      },
      manager: {
        userName: "",
        userPassword: "",
        password: "",
        phone: "",
        validate: ""
      },
      checked: true
    };
  },
  methods: {
    login() {
      this.password = md5(this.password);
      console.log(md5(this.password));
      let qs = require("qs");
      // let instance = axios.create({
      //   headers: { "content-type": "application/x-www-form-urlencoded" }
      // });
      let data = qs.stringify({
        phone: this.userName,
        passWord: this.password
      });
      axios
        .post("/api/user/login", data)
        .then(result => {
          console.log(result.data);
        })
        .catch(err => {
          console.log(err);
          console.log(this.userName);
        });
    },
    handleRemove(file, fileList) {
      console.log(file, fileList);
    },
    handlePreview(file) {
      console.log(file);
    },
    handleExceed(files, fileList) {
      this.$message.warning(
        `当前限制选择 3 个文件，本次选择了 ${
          files.length
        } 个文件，共选择了 ${files.length + fileList.length} 个文件`
      );
    },
    beforeRemove(file, fileList) {
      return this.$confirm(`确定移除 ${file.name}？`);
    }
  },
  components: {},
  created() {
    axios
      .get("/api/user/all")
      .then(response => {
        console.log(response.data.data);
      })
      .catch(function(error) {
        console.log(error);
      });
  }
};
</script>
<style lang="less" scoped>
@blue: #409eff;
@Success: #67c23a;
@Warning: #e6a23c;
@Danger: #f56c6c;
@Info: #909399;
* {
  margin: 0;
  padding: 0;
}
.login {
  text-align: center;
  width: 100%;
  height: 100vh;
  background-color: @blue;
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  top: 0;
  left: 0;

  .loginBox {
    width: 40%;
    height: auto;
    background-color: white;
    border-radius: 10px;
    box-shadow: 10px 10px 10px @Info;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
    align-items: center;
    justify-content: center;
    h1 {
      width: 100%;
      padding: 20px 0;
      letter-spacing: 2px;
      font-family: "和畅惠风 Hiragino Sans GB";
      font-weight: 500;
      color: @blue;
    }
    p {
      width: 60%;
      padding: 10px 0;
      button {
        width: 100%;
      }
    }
    .registerSkip {
      text-align: right;
      padding-bottom: 50px;
      font-size: 12px;
      span {
        display: inline-block;
        margin-left: 5px;
        cursor: pointer;
        &:hover {
          color: @blue;
        }
      }
    }
    .proving {
      text-align: left;
      .el-input {
        width: 40%;
      }
      .el-button {
        width: 20% !important;
        height: 30px;
      }
      span {
        margin-left: 3%;
        display: inline-block;
        width: 30%;
        border: 1px solid red;
      }
    }
  }
}
.adminLogin {
  width: 40%;
  height: auto;
  background-color: white;
  border-radius: 10px;
  box-shadow: 10px 10px 10px @Info;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
  h1 {
    width: 100%;
    padding: 20px 0;
    letter-spacing: 2px;
    font-family: "和畅惠风 Hiragino Sans GB";
    font-weight: 500;
    color: @blue;
  }
  p {
    width: 60%;
    padding: 10px 0;
    button {
      width: 100%;
    }
  }
  .registerSkip {
    text-align: right;
    padding-bottom: 50px;
    font-size: 12px;
    span {
      display: inline-block;
      margin-left: 5px;
      cursor: pointer;
      &:hover {
        color: @blue;
      }
    }
  }
  .proving {
    font-size: 12px;
    text-align: left;
    position: relative;
    .el-input:nth-child(1) {
      width: 40%;
    }
    .el-input:nth-child(2) {
      width: 58.5%;
    }
    .el-button {
      position: absolute;
      top: 20%;
      right: 1.5%;
      width: 70px;
      height: 36px;
      margin-left: 4%;
      border: none;
    }
  }
}
.register {
  position: absolute;
  right: 10%;
  width: 35%;
  background-color: white;
  box-shadow: 10px 10px 10px @Info;
  border-radius: 10px;
  padding-bottom: 30px;
  .next {
    margin-top: 30px;
  }
  h2 {
    letter-spacing: 2px;
    font-weight: 500;
    padding: 10px 0;
  }
  ul {
    li {
      list-style-type: none;
      margin-bottom: 5px;
      label {
        display: inline-block;
        width: 30%;
        text-align: right;
      }
      .validate {
        width: 30%;
      }
      a {
        text-decoration: none;
        color: @blue;
      }
    }
  }
  .el-input {
    width: 50%;
    padding: 0;
  }
  .titleText {
    width: 100%;
    font-size: 12px;
    color: #909399;
    text-align: center;
    padding: 10px 0 10px 20px;
  }
  .upload-demo {
    width: 50%;
    display: inline-block;
    padding: 0;
  }
  .registerBtn {
    width: 30%;
  }
  .loginSkip {
    padding-right: 40px;
    margin-top: 20px;
    text-align: right;
    font-size: 12px;
    cursor: pointer;
  }
}
</style>
