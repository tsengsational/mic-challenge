<template>
  <div id="app">
    <article-table :articles="displayArticles" v-on:sortChange="sortArticles" ></article-table>
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
      fetchCount: 0,
      sortBy: "none",
      sortWordsAsc: false,
      sortSubAsc: false,
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
      if (this.articles.length > 0) {
        this.addArticles(10)
        this.sortArticles(this.sortBy)
      } else if (this.articles.length === 0 && this.fetchCount < 2) {
          this.getArticles("/more-articles.json")
            .then(() => {
                this.addArticles(10)
                this.sortArticles(this.sortBy)
              } 
            ) 
      } else {
          throw "no more articles"
      }
    },
    addArticles: function(num) {
      let addArr = this.articles.splice(0, num)
      this.displayArticles = this.displayArticles.concat(addArr)
    },
    sortArticles: function(sortType) {
      this.sortBy = sortType
      switch (sortType) {
        case "words":
          this.sortWordsAsc = !this.sortWordsAsc
          this.sortWordsAsc ? this.displayArticles.sort(this.compareWords) : this.displayArticles.sort(this.compareWordsReverse)
          break;
        case "submitted":
          this.sortSubAsc = !this.sortSubAsc
          this.sortSubAsc ? this.displayArticles.sort(this.compareSubmitted) : this.displayArticles.sort(this.compareSubmittedReversed)
          break;
        default:
          this.displayArticles.sort()
          break;
      }
    },
    compareWords: function(a, b) {
      if (a.words < b.words) {
        return -1;
      } else if (a.words > b.words) {
        return 1;
      } else {
        return 0
      }
    },
    compareWordsReverse: function(a, b) {
      if (a.words > b.words) {
        return -1;
      } else if (a.words < b.words) {
        return 1;
      } else {
        return 0
      }
    },
    compareSubmitted: function(a, b) {
      let aDate = new Date(a.publish_at)
      let bDate = new Date(b.publish_at)
      if (aDate < bDate) {
        return -1;
      } else if (aDate > bDate) {
        return 1;
      } else {
        return 0
      }
    },
    compareSubmittedReversed: function(a, b) {
      let aDate = new Date(a.publish_at)
      let bDate = new Date(b.publish_at)
      if (aDate > bDate) {
        return -1;
      } else if (aDate < bDate) {
        return 1;
      } else {
        return 0
      }
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
