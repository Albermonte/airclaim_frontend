<template>
<div class="intro-info text-center q-pa-lg">
    <div v-if="airclaimStatus === 'Running'">
        <div class="text-h4 text-weight-medium q-mb-none">Welcome to the</div>
        <div class="text-h3 text-weight-medium text-primary q-mb-md">Freeos AirClaim</div>

        <div class="text-weight-regular">
            <div class="text-h5 q-mb-md q-mt-lg">
                Access the Freeos Economic System here:
            </div>
        </div>
        <div class="q-mt-xs q-mb-sm" v-if="isFreeosEnabled === true">
            <q-btn unelevated no-caps size="lg" outline color="primary" @click="connectWallet('anchor')">Connect Wallet</q-btn>
        </div>
        <div class="q-mt-xs q-mb-sm" v-if="isFreeosEnabled === false">
            <div class="q-pa-md  bg-negative" style="max-width:600px;margin:0 auto;">
                <h4 class="text-white">The AirClaim is unavailable at this time, please try later </h4>
            </div>
        </div>
    </div>
    <div v-if="airclaimStatus === 'Pending'">
        <div class="text-h4 text-weight-medium q-mb-none">Welcome, the</div>
        <div class="text-h3 text-weight-medium text-primary q-mb-none">Freeos AirClaim</div>
        <div class="text-h3 text-weight-medium q-mb-md">Starts in</div>
        <Countdown :startDate="nextIteration.startStamp" />
    </div>
    <div v-if="airclaimStatus === 'Complete'">
        <div class="text-h3 text-weight-medium text-primary q-mb-none">the Freeos AirClaim</div>
        <div class="text-h3 text-weight-medium q-mt-sm q-mb-mb">Has now Closed</div>
        <div class="text-h6 text-weight-medium q-mt-md q-mb-mb">You can still login to Check your Balance, Mint & Transfer for a limited time.</div>
        <div class="q-mt-md q-mb-sm" v-if="isFreeosEnabled === true">
            <q-btn unelevated no-caps size="lg" outline color="primary" @click="connectWallet('anchor')">Connect Wallet</q-btn>
        </div>
    </div>
    <div v-if="!airclaimStatus">
            <div class="q-pa-md  bg-negative" style="max-width:600px;margin:0 auto;">
                <h4 class="text-white">The AirClaim is unavailable at this time, please try later </h4>
            </div>
    </div>
    <div class="text-h6 q-mt-md text-weight-medium q-mb-none">For more info visit <a target="_blank" href="https://freeos.io/">freeos.io</a></div>
    <div class="q-mt-lg" style="">
        <img src="../assets/join-screen-image.svg" alt="">
    </div>
</div>

</template>

<script>
import {
    mapState,
    mapActions,
    mapGetters
} from 'vuex'
import Countdown from "components/Countdown.vue";

export default {
    name: 'PageIndex',
    components: {
        Countdown
    },
    computed: {
        //...mapGetters('account', ['isAuthenticated']),
        ...mapGetters('freeos', ['isFreeosEnabled', 'isAuthenticated', 'currentIteration', 'nextIteration','airclaimStatus'])
    },
    methods: {
        ...mapActions('account', ['checkIfLoggedIn', 'connectWallet', 'logout', 'getAccountInfo', 'getClaimDetailInfo']),
        ...mapActions('freeos', ['fetch'])
    },
    watch: {
        isAuthenticated: {
            immediate: true,
            handler: function (val, oldVal) {
                console.log(val, oldVal)
                if (val === true && this.isFreeosEnabled !== false) {
                    console.log('this.$route.query', this.$route.query.returnUrl)
                    if (this.$route.query.returnUrl) {
                        this.$router.push({
                            path: this.$route.query.returnUrl
                        })
                    } else {
                        this.$router.push({
                            path: '/claim'
                        })
                    }
                }
            },
        },
    },
    async mounted() {}
}
</script>

<style>
.q-focus-helper {
    display: none;
}

.add-hover {
    transition: .3s all ease-in-out;
}

.add-hover:hover {
    color: #fff !important;
    border: none !important;
    background-color: #f7931e;
    transition: .3s all ease-in-out;
}
</style>
