<template>
    <b-container class="grant-total">
        <b-row align-h="center">
            <b-col cols="3">
                <span class="counter">{{ numberWithCommas(Count) }}</span><span class="grant-text">GRANTS</span>
            </b-col>
            <b-col cols="6">
                <span class="counter">${{ numberWithCommas(Amount) }}</span><span class="grant-text">provided since 1999</span>
            </b-col>
        </b-row>
    </b-container>
</template>

<script>
    import {mapState} from 'vuex'
    export default {
        name: 'grantTotal',
        created() {
            this.$store.dispatch('totalCount')
        },
        methods: {
            numberWithCommas(x) {
                return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
            }
        },
        computed: mapState([
            'Amount', 'Count'
        ])
    }
</script>

<style>
.grant-total {
    /* display: flex; */
    font-family:'Dinot-Bold'!important;
    /* justify-content: center; */
    /* align-items: center; */
    /* padding-bottom: 80px; */
}

.counter {
    /* display: inline-block; */
    font-size:60px;
    -webkit-animation: flip .2s 2 linear;
    animation: flipX .2s 2 linear;
}

.grant-text {
    font-size:36px;
    padding-left:12px;
}

/* .two {
    padding-left: 30px;
} */

</style>
