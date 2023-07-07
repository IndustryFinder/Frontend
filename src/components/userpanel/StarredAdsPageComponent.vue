<template>
  <v-app>
    <v-sheet fill-height color="transparent" class="pa-10">
      <v-row class="mb-5" align="center">
        <strong>نشانک‌های من</strong>
      </v-row>
      <v-row
      justify="center"
      align="center"
      class="mx-2"
      :dense="this.$vuetify.breakpoint.smAndDown"
      >
        <v-col
          v-for="(ad, index) in adList"
          :key="index"
          cols="12"
          md="6"
          lg="4"
          xl="3"
        >
          <starred-ad-card :ad="item"  @togglestar="toggleStarred(index)"/>
        </v-col>
      </v-row>
    </v-sheet>
  </v-app>
</template>

<script>
import StarredAdCard from "@/components/userpanel/StarredAdCard.vue";
export default {
  components: {StarredAdCard},
  data () {
    return {
      adList:[],
      marked_id_list: [],
      pic:"",
      title: "",
      description: "",
      category: "",
    }
  },
  methods: {
    toggleStarred(index) {
      if (this.deleteBookmark(this.adList[index].id)) {
        this.adList.splice(index, 1);
      }
    },
    // async get_company (id = this.$route.params.id) {
    //   var axios = require('axios');
    //   var config = {
    //     method: 'get',
    //     url: this.$store.state.host + 'company/show/' + id,
    //     headers: {
    //       'Accept': 'application/json',
    //     },
    //   };
    //   let that = this;
    //   try {
    //     const response = await axios(config);
    //     that.adList.push(response.data);
    //   } catch (error) {
    //     console.log(error);
    //   }
    // },
    async showBookmarks() {
      var axios = require('axios');
      var config = {
        method: 'get',
        url: this.$store.state.host + 'user/bookmarks',
        headers: {
          'Authorization': 'Bearer '+this.$cookies.get('token'),
          'Accept': 'application/json',
        },
      };
      let that = this;
      await axios(config).then(function (response) {
        that.adList = response.data;
      });
      // await axios(config)
      //     .then(function (response) {
      //       that.marked_id_list = [];
      //       for (let i = 0; i < response.data.length; i++) {
      //         that.marked_id_list.push(response.data[i].marked_id);
      //       }
      //     })

      // for (let i = 0; i < this.marked_id_list.length; i++) {
      //   that.get_company(that.marked_id_list[i]);
      // }
    },
    async deleteBookmark(id) {
      const axios = require("axios");

      let config = {
        method: "delete",
        url: this.$store.state.host + "user/bookmarks/del/" + id,
        headers: {
          Authorization: "Bearer " + this.$cookies.get('token'),
        },
      };
      axios
        .request(config)
        .then(() => {
          return true;
        })
        .catch(() => {
          return false;
        });
    },
  },
  computed: {
    starredAds () {
      return this.adList.filter((x) => x.isStarred)
    }
  },
  created(){
    this.showBookmarks();
  }
}
</script>

<style scoped>

</style>
