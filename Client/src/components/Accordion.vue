<template>
    <div role="tablist">
        <div v-infinite-scroll="loadMore" infinite-scroll-disabled="busy" infinite-scroll-distance="10">

        </div>
        <b-container class="grantList">
                <b-row>
                    <b-col cols="2">
                        <div>year</div>
                    </b-col>
                        <b-col class="panel-left col-5" v-infinite-scroll="loadMore" infinite-scroll-disabled="busy" infinite-scroll-distance="limit">
                            <b-card no-body v-for="(data, index) in grantList" v-if="index % 2" v-bind:key="data.grants">
                                <b-card-header class="p-1 animated fadeInUpBig" header-tag="header" role="tab">
                                    <b-button block href="#" variant="info" v-b-toggle="'accordion' + data.id"><div><b>{{data.organization.name}}</b></div> <div>${{formatMoney(data.amount)}}</div></b-button>
                                </b-card-header>
                                <b-collapse accordion="my-accordion" role="tabpanel" :id="'accordion' + data.id">
                                    <b-card-body>
                                        <b-card-text>Given in {{formatDate(data.startDate)}} "{{ data.title }}"</b-card-text>
                                    </b-card-body>
                                </b-collapse>
                            </b-card>
                        </b-col>
                        <b-col class="panel-left col-5">
                            <b-card no-body v-for="(data, index) in grantList" v-if="indexmath(index)" v-bind:key="data.grants">
                                <b-card-header class="p-1 animated fadeInUpBig" header-tag="header" role="tab">
                                    <b-button block href="#" variant="info" v-b-toggle="'accordion' + data.id"><div><b>{{data.organization.name}}</b></div> <div>${{formatMoney(data.amount)}}</div></b-button>
                                </b-card-header>
                                <b-collapse accordion="my-accordion" role="tabpanel" :id="'accordion' + data.id">
                                    <b-card-body>
                                        <b-card-text>Given in {{formatDate(data.startDate)}} "{{ data.title }}"</b-card-text>
                                    </b-card-body>
                                </b-collapse>
                            </b-card>
                        </b-col>
                </b-row>
        </b-container>
    </div>
</template>

<script>
    import { mapState } from 'vuex'
    import infiniteScroll from 'vue-infinite-scroll'

    export default {
        name: 'accordion',
        data() {
            return {
                busy: false,
                grantList: [],
                limit: 15
            }
        },
        directives: { infiniteScroll },
        mounted() {
            if (this.$route.name === 'region-grants' || this.$route.name === 'grants' || this.$route.name === 'topic-grants') {
                this.$store.dispatch('loadFiltered')
            }

            this.loadMore()
        },
        methods: {
            formatDate(value) {
                var moment = require('moment');
                var grantDate = moment(value).format('MMMM YYYY')
                return grantDate;
            },
            formatMoney(value) {
                return value.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")
            },
            indexmath(val) {
                if (val % 2) return false
                else return true
            },
            loadMore: function () {
                this.busy = true;
                const append = this.grants.slice(
                    this.grantList.length,
                    this.grantList.length + this.limit
                );
                this.grantList = this.grantList.concat(append);
                this.busy = false;
             }
        },
        computed: {
            ...mapState(['grants'])
        }
}
</script>

<style scoped>

@import "https://cdn.jsdelivr.net/npm/animate.css@3.7.2"; /*Animation library*/


    .grantList {
        position:relative;
        overflow-y:scroll; 
        height: 40vh;
        width: 80vw;
    }
    .btn,
    .card,
    .card-header,
    .btn-info,
    .btn-info:focus,
    .btn-info:not(:disabled):not(.disabled):active {
        background: white;
        color: black;
        background-color: white;
        border: none;
        box-shadow: none;
    }
    .btn-block {
        font-size: 24px;
        display: inherit;
        text-align: left;
        font-family: DINOT;
    }
    .card-body {
        margin: auto;
        width: 80%;
    }

    .icon::before {
        display: inline-block;
        font-style: normal;
        font-variant: normal;
        text-rendering: auto;
        -webkit-font-smoothing: antialiased;
    }

    .card-header > a:before {
        color:#ce6b29;
        float: left !important;
        font-family: "Font Awesome 5 Free";
        font-size:48px;
        font-weight: 900;
        content: "\f056";
        padding-right: 20px;
    }
    .card-header {
        min-height: 100px;
    }
    .card-header > a.collapsed:before {
        color:#ce6b29;
        float: left !important;
        font-family: "Font Awesome 5 Free";
        font-size:65px;
        font-weight: 900;
        content: "\f055";
    }
</style>

