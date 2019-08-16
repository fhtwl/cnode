<template>
        <div>
            <div class="postList"
            @touchstart="touchstart($event)"
            @touchmove="touchmove($event)"
            @touchend="touchend($event)">
                <div class="item" v-for="(item,index) in arr" :key="index">
                    <a href="#" class="author"><img :src="item.author.avatar_url" alt=""></a>
                    <div class="tag">
                        <span v-if="item.top">置顶</span>
                        <span v-else-if="item.tab == 'share'">分享</span>
                        <span v-else-if="item.tab == 'ask'">问答</span>
                        <span v-else-if="item.tab == 'job'">招聘</span>
                        <span v-else-if="item.tab == 'good'">精华</span>
                        <span v-else>11</span>
                    </div>
                    <router-link class="txt" :to="{ path: '/content', query: { id: item.id }}" :title="item.author_id">
                        <div class="title row_1">{{item.title}}</div>
                        <div class="label">
                            <div class="num">{{item.reply_count}}/{{item.visit_count}}</div>
                            <div class="time">{{item.last_reply_at | time}}</div>
                        </div>
                    </router-link>
                    
                </div>

                <div
                v-loading="loading"
                element-loading-text="拼命加载中"
                element-loading-spinner="el-icon-loading"
                element-loading-background="#fff"
                :data="arr"
                style="width: 100%;height:1rem;color:#333;">
                <!-- <div class="loading" v-if="loading">
                    Loading...
                </div> -->
                </div>
            </div>
        </div>

    
</template>

<script>
import Vue from 'vue' 
import axios from 'axios'
import { setTimeout } from 'timers';
import { constants } from 'crypto'
export default {
    name: 'PostList',
    data() {
        return {
            arr: [],
            _arr:[],
            loading:true,
            page:1,
            is_load:true,
            top:0
        }
    },
    methods: {
        getJson(){
            axios({
                url:"https://cnodejs.org/api/v1/topics?limit=20&page="+this.page,
                method:"get"
            })
            .then(response => {
                if( response.data.success === true ){
                      this._arr = this.arr.concat(response.data.data);
                      console.log(response.data.data)
                    //   if(this._arr.length > 60) {
                    //       this._arr.splice(0,20)
                    //   }
                      this.arr = this._arr
                      this.loading = false;
                      this.page++
			  	}
            })
            
        },
        touchstart(e) {

        },
        touchmove(e) {
            
        },
        touchend(e) {
            var top = this.getScrollTop()   
            var height = parseInt(window.getComputedStyle(document.querySelector('.postList')).getPropertyValue('height'))
            // var height = 3280*document.body.clientWidth/750
            // console.log(top,window.innerHeight,height)
            if(top + window.innerHeight >= height && !this.loading) {
                this.loading = true
                // 防止多次触发
                this.getJson()
                
            }
        },
        getLink(author){
            this.$router.push({
                path: `/about/${author}`,
            })
        },
        
        getScrollTop() {
            // 考虑到浏览器版本兼容性问题，解析方式可能会不一样
            return document.documentElement.scrollTop || document.body.scrollTop
        },
        // setLoad(index) {
        //     Vue.set(this.arr,index,true)
        // },
        is_bottom() {
            var top = this.getScrollTop()
            // var height = parseInt(window.getComputedStyle(document.querySelector('.postList')).getPropertyValue('height'))
            var height = 3280*document.body.clientWidth/750
            console.log(top,window.innerHeight,height)
            if(top + window.innerHeight >= height) {
                return true
            }
        },
        Ftime(timespan) {
            var dateTime = new Date(timespan * 1000);
            var year = dateTime.getFullYear();
            var month = dateTime.getMonth() + 1;
            var day = dateTime.getDate();
            var hour = dateTime.getHours();
            var minute = dateTime.getMinutes();
            //当前时间
            var now = Date.parse(new Date()); //typescript转换写法
            var milliseconds = 0;
            var timeSpanStr;
            //计算时间差
            milliseconds = (now / 1000) - timespan;
            
            //一分钟以内
            if (milliseconds <= 60) {
                timeSpanStr = '刚刚';
            }
            //大于一分钟小于一小时
            else if (60 < milliseconds && milliseconds <= 60 * 60) {
                timeSpanStr = Math.ceil((milliseconds / (60))) + '分钟前';
            }
            //大于一小时小于等于一天
            else if (60 * 60 < milliseconds && milliseconds <= 60 * 60 * 24) {
                timeSpanStr = Math.ceil(milliseconds / (60 * 60)) + '小时前';
            }
            //大于一天小于等于15天
            else if (60 * 60 * 24 < milliseconds && milliseconds <= 60 * 60 * 24 * 30) {
                timeSpanStr = Math.ceil(milliseconds / (60 * 60 * 24)) + '天前';
            }
            //大于一个月小于一年
            else if (60 * 60 * 24 * 30 < milliseconds && milliseconds <= 60 * 60 * 24 * 30 * 12){
                timeSpanStr = Math.ceil(milliseconds / (60 * 60 * 24 * 30)) + '个月前';
            }
            //超过一年显示
            else {
                timeSpanStr = year + '年' + month + '月' + day + '日 ' + hour + ':' + minute;  
            }
            return timeSpanStr;
            }
    },
    mounted:function () {
        this.getJson()
    } ,
    filters: {
        time: function (value) {
            var dateTimeStamp = value
            let result = ""
            var minute = 1000 * 60;
            var hour = minute * 60;
            var day = hour * 24;
            var halfamonth = day * 15;
            var month = day * 30;
            var now = new Date().getTime();
            var diffValue = now - new Date(dateTimeStamp).getTime();
            if(diffValue < 0){return;}
            var monthC =diffValue/month;
            var weekC =diffValue/(7*day);
            var dayC =diffValue/day;
            var hourC =diffValue/hour;
            var minC =diffValue/minute;
            if(monthC>=1){
                result="" + parseInt(monthC) + "月前";
            }
            else if(weekC>=1){
                result="" + parseInt(weekC) + "周前";
            }
            else if(dayC>=1){
                result=""+ parseInt(dayC) +"天前";
            }
            else if(hourC>=1){
                result=""+ parseInt(hourC) +"小时前";
            }
            else if(minC>=1){
                result=""+ parseInt(minC) +"分钟前";
            }
            else
            result="刚刚";
            return result;

        }
    }

  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
    .postList {
        font-size:0.26rem;
        padding:0.2rem;
        box-sizing: border-box;
        // height:49rem;
        .item {
            display: flex;
            justify-content: space-between;
            align-items:center;
            padding:0.1rem 0;
            border-bottom:0.01rem solid #f5f5f5;
            .author {
                font-size:0;
                width:0.6rem;
                height:0.6rem;
                img {
                    width:100%;
                    height:100%;
                    border-radius:50%;
                }
            }
            .tag {
                font-size:0.20rem;
                background-color: #e5e5e5;
                color: #999;
                padding:0.04rem;
                border-radius: 0.04rem;
                // margin:0 0.1rem;
                align-self: flex-start;
            }
            .txt {
                width:5.8rem;
                .title {
                   color:#333; 
                   line-height:1.3;
                }
                .label {
                    font-size:0.22rem;
                    color: #b4b4b4;
                    display: flex;
                    justify-content: space-between;
                    margin-top:0.1rem;
                }
            }
        }
    }
</style>
