<template>
  <v-app>
    <v-sheet>
      <v-sheet color="transparent" class="pa-4">
        <div class="pa-4">
          <v-row class="mb-5">
            <strong>دید کلی</strong>
          </v-row>

          <v-row>
            <v-spacer />
            <v-col class="cols" cols="12" sm="6" md="3" lg="3" xl="2">
              <TopDisplayRowCardVue
                name="item"
                :item="{ Title: 'بازدید از آگهی های شما', Info: totalVisit }"
              />
            </v-col>
            <v-col class="cols" cols="12" sm="6" md="3" lg="3" xl="2">
              <TopDisplayRowCardVue
                name="item"
                :item="{ Title: 'آگهی‌های باقی‌مانده', Info: ads }"
              />
            </v-col>
            <v-col class="cols" cols="12" sm="6" md="3" lg="3" xl="2">
              <TopDisplayRowCardVue
                name="item"
                :item="{ Title: 'آگهی های مرتبط', Info: '4' }"
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
        <v-row>
          <AdsContainerVue :adsList="adsList" />
        </v-row>
      </v-sheet>

      <v-sheet v-if="this.$vuetify.breakpoint.mdAndUp" class="pa-8">
        <v-row>
          <v-col class="v-col" cols="6">
            <DashboardChecklistVue class="mx-8" />
          </v-col>
          <v-col class="v-col" cols="6">
            <UnansweredTicketsVue class="mx-8" />
          </v-col>
        </v-row>
      </v-sheet>
      <div v-else>
        <DashboardChecklistVue class="mb-3" />
        <UnansweredTicketsVue />
      </div>
    </v-sheet>
  </v-app>
</template>

<script>
import TopDisplayRowCardVue from "./TopDisplayRowCard.vue";
import AdsContainerVue from "./AdsContainer.vue";
import DashboardChecklistVue from "./DashboardChecklist.vue";
import UnansweredTicketsVue from "./UnansweredTickets.vue";

export default {
  components: {
    UnansweredTicketsVue,
    AdsContainerVue,
    TopDisplayRowCardVue,
    DashboardChecklistVue,
  },
  onMounted() {
    this.$vuetify.rtl = true;
  },
  data() {
    return {
      adsList: [],
      totalVisit:'',
      ads:'',
      related:'',
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
        //console.log(response.data);
        that.adList = response.data;
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
          that.$cookies.set("user", response.data);
          this.totalVisit = response.company.ViewCount;
          this.ads = response.AdsRemaining;
          //fix this later
          this.related = 0;
          this.plan = response.activePlan;
        })
        .catch(() => {
          that.$cookies.remove("user");
          that.$cookies.remove("token");
        });
    },
  },
  beforeMount() {
    this.adFinder();
    this.userInfo();
  },
};
</script>

<style scoped></style>
