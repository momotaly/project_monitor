<template>
    <div>
        <p v-for="message in messages">{{ message }}</p>
        <input v-model="text">
        <button @click="postMessage" :disabled="!contentExists">submit</button>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                text: '',
                messages: []
            }
        },
        computed: {
            contentExists() {
                return this.text.length > 0;
            }
        },
        methods: {
             getMessages() {
                axios.get('/getAll').then((response) => {
                    this.messages = response.data;
                });
            },
            postMessage() {
                axios.post('/post', {message: this.text}).then(({data}) => {
                    this.messages.push(data);
                    this.text = '';
                });
            },
            , 
            listen() {
                Echo.channel('chat-box').listen('.MessageSent', (message) => 
                    {
                    this.messages.push(message);
                     console.log(message);
                });
            }
        },
        created() {
            axios.get('/getAll').then(({data}) => {
                this.messages = data;
            });
        }
    }
    

</script>