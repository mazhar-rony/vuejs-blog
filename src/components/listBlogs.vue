<template>
 <div id="show-blogs">
    <h1>List Blog Titles</h1>
    <input type="text" v-model="search" placeholder="Search Blogs">
    <div v-for="blog in filteredBlogs" v-bind:key="blog" class="single-blog">
        <h2 v-rainbow>{{ blog.title | toUppercase }}</h2>
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
      this.$http.get('https://jsonplaceholder.typicode.com/posts').then(function(data){
          //console.log(data);
          this.blogs = data.body.slice(0,10);
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
  mixins:[serachMixin]
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
