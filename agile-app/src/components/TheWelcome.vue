<template>
  <div id="app">
    <h1>Blog App</h1>
    <input type="text" v-model="name" placeholder="Blog name" />
    <input type="text" v-model="posts" placeholder="Post" />
    <button v-on:click="createBlog">Create Blog</button>
    <div v-for="item in blogs" :key="item.id">
      <h3>{{ item.name }}</h3>
    </div>
    <input type="text" v-model="title" placeholder="Post Title" />
    <input type="text" v-model="comments" placeholder="Post Comments"/> 
    <div v-for="item in posts" :key="item.id">
      <h2>{{ item.title }}</h2>
      <h2>{{ item.comments }}</h2>
    </div>
  </div>
</template>

<script>
  import { API } from 'aws-amplify';
  import { createBlog, createPost, createComment } from '../graphql/mutations';
  import { listBlogs, listPosts, listComments } from '../graphql/queries';

  export default {
    name: 'app',
    async created() {
      this.getBlogs();
      this.getPosts();
      this.getComments();
    },
    data() {
      return {
        name: '',
        posts: [],
        blogs: [], 
        comments: []
      };
    },
    methods: {
      async createPost(){
        
      },
      async createComment(){
        
      },
      async createBlog() {
        const { name, posts } = this;
        if (!name || !posts) return;
        const blog = { name, posts };
        await API.graphql({
          query: createBlog,
          variables: { input: blog }
        });
        this.name = '';
      },
      async getBlogs() {
        const blogs = await API.graphql({
          query: listBlogs
        });
        this.blogs = blogs.data.listBlogs.items;
        console.log(blogs.data.listBlogs.items)
      }, 
      async getPosts() {
        const posts = await API.graphql({
          query: listPosts
        });
        this.posts = posts.data.listPosts.items;
      },
      async getComments() {
        const posts = await API.graphql({
          query: listComments
        });
        this.posts = posts.data.listComments.items;
      }
    }
  };
</script>