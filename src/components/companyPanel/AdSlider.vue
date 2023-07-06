<template>
  <v-sheet class="mx-auto" elevation="8" max-width="800">
    <v-slide-group
      v-model="model"
      class="pa-4"
      selected-class="bg-success"
      show-arrows
    >
      <v-slide-group-item
        v-for="(ad, index) in adList"
        :key="index"
        v-slot="{ isSelected, toggle, selectedClass }"
      >
      <CompanyAdCardVue :ad="ad" />
        <v-card
          color="grey-lighten-1"
          :class="['ma-4', selectedClass]"
          height="200"
          width="100"
          @click="toggle"
        >
          <div class="d-flex fill-height align-center justify-center">
            <v-scale-transition>
              <v-icon
                v-if="isSelected"
                color="white"
                size="48"
                icon="mdi-close-circle-outline"
              ></v-icon>
            </v-scale-transition>
          </div>
        </v-card>
      </v-slide-group-item>
    </v-slide-group>
  </v-sheet>
</template>

<script>
export default {
  data: () => ({
    model: null,
    adList: [],
  }),
  methods: {
    async showAds() {
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
        that.adList = response.data;
      });
    },
  },
  created() {
    this.showAds();
  },
};
</script>
