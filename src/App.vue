<template>
  <div id="app">
    <article-table :articles="displayArticles" ></article-table>
    <button class="more-btn" @click="loadMore" >Load More</button>
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
      articles: [],
      displayArticles: [],
      fetchCount: 0
    }
  },
  methods: {
    getArticles: function(url) {
      return fetch(url)
        .then (resp => resp.json() 
        )
        .then (json => {
            this.articles = json
            this.fetchCount++
          }
        )
    },
    loadMore: function() {
      console.log(this.articles.length, this.fetchCount)
      if (this.articles.length > 0) {
        this.addArticles(10)
      } else if (this.articles.length === 0 && this.fetchCount < 2) {
          this.getArticles("/more-articles.json")
            .then(() => {
                this.addArticles(10)
              } 
            ) 
      } else {
          throw "no more articles"
      }
    },
    addArticles: function(num) {
      let addArr = this.articles.splice(0, num)
      this.displayArticles = this.displayArticles.concat(addArr)
    }
  },
  mounted: function() {
    let url = "/articles.json"
    this.getArticles(url)
      .then (()=> {
        this.loadMore()
      }
      )
  }
}
</script>

<style lang="scss">
#app {
  font-family: Helvetica, Arial, sans-serif;
  letter-spacing: -.25px;

  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.more-btn {
  padding: 15px;
  width: 200px;
  height: 60px;
  font-size: 16px;
  background: linear-gradient(#c21e46, #8b1130);
  border-radius: 10px;
  border: 3px #8b1130 solid;
  margin: 16px 0;
  color: #fff;
  text-transform: uppercase;
  font-weight: 600;

  &:active {
    background: linear-gradient(#8b1130, #560b1d)
  }
}
</style>
