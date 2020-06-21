<template>
  <div class="container">
      <ul class="list-group list-group-flush col-md-6 offset-md-3">
        <a class="list-group-item" v-for="(item, index) in blogList" :key="index" :href="articleUrl(item.id)">
            <div v-text="item.title" class="text-left"></div>
        </a>
      </ul>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'Blog',
  data(){
      return {
        blogList: [
            {'title': '第一篇文章', id: 1},
            {'title': '第二篇文章', id: 2}
        ],
        baseUrl: 'http://127.0.0.1:8000/api/blog/'
      }
  },
  methods: {
      getBlogList(){
          let url = this.baseUrl

          this.$axios.get(url)
          .then((result) => {
              this.blogList = result.data
          }).catch((err) => {
              console.log(err.response)
          });
      },

      articleUrl(articleId){
          return `/acticle/${articleId}`
      }
  },

}
</script>
