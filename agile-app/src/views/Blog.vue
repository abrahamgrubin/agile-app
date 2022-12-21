<template>
    <h1>This is a blog with blog id {{this.blog.name}}</h1>
</template>

<script> 
import { API } from 'aws-amplify';
import { getBlog } from '../graphql/queries';
export default {
    data() {
      return {
          blog: {},
          blogID: this.$route.params.id
      };
    },
    async created() {
      console.log(this.blogID)
      this.getBlog();
    },
    methods: {
      async getBlog() {
        console.log(this.blogID)
        const blog = await API.graphql({
          query: getBlog, 
          variables: {id: this.blogID}
        });
        this.blog = blog.data.getBlog
      }
    }
}
</script>