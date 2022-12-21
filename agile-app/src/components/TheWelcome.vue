<template>
  <div id="app">
    <h1>Blog App</h1>
    <input type="text" v-model="name" placeholder="Blog name" />
    <h2>New Post</h2>
    <input type="text" v-model="title" placeholder="Post Title"/>
    <h2>New Comment</h2>
    <input type="text" v-model="comment" placeholder="Post Comment" />
    
    <button v-on:click="createBlog">Create Blog</button>
    <button v-on:click="createPost">Create Post</button>
    <div v-for="item in blogs" :key="item.id">
      <RouterLink :to="{ name: 'blogs', params: { id: item.id } }"><h3>{{ item.name }}</h3></RouterLink>
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
    },
    data() {
      return {
        name: '',
        blogs: [],
        blog: {},
        posts: []
      };
    },
    methods: {
      async createBlog() {
        const { name } = this;
        if (!name) return;
        const blog = { name };
        await API.graphql({
          query: createBlog,
          variables: { input: blog }
        });
      },
      async createPost() {
        const { title } = this;
        const post = { title };
        debugger;
        await API.graphql({
          query: createPost, 
          variables: { input: post }
        })
      },
      async getBlogs() {
        const blogs = await API.graphql({
          query: listBlogs
        });
        this.blogs = blogs.data.listBlogs.items;
      },
      
    }
  };
</script>