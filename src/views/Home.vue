<template>
  <div class="home">
    <Head></Head>
    <NavList></NavList>
    <PostList></PostList>
  </div>
</template>

<script>
// @ is an alias to /src
import Head from '@/components/Head.vue'
import NavList from '@/components/NavList.vue'
import PostList from '@/components/PostList.vue'

export default {
  name: 'home',
  components: {
    Head,
    NavList,
    PostList
  }
}
</script>
