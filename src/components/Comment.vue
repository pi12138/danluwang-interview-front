<template>
  <div class="container">
      <div class="commentForm">
          <form>
              <div class="row">
                <div class="form-group col-md-10">
                    <input type="email" class="form-control" v-model="email" placeholder="发表评论的话，请留下你的邮箱吧" required>
                </div>
                <div class="form-group col-md-2">
                    <input type="button" value="发表" class="form-control" v-show="btnShow" @click="createComment">
                </div>
              </div>
              <div class="form-group">
                  <textarea cols="30" rows="3" v-model="content" class="form-control drag" placeholder="在此处发表评论" required></textarea>
              </div>
          </form>
      </div>

      <div id="commentList">
        <h4>评论列表</h4>
        <ul class="list-group list-group-flush">
            <li class="list-group-item text-left" v-for="(item, index) in commentList" :key="index">
                <div v-text="item.content"></div> 
            </li>
        </ul>
      </div>
    
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'Comment',
  data(){
      return {
        baseUrl: 'http://127.0.0.1:8000/api/blog/comment/',
        commentList: [
            {'user_address': '上海', 'email': '1558255789@qq.com', 'content': '写的还行'},
            {'user_address': '上海', 'email': '1558255789@qq.com', 'content': '写的还不错'}
        ],

        email: '',
        content: '',
        articleId: this.$route.params.id
      }
  },
  methods: {
      getCommentList(){
        //   获取评论列表
          let url = this.baseUrl

          this.$axios.get(url, {articleId: this.articleId})
          .then((result) => {
              this.commentList = result.data
          }).catch((err) => {
              console.log(err.response)
          });
      },

      createComment(){
        // 创建评论
        let url = this.baseUrl
        let data = {
            'email': this.email,
            'content': this.content
        }

        this.$axios.post(url, {data})
        .then((result) => {
            this.commentList.push(result.data)
        }).catch((err) => {
            console.log(err.response)
        });
        
      }
  },
  computed: {
      btnShow: function(){
          if (this.email === "" || this.content === ""){
              return false
          }
          return true
      }
  },
  beforeMount(){
      this.getCommentList()
  }
}
</script>

<style scoped>
    .drag{
        resize: none;
    }
</style>