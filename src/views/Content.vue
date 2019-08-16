<template>
        <div>
            <Head></Head>
            <div>
                <div class="content_article">
                    <h1>{{post.title}}</h1>
                    <!-- <ul>
                        <li>• 分类:{{ postTab }}</li>
                        <li>• {{ post.visit_count }}次访问</li>
                        <li>• 发布于:{{ post.create_at | formatDate }}</li>
                        <li>
                            • 作者:
                            <router-link :to="{ name:'user_info',params:{name:post.author.loginname }}">{{ post.author.loginname }}</router-link>
                        </li>
                    </ul> -->
                    <div v-html="post.content" id="content"></div>
                </div>
                <div
                v-loading="loading"
                element-loading-text="拼命加载中"
                element-loading-spinner="el-icon-loading"
                element-loading-background="#fff"
                :data="post"
                style="width: 100%;height:1rem;color:#333;">
                </div>
            </div>
        </div>

    
</template>

<script>
import axios from 'axios'
import Head from '@/components/Head.vue'
export default {
    name: 'Content',
    data() {
        return {
            post:{},
            loading:true,
            page:1,
            is_load:true,
            top:0
        }
    },
    methods: {
        getJson(){
            let id = this.$route.query.id
            axios({
                url:"https://cnodejs.org/api/v1/topic/"+id,
                method:"get"
            })
            .then(response => {
                if( response.data.success === true ){
                    this.post = response.data.data;
                    console.log(this.post)
                    // //   if(this._arr.length > 60) {
                    // //       this._arr.splice(0,20)
                    // //   }
                    //   this.arr = this._arr
                      this.loading = false;
                    //   this.page++
			  	}
            })
            
        },

    },
    mounted:function () {
        this.getJson()
    } ,
    components:{
        Head
    }
  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
    .content_article {
        width:100%;
        #content {
            width:100%;
            margin:0 auto;
            font-size:0.15rem;
            line-height: 0.5rem;
            h2 {
                font-size: 0.26rem;
                width:6.9rem;
                margin:0.3rem auto 0.2rem auto;
                border-bottom:0.01rem solid #eee;
                padding:0 0 0.06rem 0;
            }
            p {
                width:6.9rem;
                margin:0 auto;
                line-height: 0.28rem;
                font-size: 0.15rem;
            }
            ul {
                width:6.9rem;
                margin: 0.1rem 0 0.1rem 0.55rem;
                li {
                    list-style-type: disc;
                    height:0.28rem;
                    line-height: 0.28rem;
                    font-size:0.14rem;
                }
            }
            a {
                color:#80bd01;
            }
            .prettyprint {
                width:6.9rem;
                margin:0 auto;
                overflow-x: scroll;
                padding:0.1rem 0.2rem;
                box-sizing: border-box;
            }
            .language-bash {
                background:#f7f7f7;
            }
        }
    }
</style>
