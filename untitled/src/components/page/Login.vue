<template>
  <div class="login-wrap">
    <div class="ms-login">
      <div class="ms-title">项目管理系统</div>
      <el-form ref="login" :model="param" :rules="rules" class="ms-content" label-width="0px">
        <el-form-item prop="username">
          <el-input v-model="param.username" placeholder="username"
                    @keyup.enter.native="submitForm()">
            <el-button slot="prepend" icon="el-icon-lx-people"></el-button>
          </el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input
              v-model="param.password"
              placeholder="password"
              type="password"
              @keyup.enter.native="submitForm()"
          >
            <el-button slot="prepend" icon="el-icon-lx-lock"></el-button>
          </el-input>
        </el-form-item>
        <div class="login-btn">
          <el-button type="primary" @click="submitForm()">登录</el-button>
        </div>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      param: {
        username: this.$cookie.get('UserID'),
        password: this.$cookie.get('UserPassword')
      },
      rules: {
        username: [{ required: true, message: '请输入用户名', trigger: 'blur' }],
        password: [{ required: true, message: '请输入密码', trigger: 'blur' }]
      }
    };
  },
  methods: {
    submitForm() {
      this.$refs.login.validate(valid => {
        if (valid) {
          this.$cookie.set('UserID', this.param.username);
          this.$cookie.set('UserPassword', this.param.password);
          let formData = new FormData();
          formData.append('UserID', this.$cookie.get('UserID'));
          formData.append('UserPassword', this.$cookie.get('UserPassword'));
          this.$axios({
            url: '/login',
            method: 'POST',
            data: formData
          }).then(response => {
            if (response.data['finish'] === 'wrong password') {
              this.$message.error(response.data['finish']);
            } else {
              this.$message.success(response.data['finish']);
              this.$cookie.set('LoginType', response.data['loginType']);
              this.$cookie.set('TeamID', response.data['teamID']);
              this.$cookie.set('UserName', response.data['userName']);
              this.$cookie.set('TeamName', response.data['teamName']);
              this.$cookie.set('RoleName', response.data['roleName']);
              this.$cookie.set('TeamName', response.data['teamName']);
              this.$router.push('/');
            }
          }).catch(err => {
            console.log(err);
          });
        } else {
          this.$message.error('请输入账号和密码！！');
          return false;
        }
      });
    }
  }
};
</script>

<style scoped>
.login-wrap {
  position: relative;
  width: 100%;
  height: 100%;
  background-image: url(../../assets/img/login-bg.jpg);
  background-size: 100%;
}

.ms-title {
  width: 100%;
  line-height: 50px;
  text-align: center;
  font-size: 20px;
  color: navy;
  border-bottom: 1px solid #ddd;
}

.ms-login {
  position: absolute;
  left: 50%;
  top: 50%;
  width: 350px;
  margin: -190px 0 0 -175px;
  border-radius: 5px;
  background: rgba(255, 255, 255, 0.3);
  overflow: hidden;
}

.ms-content {
  padding: 30px 30px;
}

.login-btn {
  text-align: center;
}

.login-btn button {
  width: 100%;
  height: 36px;
  margin-bottom: 10px;
}

.login-tips {
  font-size: 12px;
  line-height: 30px;
  color: #fff;
}
</style>