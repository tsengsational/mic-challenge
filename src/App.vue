<template>
  <div id="app">
    <article-table :articles="articles" ></article-table>
  </div>
</template>

<script>
import ArticleTable from './components/ArticleTable'

export default {
  name: 'app',
  components: {
    ArticleTable
  },
  data: function () {
    return {
      articles: []
    }
  },
  methods: {
    getArticles: function() {
      fetch("/articles.json")
        .then (resp => resp.json() 
        )
        .then (json => {
            this.articles = json
          }
        )
    }
  },
  created: function() {
    this.getArticles()
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
