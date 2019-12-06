<template>
    <div>
        <div>
            <div>
                <el-input v-model="chatMessage" placeholder="来打个招呼吧">
                </el-input>
            </div>
            <div>
                <el-button @click="sendMessage('chatMessage')">发送</el-button>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'WeChat',
        data(){
            return {
                chatMessage: '',
                webSocket: null
            }
        },
        created() {
            this.initWebSocket()
        },
        methods: {
            initWebSocket(){
                const $vm = this;
                this.webSocket = new WebSocket('ws://localhost:10086/websocket')
                this.webSocket.onopen = function() {
                    console.log('WebSocket已连接')
                }

                this.webSocket.onmessage = evt => {
                    console.log(evt.data)                    
                    console.log('数据已接收...')
                }

                this.webSocket.onclose = function () {
                    console.log('WebSocket连接已关闭...')
                }

                this.webSocket.onclose = function () {
                    console.log('连接已关闭...')
                }

                window.onbeforeunload = function() {
                    this.websocket.close();
                }
            },
            sendMessage(){
                if (this.chatMessage != '') {
                    this.webSocket.send(this.chatMessage)
                }
            }
        }
    }
</script>

<style scoped>

</style>