<template>
  <v-sheet color="transparent" class="pa-4">
    <div class="pa-4">
      <v-row class="mb-5">
        <strong style="font-size: 1.5em">خرید پلن</strong>
      </v-row>
      <hr class="my-3"/>
      <v-row v-if="this.$vuetify.breakpoint.mdAndUp">
        <v-col v-for="(plan, index) in plans" :key="index" cols="3">
          <v-card width="100%" elevation="1" class="rounded-xl pa-4">
            <v-card-text>
              <v-row class="my-4" style="font-size: 1.4em; font-weight: bolder">
                <span>پلن:</span>
                <v-spacer />
                <span>{{ plan.Title }}</span>
              </v-row>
              <v-divider />
              <v-row class="my-4" style="font-size: 1.2em">
                <span>روزهای فعال:</span>
                <v-spacer />
                <span>{{ plan.Days }}</span>
              </v-row>
              <v-divider />
              <v-row class="my-4" style="font-size: 1.2em">
                <span>تعداد آگهی ها:</span>
                <v-spacer />
                <span>{{ plan.Ads }}</span>
              </v-row>
            </v-card-text>
            <v-card-actions>
              <v-row class="my-4" style="font-size: 1.2em" justify="center" align="center">
                <v-btn class="rounded-lg" outlined width="80%" color="#3751FF">
                  پرداخت (از اعتبار)
                </v-btn>
              </v-row>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
      <div v-else>
        <v-row v-for="(plan, index) in plans" :key="index" class="my-3">
          <v-card width="100%" elevation="1" class="rounded-xl pa-4">
            <v-card-text>
              <v-row>
                <v-col cols="12">
                  <v-row class="my-4" style="font-size: 1.4em; font-weight: bolder">
                    <span>پلن:</span>
                    <v-spacer />
                    <span>{{ plan.Title }}</span>
                  </v-row>
                  <v-divider />
                  <v-row class="my-4" style="font-size: 1.2em">
                    <span>روزهای فعال:</span>
                    <v-spacer />
                    <span>{{ plan.Days }}</span>
                  </v-row>
                  <v-divider />
                  <v-row class="my-4" style="font-size: 1.2em">
                    <span>تعداد آگهی ها:</span>
                    <v-spacer />
                    <span>{{ plan.Ads }}</span>
                  </v-row>
                  <v-divider />
                  <v-row class="my-4" style="font-size: 1.2em">
                    <span>قیمت:</span>
                    <v-spacer />
                    <span>{{ plan.Price }}</span>
                    &nbsp;
                    <span>تومان</span>
                  </v-row>
                  <v-row class="mt-5" style="font-size: 1.2em">
                    <v-btn class="rounded-lg pa-3" outlined width="100%" height="100%" color="#3751FF" @click="buyPlan(index)">
                      پرداخت
                      (از اعتبار)
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
      default: 0
    }
  },
  name: "PurchasePlanComponent",
  data () {
    return {
      planColor: {
        NoPlan: '#ffffff',
        Classic: '#caf0f8',
        Pro: '#ade8f4',
        Deluxe: '#90e0ef',
        Max: '#48cae4'
      },
      plans: [
        {Title: 'کلاسیک', Color: '#caf0f8', Days: 60, Ads: 10, Price: 20000},
        {Title: 'پرو', Color: '#ade8f4', Days: 60, Ads: 15, Price: 40000},
        {Title: 'دلوکس', Color: '#90e0ef', Days: 90, Ads: 30, Price: 60000},
        {Title: 'مکس', Color: '#48cae4', Days: 120, Ads: 45, Price: 80000}
      ]
    }
  }
}
</script>

<style scoped>

</style>
