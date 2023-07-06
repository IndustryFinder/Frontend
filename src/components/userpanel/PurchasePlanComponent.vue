<template>
  <v-sheet color="transparent" class="pa-4">
    <div class="pa-4">
      <v-row class="mb-5">
        <strong>خرید پلن</strong>
      </v-row>

      <v-row v-if="this.$vuetify.breakpoint.mdAndUp">
        <v-col v-for="(plan, index) in plans" :key="index" cols="3">
          <v-card width="100%" elevation="12" class="rounded-xl pa-4 plan-card">
            <v-card-text class="white--text">
              <v-row class="my-4" style="font-size: 1.4em; font-weight: bolder">
                <span class="white--text">پلن:</span>
                <v-spacer />
                <span>{{ plan.Title }}</span>
              </v-row>
              <v-spacer class="my-8"/>
              <v-row class="my-4" style="font-size: 1.2em">
                <span class="white--text">روزهای فعال:</span>
                <v-spacer />
                <span>{{ plan.Days }}</span>
              </v-row>
              <v-spacer class="my-8"/>
              <v-row class="my-4" style="font-size: 1.2em">
                <span class="white--text">تعداد آگهی ها:</span>
                <v-spacer />
                <span>{{ plan.Ads }}</span>
              </v-row>
            </v-card-text>
            <v-card-actions>
              <v-row
                class="my-4"
                style="font-size: 1.2em"
                justify="center"
                align="center"
              >
                <v-btn
                  class="rounded-lg white--text"
                  outlined
                  width="80%"
                  style="background-color: #3751ff; border: none"
                >
                  پرداخت (از اعتبار)
                </v-btn>
              </v-row>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
      <div v-else>
        <v-row v-for="(plan, index) in plans" :key="index" class="my-3">
          <v-card width="100%" elevation="12" class="rounded-xl pa-4 plan-card">
            <v-card-text class="white--text">
              <v-row>
                <v-col cols="12">
                  <v-row
                    class="my-4"
                    style="font-size: 1.4em; font-weight: bolder"
                  >
                    <span class="white--text">پلن:</span>
                    <v-spacer />
                    <span>{{ plan.Title }}</span>
                  </v-row>
                  <v-spacer class="my-8"/>
                  <v-row class="my-4" style="font-size: 1.2em">
                    <span class="white--text">روزهای فعال:</span>
                    <v-spacer />
                    <span class="white--text">{{ plan.Days }}</span>
                  </v-row>
                  <v-spacer class="my-8"/>
                  <v-row class="my-4" style="font-size: 1.2em">
                    <span>تعداد آگهی ها:</span>
                    <v-spacer />
                    <span>{{ plan.Ads }}</span>
                  </v-row>
                  <v-spacer class="my-8"/>
                  <v-row class="my-4" style="font-size: 1.2em">
                    <span class="white--text">قیمت:</span>
                    <v-spacer />
                    <span>{{ plan.Price }}</span>
                    &nbsp;
                    <span>تومان</span>
                  </v-row>
                  <v-row class="mt-5" style="font-size: 1.2em">
                    <v-btn
                      class="rounded-lg pa-3 white--text"
                      outlined
                      width="100%"
                      height="100%"
                      style="background-color: #3751ff; border: none"
                      @click="buyPlan(index)"
                    >
                      پرداخت (از اعتبار)
                    </v-btn>
                  </v-row>
                </v-col>
                <v-spacer />
              </v-row>
            </v-card-text>
          </v-card>
        </v-row>
      </div>
    </div>
  </v-sheet>
</template>

<script>
export default {
  props: {
    credit: {
      type: Number,
      default: 0,
    },
  },
  name: "PurchasePlanComponent",
  data() {
    return {
      planColor: {
        NoPlan: "#ffffff",
        Classic: "#caf0f8",
        Pro: "#ade8f4",
        Deluxe: "#90e0ef",
        Max: "#48cae4",
      },
      plans: [
        {
          Title: "کلاسیک",
          Color: "rgba(55, 81, 255, 0)",
          Days: 60,
          Ads: 10,
          Price: 20000,
        },
        {
          Title: "پرو",
          Color: "rgba(55, 81, 255, 0.1)",
          Days: 60,
          Ads: 15,
          Price: 40000,
        },
        {
          Title: "دلوکس",
          Color: "rgba(55, 81, 255, 0.2)",
          Days: 90,
          Ads: 30,
          Price: 60000,
        },
        {
          Title: "مکس",
          Color: "rgba(55, 81, 255, 0.3)",
          Days: 120,
          Ads: 45,
          Price: 80000,
        },
      ],
    };
  },
  methods:{
    async buyPlan(index){
      var axios = require("axios");
      // var FormData = require("form-data");
      // var data = new FormData();
      // data.append("description", this.message);
      var config = {
        method: "get",
        url: this.$store.state.host + "user/BuyPlan/" + (index+1),
        headers: {
          Accept: "application/json",
          Authorization: "Bearer " + this.$cookies.get("token"),
        },
        // data: data,
      };
      await axios(config).then();
    }
  }
};
</script>

<style scoped>
.plan-card {
  background-image: url("../../assets/plan-background.svg");
}
</style>
