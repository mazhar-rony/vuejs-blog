<template>
  <!-- custom directive globaly register in main.js -->
  <!-- <div v-theme:column="'narrow'" id="show-blogs"> -->
  <div id="show-blogs">
    <h1>All Blog Articles</h1>
    <input type="text" v-model="search" placeholder="Search Blogs">
    <!-- <div v-for="blog in blogs" v-bind:key="blog" class="single-blog"> -->
    <!-- using computed function to search blogs -->
    <div v-for="blog in filteredBlogs" v-bind:key="blog" class="search-blog">
        <!-- custom directive globally register  -->
        <!-- <h2 v-rainbow>{{ blog.title }}</h2> -->
        <!-- filters Globally register in main.js-->
        <!-- <h2>{{ blog.title | to-uppercase }}</h2> -->
        <!-- <article>{{ blog.body | snippet }}</article> -->
        <!-- locally register filters and custom diretives-->
        <router-link v-bind:to="'/blog/' + blog.id"><h2 v-rainbow>{{ blog.title | toUppercase }}</h2></router-link>
        <article>{{ blog.content | snippet }}</article>
    </div>
  </div>
</template>

<script>

import serachMixin from '../mixins/searchMixin';

export default {
  data () {
    return {
        blogs: [],
        search: ''
    }
  },
  created() {
      // this.$http.get('https://jsonplaceholder.typicode.com/posts').then(function(data){
      //     //console.log(data);
      //     this.blogs = data.body.slice(0,10);
      // });
      this.$http.get('https://vuejs-blog-2def7.firebaseio.com/posts.json').then(function(data){
          //console.log(data.json());
          return data.json();
      }).then(function(data){
          
          var blogsArray = [];
          
          for(let key in data){
            //console.log(key);
            //console.log(data[key]);
            data[key].id = key;
            blogsArray.push(data[key]);
          }
          //console.log(data);
          //console.log(blogsArray);
          this.blogs = blogsArray;
      });
  },
  //for search blogs
  computed: {
    // filteredBlogs: function(){
    //   return this.blogs.filter((blog) => {
    //     return blog.title.match(this.search);
    //   });
    // }
  },
  // locally register filters
  filters: {
    toUppercase(value){
      return value.toUpperCase();
    },
    snippet(value){
      return value.slice(0, 100) + "...";
    }
  },
  //locally register custom directives
  directives: {
    'rainbow': {
      bind(el, binding, vnode){
        el.style.color = "#" + Math.random().toString().slice(2,8);
      }
    }
  },
  mixins: [serachMixin]
}
</script>

<style scoped>
#show-blogs{
    max-width: 800px;
    margin: 0 auto;
}
.search-blog{
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
