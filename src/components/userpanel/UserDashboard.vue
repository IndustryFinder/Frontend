<template>
  <v-app>
    <v-sheet>
      <v-sheet color="transparent" class="pa-4">
        <div class="pa-4">
          <v-row class="mb-5 mx-8">
            <strong>دید کلی</strong>
          </v-row>
          <v-row class="mx-12">
            <v-spacer />
            <v-col class="cols" cols="12" sm="6" md="4" lg="4" xl="2">
              <top-display-row
                name="item"
                :item="{ Title: 'موجودی حساب شما', Info: wallet }"
              />
            </v-col>
            <v-col class="cols" cols="12" sm="6" md="4" lg="4" xl="2">
              <top-display-row
                name="item"
                :item="{ Title: 'آگهی‌های باقی‌مانده', Info: ads }"
              />
            </v-col>
            <v-col class="cols" cols="12" sm="6" md="4" lg="4" xl="2">
              <top-display-row
                name="item"
                :item="{ Title: 'آگهی‌های ثبت شده', Info: companyAds }"
              />
            </v-col>
            <v-col class="cols" cols="12" sm="6" md="3" lg="3" xl="2">
              <TopDisplayRowCardVue
                name="item"
                :item="{ Title: 'پلن شما', Info: plan }"
              />
            </v-col>
            <v-spacer />
          </v-row>
        </div>
      </v-sheet>

      <ads-container :ads-list="adsList" />

      <v-sheet v-if="this.$vuetify.breakpoint.mdAndUp" class="pa-8">
        <v-row>
          <v-col class="v-col" cols="6">
            <dashboard-checklist />
          </v-col>
          <v-col class="v-col" cols="6">
            <v-img src="@/assets/activity.png" class="mx-16" height="250px" width="400px"></v-img>
          </v-col>
        </v-row>
      </v-sheet>
      <div v-else>
        <dashboard-checklist class="mb-3 mx-16" />
      </div>
    </v-sheet>
  </v-app>
</template>

<script>
import TopDisplayRow from "@/components/userpanel/TopDisplayRowCard";
import AdsContainer from "@/components/userpanel/AdsContainer";
import DashboardChecklist from "@/components/userpanel/DashboardChecklist";

export default {
  components: {
    AdsContainer,
    TopDisplayRow,
    DashboardChecklist,
  },
  onMounted() {
    this.$vuetify.rtl = true;
  },
  data() {
    return {
      adsList: [],
      wallet:'',
      ads:'',
      companyAds:'',
      plan:''
    };
  },
  methods: {
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
        that.companyAds = response.data.length;
        that.adList = response.data;
        console.log(that.adList);
      });
    },
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
      await axios(config)
        .then(function (response) {
          that.wallet = response.data.wallet;
          that.ads = response.data.AdsRemaining;
          that.plan = response.data.activePlan;
        })
    },
  },
  beforeMount() {
    this.adFinder();
    this.userInfo();
  },
};
</script>

<style scoped></style>
