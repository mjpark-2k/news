<template lang="">
    <div class="news" v-if="true">
        <div class="btn">
            <button 
            v-for="tab in tabs" :key="tab.value"
            @click="tab.value === 'all' ? openApiAll() : openApi(tab.value)"
            :class="{active: tabActive === tab.value}">
            {{ tab.label }}
        </button>
        </div>
        <div class="newscard" v-for="(card, i) in news" :key="i">
            <a :href="card.content_url" target="_blank">
                <figure><img :src="card.thumbnail_url || 'example.jpg'"/></figure>
                <div class='txt'>
                    <p>{{ card.published_at }}</p>
                    <h3>{{ card.title }}</h3>
                    <p>{{ card.publisher }} {{ card.author }}</p>
                </div>
            </a>
        </div>
        <div v-if="!news.length && tabActive === 'tech'">
            <p>기술/IT 관련 뉴스가 없습니다.</p>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    props:["content"],
    data(){
        return{
            news: [],
            tabActive: 'all',
            tabs: [
                { value: 'all', label: '전체' },
                { value: 'politics', label: '정치' },
                { value: 'economy', label: '경제' },
                { value: 'society', label: '사회' },
                { value: 'culture', label: '문화' },
                { value: 'world', label: '세계' },
                { value: 'tech', label: '기술/IT' },
                { value: 'entertainment', label: '연예' },
                { value: 'opinion', label: '사설' }
            ]
        };
    },
    created(){
        this.openApiAll()
    },
    methods: {
        async openApiAll(){
            const res = await axios.get(`https://server-ten-dusky.vercel.app/news?t=${this.content}`);
            this.news = res.data.data
            this.tabActive = 'all';
        },
        async openApi(c){
            const res = await axios.get(`https://server-ten-dusky.vercel.app/news/sec?t=${this.content}&s=${c}`);
            this.news = res.data.data
            this.tabActive = c;
        }
    },
}
</script>

<style lang="scss">
    .home,
    .about{
        justify-items: center;
    }
    .news{
        width: 843px;
    }
    .btn{
        margin-bottom: 15px;
        button{
            border: 0;
            background-color: white;
            font-size: 18px;
            font-weight: 500;
            color: #2c3e50;
            cursor: pointer;

            &.active {
                color: #42b983;
            }
        }
    }
    .newscard{
        border: 1px solid #ddd;
        margin: 10px 0;
        padding: 10px;
        
        
        a{
            display: flex;
            justify-content: space-around;
            align-items: center;

            text-decoration: none;
            color: inherit;
        }
        figure{
            margin: 0;
            width: 40%;
            img{width: 100%;}
        }
        .txt{
            width: 40%;
        }
    }
</style>