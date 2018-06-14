<template>
  <div id="app">
    <article-table :articles="displayArticles" v-on:sortChange="sortArticles" ></article-table>
    <div class="btn-container" >
      <button class="more-btn" @click="loadMore" >Load More</button>
    </div>
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
          alert("No more articles. :(")
      }
    },
    addArticles: function(num) {
      let addArr = this.articles.splice(0, num)
      this.displayArticles = this.displayArticles.concat(addArr)
    },
    sortArticles: function(sortType) {
      this.sortBy = sortType
      window.localStorage.setItem("micSortBy", sortType)
      switch (sortType) {
        case "words":
          this.sortWordsAsc = !this.sortWordsAsc
          window.localStorage.setItem("micSortWordsAsc", this.sortWordsAsc)
          this.sortWordsAsc ? this.displayArticles.sort(this.compareWords) : this.displayArticles.sort(this.compareWordsReverse)
          break;
        case "submitted":
          this.sortSubAsc = !this.sortSubAsc
          window.localStorage.setItem("micSortSubAsc", this.sortSubAsc)
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
    },
    getPastSort: function() {
      let sortWordsAsc = window.localStorage.getItem("micSortWordsAsc") === "true" ? true : false
      let sortSubAsc = window.localStorage.getItem("micSortSubAsc") === "true" ? true : false
      let sortType = window.localStorage.getItem("micSortBy")
      return {
        sortWordsAsc, sortSubAsc, sortType
      }
    }
  },
  mounted: function() {
    let url = "/articles.json"
    this.getArticles(url)
      .then (()=> {
        let store = this.getPastSort()
        this.loadMore()
        if (store.sortType) {
          if(store.sortSubAsc === null) {null} 
          else {
            this.sortSubAsc = !store.sortSubAsc
          }
          if(store.sortWordsAsc === null) {null} 
          else {
            this.sortWordsAsc = !store.sortWordsAsc
          }
          this.sortArticles(store.sortType)
        }
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

body {
  margin: 0;
}

.more-btn {
  background-color: #fff;
  border: 2px #014768 solid;
  color: #00567e;
  height: 60px;
  font-size: 16px;
  font-weight: 600;
  margin: 16px 0;
  padding: 15px;
  position: absolute;
  right: 0;
  text-transform: uppercase;
  transition: background-color .3s, color .3s;
  width: 160px;

  &:hover {
    color: #fff;
    background-color: #00567e;
  }

}

.btn-container {
  height: 80px;
  left: 10vw;
  position: relative;
  margin-bottom: 32px;
  top: 59px;
  width: 80vw;
}

.hidden {
  display: none;
}
</style>
