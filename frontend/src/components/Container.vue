<template>
  <div class="row">
    <div class="col-md-4">
      <div class="form-group">
        <input type='hidden' v-model="url">
      </div>

      <div class="form-group">
        <input type='text' v-model="title" placeholder="标题">
      </div>

      <div class="form-group">
        <textarea class="form-control" v-model="content" placeholder="内容"></textarea>
      </div>

      <div class="form-group">
        <button class="btn btn-block btn-success" @click="saveBlog()">保存</button>
      </div>
    </div>

    <div class="col-md-8">
      <table class="table">
        <thead>
          <tr>
            <th>标题</th>
            <th>内容</th>
            <th>编辑</th>
            <th>删除</th>
          </tr>
        </thead>
        
        <tbody>
          <tr v-for="blog in blogs" :key="blog.url">
            <td>{{ blog.title }}</td>
            <td>{{ blog.content }}</td>
            <td>
              <button class="btn btn-success" @click='editBlog(blog)'>
                <i class="fa fa-edit"></i>
              </button>
            </td>
            <td>
              <button class="btn btn-success" @click='deleteBlog(blog)'>
                <i class="fa fa-trash"></i>
              </button>
            </td>
          </tr>
          
        </tbody>
      </table>
    </div>

  </div>


   
</template>

<script>
  import axios from 'axios';
  export default {
    name: 'Container',
    props: {
    },
    data(){
      return{
        base_url:'http://127.0.0.1:8000/blog/',
        blogs:null,
        url:'',
        title:'',
        content:''}
     },
    methods:{
      getAll(){
        axios.get(this.base_url)
          .then(res=>{
            this.blogs = res.data;
            this.url = '';
            this.title = '';
            this.content = ''
          });
      },
      saveBlog(){
        //两种情况， 一种是新建博客，另一种是修改博客
        //通过url是否为空来判定
        if (this.url == ''){
          //新增
          axios.post(this.base_url, {title: this.title, content: this.content})
            .then(()=>{
              this.getAll();
            })
        }else{
          //修改
          axios.put(this.url, {title: this.title, content: this.content})
          .then(()=>{
            this.getAll();
          })
        
        }


      },

      editBlog(blog){
        this.url = blog.url;
        this.title = blog.title;
        this.content = blog.content;
      },

      deleteBlog(blog){
        axios.delete(blog.url)
          .then(()=>{
            this.getAll();
          });
      }
    },
    
    mounted(){
      this.getAll();
    }
  }
</script>


