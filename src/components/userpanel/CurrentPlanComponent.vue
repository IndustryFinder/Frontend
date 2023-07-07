<template>
  <v-sheet color="transparent" class="pa-4">
    <div class="pa-4">
      <v-row class="mb-5 mx-16">
        <strong>پلن فعال</strong>
      </v-row>
      <v-row>
        <v-spacer />
        <v-col cols="10" md="10" lg="10" xl="8" sm="8">
          <v-card
            v-if="this.$vuetify.breakpoint.xsOnly"
            width="100%"
            elevation="1"
            class="rounded-xl py-2 px-4 plan-card"
            outlined
            style="min-height: 181px; max-width: 277px; margin: 0 auto"
          >
            <v-card-text class="white--text">
              <v-row class="my-2" style="font-size: 1.2em; font-weight: bolder">
                <span>پلن فعال:</span>
                <v-spacer />
                <span>{{ currentPlan.planName }}</span>
              </v-row>
              <v-spacer class="my-8" />
              <v-row class="my-2" style="font-size: 1em">
                <span>روزهای باقی مانده:</span>
                <v-spacer />
                <span>{{ currentPlan.DaysRemaining }}</span>
              </v-row>
              <v-spacer class="my-8" />
              <v-row class="my-2" style="font-size: 1em">
                <span>آگهی های باقی مانده:</span>
                <v-spacer />
                <span>{{ currentPlan.AdsRemaining }}</span>
              </v-row>
              <v-spacer class="my-8" />
              <v-row class="my-2" style="font-size: 1em">
                <span>آگهی های استفاده شده:</span>
                <v-spacer />
                <span>{{ currentPlan.usedAds }}</span>
              </v-row>
            </v-card-text>
          </v-card>
          <v-card
            v-else
            width="100%"
            elevation="1"
            class="rounded-xl py-2 px-4 plan-card"
            outlined
            style="min-height: 218px; max-width: 377px; margin: 0 auto"
          >
            <v-card-text class="white--text">
              <v-row class="my-2" style="font-size: 1.2em; font-weight: bolder">
                <span>پلن فعال:</span>
                <v-spacer />
                <span>{{ currentPlan.planName }}</span>
              </v-row>
              <v-spacer class="my-8" />
              <v-row class="my-2" style="font-size: 1em">
                <span>روزهای باقی مانده:</span>
                <v-spacer />
                <span>{{ currentPlan.DaysRemaining }}</span>
              </v-row>
              <v-spacer class="my-8" />
              <v-row class="my-2" style="font-size: 1em">
                <span>آگهی های باقی مانده:</span>
                <v-spacer />
                <span>{{ currentPlan.AdsRemaining }}</span>
              </v-row>
              <v-spacer class="my-8" />
              <v-row class="my-2" style="font-size: 1em">
                <span>آگهی های استفاده شده:</span>
                <v-spacer />
                <span>{{ currentPlan.usedAds }}</span>
              </v-row>
            </v-card-text>
          </v-card>
        </v-col>
        <v-spacer />
      </v-row>
    </div>
  </v-sheet>
</template>

<script>
export default {
  name: "CurrentPlanComponent",
  data() {
    return {
      planColor: {
        NoPlan: "#ffffff",
        Classic: "#caf0f8",
        Pro: "#ade8f4",
        Deluxe: "#90e0ef",
        Max: "#48cae4",
      },
      currentPlan: {
        planName: "",
        DaysRemaining: "",
        AdsRemaining: "",
        usedAds: "",
      },
    };
  },
  methods: {
    async userInfo() {
      let axios = require("axios");
      let config = {
        method: "get",
        url: this.$store.state.host + "authentication/this",
        headers: {
          Accept: "application/json",
          Authorization: "Bearer " + this.$cookies.get("token"),
        },
      };
      let that = this;
      await axios(config).then(function (response) {
        that.currentPlan.planName = response.data.activePlan;
        const currentDate = new Date();
        const previousDate = new Date(response.data.PlanExpireDate);
        const timeDifference = previousDate.getTime() - currentDate.getTime();
        that.currentPlan.DaysRemaining = Math.floor(
          timeDifference / (1000 * 60 * 60 * 24)
        );
        that.currentPlan.AdsRemaining = response.data.AdsRemaining;
        console.log(that.currentPlan.DaysRemaining);
      });
    },
    async adFinder() {
      var axios = require("axios");
      var config = {
        method: "get",
        url: this.$store.state.host + "ad/searchBySender",
        headers: {
          Accept: "application/json",
          Authorization: "Bearer " + this.$cookies.get("token"),
        },
      };
      let that = this;
      await axios(config).then(function (response) {
        that.currentPlan.usedAds = response.data.length;
      });
    },
  },
  beforeMount() {
    this.adFinder();
    this.userInfo();
  },
};
</script>

<style scoped>
.plan-card {
  background-image: url("../../assets/plan-background.svg");
}
</style>
