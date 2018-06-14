<template>
    <tr class="article-row" :class="{ even: isEven }"  >
        <td><div class="article-title"><div class="article-image" :style="imageStyle" ></div> <span class="title-text" ><a :href="article.url">{{article.title}}</a> </span> </div>  </td>
        <td><div class="article-author"> <a href="#">{{name}}</a></div> </td>
        <td><div class="article-words">{{article.words}}</div> </td>
        <td><div class="article-submitted">{{time}}</div> </td>
    </tr>
</template>

<script>
export default {
    props: ["article", "index"],
    computed: {
        name: function() {
            return this.article.profile.first_name + " " + this.article.profile.last_name
        },
        time: function() {
            let now = new Date()
            let publishDate = new Date(this.article.publish_at)
            let diffMs = (now - publishDate); // milliseconds between now & publishDate
            let years = Math.floor(diffMs / 31556952000) // years
            let months = Math.floor(diffMs / 2592000000) // months
            let days = Math.floor(diffMs / 86400000); // days
            let hrs = Math.floor(diffMs / 3600000); // hours
            let mins = Math.round(diffMs / 60000); // minutes
            let timeParse = ""
            if ( mins <= 59 ) {
                timeParse = mins + " minutes ago"
            } else if ( hrs > 0 && days < 1 ) {
                let hourParse = hrs === 1 ? " hour ago" : " hours ago"
                timeParse = hrs + hourParse
            } else if ( days > 0 && months < 1 ) {
                let dayParse = days === 1 ? " day ago" : " days ago"
                timeParse = days + dayParse
            } else if ( months > 0 && years < 1 ) {
                let monthParse = months === 1 ? " month ago" : " months ago"
                timeParse = months + monthParse
            } else if ( years > 0 ) {
                let yearParse = years === 1 ? " year ago" : " years ago"
                timeParse = years + yearParse
            }
            return timeParse
        },
        isEven: function() {
            let test = this.index % 2 === 0 ? true : false;
            return test
        },
        imageStyle: function() {
            return {
                backgroundImage: `url(${this.article.image})`,
                backgroundPosition: 'center',
                backgroundRepeat: 'no-repeat',
                backgroundSize: '200px'
            }
        }
    }
}
</script>

<style lang="scss">
    .article-row {
        background-color: #f4f4f4;
        transition: background-color .6s;
        a {
            color: #00567e;
            text-decoration: none;
            transition: color .3s;
            &:hover {
                color: #157cac;
            }
        }
        td {
            padding: 20px 10px;
            border-bottom: 1px #eaeaea solid;
            vertical-align: middle;
        }

        .article-title {
            width: 50vw;
        }

        .article-words {
            text-align: center;
            font-size: 14px;
            width: 8vw;

        }

        .article-submitted {
            text-align: center;
            font-size: 14px;
            width: 8vw;
        }

        .title-text {
            
            font-weight: 600;
            position: relative;
            left: 10px;
            font-size: 18px;
            display: inline-block;
            width: calc(100% - 120px);
        }

        .article-author {
            font-size: 14px;
            width: 8vw;
        }

        .article-submitted {
            width: 8vw;
        }

        .article-image {
            width: 50px;
            height: 50px;
            display: inline-block;
            vertical-align: top;
        }

        &.even {
            background-color: #f8f8f8;
        }
    }

     .article-row:hover, .article-row.even:hover {
            background-color: #d6d6d6;
        }

</style>
