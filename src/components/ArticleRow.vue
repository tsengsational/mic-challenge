<template>
    <tr>
        <td>{{article.title}} </td>
        <td>{{name}} </td>
        <td>{{article.words}} </td>
        <td>{{time}} </td>
    </tr>
</template>

<script>
export default {
    props: ["article"],
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
        }
    }
}
</script>

<style lang="scss">

</style>
