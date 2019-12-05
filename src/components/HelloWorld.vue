<template>
  <div class="hello">
    <div>
      <el-input v-model="input" placeholder="请输入内容"></el-input>
      <el-button @click="submitForm('ruleForm')">默认按钮</el-button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      input: '',
      msg: 'Welcome to Your Vue.js App'
    }
  },
  created() {
      //页面刚进入时开启长连接
      this.initWebSocket()
  },
  methods: {
            submitForm(formName) {
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        const params = new URLSearchParams()
                        params.append('username', this.ruleForm.username)
                        params.append('password', this.ruleForm.password)
                        this.$axios.post('/venus-admin-server/user/login',params).then((res) => {
                            console.log(res.data)
                            console.log(res.data.code)
                            if(res.data.status == 1){
                                localStorage.setItem('ms_username',this.ruleForm.username);
                                this.$router.push('/index')
                            }else{
                                this.msg = res.data.msg
                            }
                        })
                    } else {
                        console.log('error submit!!');
                        return false;
                    }
                });
            },
        initWebSocket(){ //初始化weosocket
        const $vm = this
        let ws = new WebSocket('ws://localhost:10086/websocket');  //这里new 一个WebSocket对象将连接地址传入（这个连接地址很重要，很多情况连接不上就是连接地址没用写对）
         ws.onopen = () => {
          // Web Socket 已连接上，使用 send() 方法发送数据
          console.log('发送数据中。。。')
          ws.send('你好')
          console.log('数据发送完成。')
        }
        ws.onmessage = evt => {    //后端发来的消息接收（注意数据放在evt的data中并且是个JSON字符串，需要parse成JSON对象去使用）
          let hhlist = JSON.parse(evt.data);
      //具体业务逻辑
		 $vm.$set($vm.panelData,"rsumhh",psu) //这里是 vue给一些已经改变了的图表数据后，显示内容没用变化时用的。（直接用=去赋值界面的显示是不会变的 Echarts 图表就是需要用 set去改变值的变化）
          console.log('数据已接收...')

        }
        ws.onclose = function () {
          // 关闭 websocket
          console.log('连接已关闭...')
        }
        // 路由跳转时结束websocket链接
        this.$router.afterEach(function () {
          ws.close()
        })
    }

        }
}
</script>

<style scoped>

</style>
