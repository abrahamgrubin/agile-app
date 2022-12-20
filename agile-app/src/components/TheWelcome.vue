<template>
  <div id="app">
    <h1>Todo App</h1>
    <input type="text" v-model="name" placeholder="Blog name" />
    <input type="text" v-model="posts.item.title" placeholder="Blog Post" />
    <button v-on:click="createBlog">Create Todo</button>
    <div v-for="item in blogs" :key="item.id">
      <h3>{{ item.name }}</h3>
      <p>{{ item.post.title }}</p>
    </div>
  </div>
</template>

<script>
  import { API } from 'aws-amplify';
  import { createBlog } from '../graphql/mutations';
  import { listBlogs } from '../graphql/queries';

  export default {
    name: 'app',
    async created() {
      this.getBlogs();
    },
    data() {
      return {
        name: '',
        posts: {}, 
        blogs: []
      };
    },
    methods: {
      async createBlog() {
        const { name, posts } = this;
        if (!name || !posts) return;
        const blog = { name, posts };
        await API.graphql({
          query: createBlog,
          variables: { input: blog }
        });
        this.name = '';
        this.posts = {};
      },
      async getBlogs() {
        const blogs = await API.graphql({
          query: listBlogs
        });
        this.blogs = blogs.data.listBlogs.items;
        console.log(blogs.data)
      }
    }
  };
</script>