<template>
  <!-- custom directive
  <div v-theme:column="'narrow'" id="show-blogs"> -->
  <div id="show-blogs">
    <h1>All Blog Articles</h1>
    <input type="text" v-model="search" placeholder="Search Blogs">
    <!-- <div v-for="blog in blogs" v-bind:key="blog" class="single-blog"> -->
    <!-- using computed function to search blogs -->
    <div v-for="blog in filteredBlogs" v-bind:key="blog" class="single-blog">
        <!-- custom directive
        <h2 v-rainbow>{{ blog.title }}</h2> -->
        <!-- filters -->
        <h2>{{ blog.title | to-uppercase }}</h2>
        <article>{{ blog.body | snippet }}</article>
    </div>
  </div>
</template>

<script>

export default {
  data () {
    return {
        blogs: [],
        search: ''
    }
  },
  created() {
      this.$http.get('https://jsonplaceholder.typicode.com/posts').then(function(data){
          //console.log(data);
          this.blogs = data.body.slice(0,10);
      });
  },
  computed: {
    filteredBlogs: function(){
      return this.blogs.filter((blog) => {
        return blog.title.match(this.search);
      });
    }
  }
}
</script>

<style>
#show-blogs{
    max-width: 800px;
    margin: 0 auto;
}
.single-blog{
    padding: 20px;
    margin: 20px 0;
    box-sizing: border-box;
    background: #eee;
}
input{
  width: 100%;
  height: 30px;
}
input[type=text]{
  font-size: 20px;
  padding: 0 10px;
  box-sizing: border-box;
}
</style>
