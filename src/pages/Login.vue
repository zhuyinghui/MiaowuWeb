<template>
  <div class="cotn_principal">
    <div class="cont_centrar">
      <div class="cont_login">
        <div class="cont_info_log_sign_up">
          <div class="col_md_login">
            <div class="cont_ba_opcitiy">
              <p>已有喵呜网账号？直接登录进入吧！</p>
              <button class="btn_login" @click="cambiar_login">登陆</button>
            </div>
          </div>
          <div class="col_md_sign_up">
            <div class="cont_ba_opcitiy">
              <p>还没有喵呜网账号？快来注册吧！</p>
              <button class="btn_sign_up" @click="cambiar_sign_up">注册</button>
            </div>
          </div>
        </div>
        <div class="cont_back_info">
          <div class="cont_img_back_grey"> <img src="../../static/assets/images/cat3.jpg" alt="" /> </div>
        </div>
        <div class="cont_forms" >
          <div class="cont_img_back_"> <img src="../../static/assets/images/cat3.jpg" alt="" /> </div>
          <div class="cont_form_login">
            <a @click="ocultar_login_sign_up"><i class="el-icon-caret-left"></i>返回</a>
            <h2>登录</h2>
              <input type="text" placeholder="用户名/邮箱" v-model="login.username"/>
              <input type="password" placeholder="密码" v-model="login.password"/>
              <p class="msg" v-show="msg.length">{{msg}}</p>
              <button class="btn_login" @click="submitlogin">确认</button>
          </div>
          <div class="cont_form_sign_up"><a @click="ocultar_login_sign_up"><i class="el-icon-caret-left"></i>返回</a>
            <h2>注册</h2>
            <input type="text" placeholder="用户名" v-model="register.username"/>
            <input type="password" placeholder="密码" v-model="register.password"/>
            <input type="password" placeholder="再次确认密码" v-model="register.password2"/>
            <p class="msg" v-show="msg2.length">{{msg2}}</p>
            <button class="btn_sign_up" @click="submitregister">确认</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
    export default {
        name: "Login",
      data(){
          return{
            login:{
              username:'',
              password:''
            },
            register:{
              username:'',
              password:'',
              password2:''
            },
            msg:'' ,
            logindata:'',
            msg2:''
          }
      },
      methods:{
        cambiar_login() {
          document.querySelector('.cont_forms').className = "cont_forms cont_forms_active_login";
          document.querySelector('.cont_form_login').style.display = "block";
          document.querySelector('.cont_form_sign_up').style.opacity = "0";
          setTimeout(function(){  document.querySelector('.cont_form_login').style.opacity = "1"; },400);
          setTimeout(function(){
            document.querySelector('.cont_form_sign_up').style.display = "none";
          },200);
        },
        submitlogin(){
          this.axios.post(this.$domain+'/api/userInfo/login',{
            username:this.login.username,
            password:this.login.password
          },).then(res=>{
            if(res.data.status===0){
              this.msg='用户名或密码不正确'
            }else{
              this.msg='登录成功';
             localStorage.setItem('name',this.login.username);
              localStorage.setItem('userid',res.data.userid);
              this.$router.push('/')
            }
          }).catch(err=>{
            console.log(err)
          });
        },
        submitregister(){
          if(!this.register.username||!this.register.password||!this.register.password2){
            this.msg2='请输入用户名或者密码';
          } else if(this.register.password!==this.register.password2){
            this.msg2='两次输入的密码不一致';
          }else{
            this.axios.post(this.$domain+'/api/userInfo/register',{
              username:this.register.username,
              password:this.register.password
            },).then(res=>{
              if(res.data.status===0){
                this.msg2='用户名已存在'
              }else{
                this.msg2='注册成功'
                this.cambiar_login()
              }
            }).catch(err=>{
              console.log(err)
            });
          }
        },
        cambiar_sign_up() {
          document.querySelector('.cont_forms').className = "cont_forms cont_forms_active_sign_up";
          document.querySelector('.cont_form_sign_up').style.display = "block";
          document.querySelector('.cont_form_login').style.opacity = "0";
          setTimeout(function(){  document.querySelector('.cont_form_sign_up').style.opacity = "1";
          },100);
          setTimeout(function(){   document.querySelector('.cont_form_login').style.display = "none";
          },400);
        },
        ocultar_login_sign_up() {
          history.back()
        }
      },
      mounted(){
        this.cambiar_login();
      },
    }
</script>
<style scoped>
  * {
    margin: 0 auto;
    padding: 0;
    text-align: center;
    font-family: 'Lato', sans-serif;
  }
  .msg{
    color: red;font-size: 14px;
  }
  .cont_centrar {
    width: 100%;height: 550px;
    background: url("../../static/assets/images/cat3.jpg");
    background-size: cover;
  }
  .cont_login {
    width: 640px;
    position: fixed;
    top: 50px;left: 20%;
  }

  .cont_back_info {
    position: relative;
    float: left;
    width: 640px;
    height: 280px;
    overflow: hidden;
    background-color: #fff;
    margin-top: 100px;
    box-shadow: 1px 10px 30px -10px rgba(0,0,0,0.5);
  }

  .cont_forms {
    position: absolute;
    overflow: hidden;
    top:100px;
    left: 0px;
    width: 320px;
    height: 280px;
    background-color: #eee;
    -webkit-transition: all 0.5s;
    -moz-transition: all 0.5s;
    -ms-transition: all 0.5s;
    -o-transition: all 0.5s;
    transition: all 0.5s;
  }

  .cont_forms_active_login {
    box-shadow: 1px 10px 30px -10px rgba(0,0,0,0.5);
    height: 420px;
    top:20px;
    left: 0px;
    -webkit-transition: all 0.5s;
    -moz-transition: all 0.5s;
    -ms-transition: all 0.5s;
    -o-transition: all 0.5s;
    transition: all 0.5s;

  }

  .cont_forms_active_sign_up {
    box-shadow: 1px 10px 30px -10px rgba(0,0,0,0.5);
    height: 420px;
    top:20px;
    left:320px;
    -webkit-transition: all 0.5s;
    -moz-transition: all 0.5s;
    -ms-transition: all 0.5s;
    -o-transition: all 0.5s;
    transition: all 0.5s;
  }

  .cont_img_back_grey {
    position: absolute;
    width: 950px;
    top:-80px;
    left: -116px;
  }

  .cont_img_back_grey > img {
    width: 100%;
    -webkit-filter: grayscale(100%);     filter: grayscale(100%);
    opacity: 0.5;
    animation-name: animar_fondo;
    animation-duration: 20s;
    animation-timing-function: linear;
    animation-iteration-count: infinite;
    animation-direction: alternate;

  }

  .cont_img_back_ {
    position: absolute;
    width: 950px;
    top:-80px;
    left: -116px;
  }

  .cont_img_back_ > img {
    width: 100%;
    opacity: 0.3;
    animation-name: animar_fondo;
    animation-duration: 20s;
    animation-timing-function: linear;
    animation-iteration-count: infinite;
    animation-direction: alternate;
  }

  .cont_forms_active_login > .cont_img_back_ {
    top:0px;
    -webkit-transition: all 0.5s;
    -moz-transition: all 0.5s;
    -ms-transition: all 0.5s;
    -o-transition: all 0.5s;
    transition: all 0.5s;
  }

  .cont_forms_active_sign_up > .cont_img_back_ {
    top:0px;
    left: -435px;
    -webkit-transition: all 0.5s;
    -moz-transition: all 0.5s;
    -ms-transition: all 0.5s;
    -o-transition: all 0.5s;
    transition: all 0.5s;
  }


  .cont_info_log_sign_up {
    position: absolute;
    width: 640px;
    height: 280px;
    top: 100px;
    z-index: 1;
  }

  .col_md_login {
    position: relative;
    float: left;
    width: 50%;
  }

  .col_md_login > h2 {
    font-weight: 400;
    margin-top: 70px;
    color: #757575;
  }

  .col_md_login > p {
    font-weight: 400;
    margin-top: 15px;
    width: 80%;
    color: #37474F;
  }

  .btn_login {
    background-color: #FF9800;
    border: none;
    padding: 10px;
    width: 200px;
    border-radius:3px;
    box-shadow: 1px 5px 20px -5px rgba(0,0,0,0.4);
    color: #fff;
    margin-top: 10px;
    cursor: pointer;
  }

  .col_md_sign_up {
    position: relative;
    float: left;
    width: 50%;
  }

  .cont_ba_opcitiy > h2 {
    font-weight: 400;
    color: #fff;
  }

  .cont_ba_opcitiy > p {
    font-weight: 400;
    margin-top: 15px;
    color: #fff;
  }
  /* ----------------------------------
  background text
  ------------------------------------
   */
  .cont_ba_opcitiy {
    position: relative;
    background-color: rgba(187, 168, 170, 0.79);
    width: 80%;
    border-radius:3px ;
    margin-top: 60px;
    padding: 15px 0px;
  }

  .btn_sign_up {
    background-color: #f44336;
    border: none;
    padding: 10px;
    width: 200px;
    border-radius:3px;
    box-shadow: 1px 5px 20px -5px rgba(0,0,0,0.4);
    color: #fff;
    margin-top: 10px;
    cursor: pointer;
  }
  .cont_forms_active_sign_up {
    z-index: 2;
  }

  @-webkit-keyframes animar_fondo {
    from { -webkit-transform: scale(1) translate(0px);
      -moz-transform: scale(1) translate(0px);
      -ms-transform: scale(1) translate(0px);
      -o-transform: scale(1) translate(0px);
      transform: scale(1) translate(0px); }
    to { -webkit-transform: scale(1.5) translate(50px);
      -moz-transform: scale(1.5) translate(50px);
      -ms-transform: scale(1.5) translate(50px);
      -o-transform: scale(1.5) translate(50px);
      transform: scale(1.5) translate(50px); }
  }
  @-o-keyframes identifier {
    from { -webkit-transform: scale(1);
      -moz-transform: scale(1);
      -ms-transform: scale(1);
      -o-transform: scale(1);
      transform: scale(1); }
    to { -webkit-transform: scale(1.5);
      -moz-transform: scale(1.5);
      -ms-transform: scale(1.5);
      -o-transform: scale(1.5);
      transform: scale(1.5); }

  }
  @-moz-keyframes identifier {
    from { -webkit-transform: scale(1);
      -moz-transform: scale(1);
      -ms-transform: scale(1);
      -o-transform: scale(1);
      transform: scale(1); }
    to { -webkit-transform: scale(1.5);
      -moz-transform: scale(1.5);
      -ms-transform: scale(1.5);
      -o-transform: scale(1.5);
      transform: scale(1.5); }

  }
  @keyframes identifier {
    from { -webkit-transform: scale(1);
      -moz-transform: scale(1);
      -ms-transform: scale(1);
      -o-transform: scale(1);
      transform: scale(1); }
    to { -webkit-transform: scale(1.5);
      -moz-transform: scale(1.5);
      -ms-transform: scale(1.5);
      -o-transform: scale(1.5);
      transform: scale(1.5); }
  }
  .cont_form_login {
    position: absolute;
    opacity: 0;
    display: none;
    width: 320px;
    -webkit-transition: all 0.5s;
    -moz-transition: all 0.5s;
    -ms-transition: all 0.5s;
    -o-transition: all 0.5s;
    transition: all 0.5s;
  }

  .cont_forms_active_login {
    z-index: 2;
  }
  .cont_forms_active_login  >.cont_form_login {
  }

  .cont_form_sign_up {
    position: absolute;
    width: 320px;
    float: left;
    opacity: 0;
    display: none;
    -webkit-transition: all 0.5s;
    -moz-transition: all 0.5s;
    -ms-transition: all 0.5s;
    -o-transition: all 0.5s;
    transition: all 0.5s;
  }


  .cont_form_sign_up > input {
    text-align: left;
    padding: 15px 5px;
    margin-left: 10px;
    margin-top: 20px;
    width: 260px;
    border: none;
    color: #757575;
  }

  .cont_form_sign_up > h2 {
    margin-top: 50px;
    font-weight: 400;
    color: #757575;
  }


  .cont_form_login > input {
    padding: 15px 5px;
    margin-left: 10px;
    margin-top: 20px;
    width: 260px;
    border: none;
    text-align: left;
    color: #757575;
  }

  .cont_form_login > h2 {
    margin-top: 110px;
    font-weight: 400;
    color: #757575;
  }
  .cont_form_login > a,.cont_form_sign_up > a  {
    color: #757575;
    position: relative;
    float: left;
    margin: 10px;
    margin-left: 30px;
    cursor: pointer;
  }
</style>
