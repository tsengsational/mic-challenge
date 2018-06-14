<template>
    <div class="article-table" >
        <table>
            <tr class="table-header">
                <th><div class="header-title">Unpublished Articles ({{articles.length}})</div></th>
                <th><div class="header-author">Author</div></th>
                <th><div class="header-words"> Words <span class="sort-icon" @click="$emit('sortChange', 'words')"><font-awesome-icon :icon="sort" /></span></div></th>
                <th><div class="header-submitted">Submitted <span class="sort-icon" @click="$emit('sortChange', 'submitted')"><font-awesome-icon :icon="sort" /></span></div></th>
            </tr>
            <div class="table-content">
                <article-row v-for="(article, key) in articles" :key="key" :index="key" :article="article" ></article-row>
            </div>
        </table>    
    </div>
</template>

<script>
import ArticleRow from './ArticleRow'
import FontAwesomeIcon from '@fortawesome/vue-fontawesome'
import faSort from '@fortawesome/fontawesome-free-solid/faSort'

export default {
    components: {
        ArticleRow,
        FontAwesomeIcon
    },
    props: ["articles"],
    computed: {
        sort: function() {
            return faSort
        }
    }
}
</script>

<style lang="scss">
    .article-table {
        left: 10vw;
        position: relative;
        width: 80vw;
        table {
            border-collapse: collapse;
            th {
                background: linear-gradient(#157cac, #00567e);
                background-clip:padding-box;
                border-bottom: 1px #00567e solid;
                box-sizing: border-box;
                color: #fff;
                font-weight: 900;
                padding: 20px 10px;
                text-align: left;
                text-transform: uppercase;
                white-space: nowrap;
                z-index: 1;
            }

            .table-content {
                position: relative;
                top: 59px;
            }

            .sort-icon {
                cursor: pointer;
            }


            .table-header {
                background: linear-gradient(#157cac, #00567e);
                position: fixed;
                width: 79vw;
                top: 0;
                z-index: 1;
                .header-title {
                    min-width: 50vw;
                }
                .header-author, .header-words, .header-submitted {
                    min-width: 8vw;
                }
            }
        }
    }
</style>
