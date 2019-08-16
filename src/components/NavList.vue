<template>
  <div class="nav">
      <router-link :to="item.link" v-for="(item,index) in arr" :key="index" :class="{active:item.active}">{{item.name}}</router-link>
  </div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'NavList',
    data() {
        return {
            arr: [
                {name:"全部",link:'/1',active:true},
                {name:"精华",link:'/2',active:false},
                {name:"分享",link:'/3',active:false},
                {name:"问答",link:'/4',active:false},
                {name:"招聘",link:'/5',active:false},
            ]
        }
    },
    methods: {
        getLink(author){
            this.$router.push({
                path: `/about/${author}`,
            })
        }
    },

  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
    .nav {
        font-size:0.22rem;
        background:#f6f6f6;
        display: flex;
        align-items: center;
        height:0.5rem;
        padding-left:0.2rem;
        a {
            color:#80bd01;
            margin-right:0.2rem;
            padding:0.06rem 0.1rem;
            border-radius: 0.04rem;
        }
        a.active {
            background:#80bd01;
            color:#fff;
        }
    }
</style>
