<template>
  <v-app>
    <v-sheet fill-height color="transparent" class="pa-10">
      <v-row class="mb-5" align="center">
        <strong>آگهی های من</strong>
      </v-row>
      <v-row justify="center" align="center" :dense="this.$vuetify.breakpoint.smAndDown">
        <v-col
            v-for="(ad, index) in adList"
            :key="index"
            cols="12"
            md="6"
            lg="4"
            xl="3">
          <ad-card :ad="ad" />
        </v-col>
      </v-row>
    </v-sheet>
  </v-app>
</template>

<script>
import AdCard from "@/components/userpanel/AdCard";
export default {
  components: {AdCard},
  data () {
    return {
      pic:"",
      name: "",
      description: "",
      category: "",
      adList:[],
    }
  },
  methods:{
    async showAds(){
      var axios = require('axios');
      var config = {
        method: 'get',
        url: this.$store.state.host + 'ad/searchBySender',
        headers: {
          'Accept': 'application/json',
          'Authorization': 'Bearer '+this.$cookies.get('token'),
        },
      };
      let that = this;
      await axios(config)
        .then(
          function (response) {
            // console.log(response.data);
            that.adList = response.data;
          }
        )
    }
  },
  created(){
    this.showAds();
  }
}
</script>

