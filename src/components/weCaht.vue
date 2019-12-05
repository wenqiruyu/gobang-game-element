<template>
    <div>
        <div>
            <el-input v-model="input" placeholder="请输入内容"></el-input>
            <el-button @click="submitForm('ruleForm')">默认按钮</el-button>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'weChat',
        data(){
            return {
                input: '',
            }
        },
        created() {
            this.initWebSocket()
        },
        methods: {
            initWebSocket(){
                const $vm = this;
                let ws = new WebSocket('ws://localhost:10086/websocket');
                ws.onopen = () => {
                    // Web Socket 已连接上，使用 send() 方法发送数据
                    console.log('发送数据中。。。')
                    ws.send('你好')
                    console.log('数据发送完成。')
                }
                ws.onmessage = evt => {    //后端发来的消息接收（注意数据放在evt的data中并且是个JSON字符串，需要parse成JSON对象去使用）
                    let hhlist = JSON.parse(evt.data);
                    //具体业务逻辑
		            $vm.$set($vm.panelData,"rsumhh",psu) 
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

<style lang="sass" scoped>

</style>