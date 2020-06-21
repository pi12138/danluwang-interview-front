<template>
  <div class="container">
      <h1>This is a Article</h1>
      <h2>{{ this.$route.params.id }}</h2>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'Article',
  data(){
      return {
        blogList: [],
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
      }
  }
}
</script>
