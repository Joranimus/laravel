<template>
    <div class="gifs">
        <div  class="button" @click="getGifs">get data</div>
        <div  class="_f _f-wrap _j-around">
            <gif
            v-for="item in gifs.data" :key="item.bitly_url"
            v-bind:url="item.images.downsized_medium.url"
            />
        </div>
    </div>
</template>

<script>
import gif from './gif.vue'
const axios = require('axios');

export default {
    name: 'gifp',
    components: {
        gif
    },
    data(){
        return{
            gifs:{},
            time:''
        }
    },
    mounted() {
        if (localStorage.getItem('gifs')) {
        this.gifs = JSON.parse(localStorage.getItem('gifs'));
        };
        if(localStorage.getItem('giftime')){this.time = localStorage.getItem('giftime')};
        if((Date.now() - this.time) >= (1000*60*60)){this.getGifs};
    },
    watch: {
        gifs(data) {
            const parsed = JSON.stringify(data);
            localStorage.setItem('gifs', parsed);
        },
        time(data){
            localStorage.setItem('giftime', data);
        }
    },
    methods:{
        getGifs(){
            axios.get('https://api.giphy.com/v1/gifs/trending?api_key=nbbaXJY4oa7KE8bMbqIm8b8xXSNpKvm7&tag=&rating=g')
                .then(response => {
                this.gifs = response.data;
                this.time = Date.now();
                console.log(this.gifs);
                }) 
                .catch(function (error) {
                // handle error
                console.log(error);
                })
                .then(function () {
                // always executed
                });
        },
        
    },
    
}
</script>

<style lang="scss">
.gifs{
    padding: 20px;
}
</style>