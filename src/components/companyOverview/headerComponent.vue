<template>
  <v-card elevation="2" class="main px-8 pb-5">
    <v-card class="img">
      <v-img elevation="5">
        <v-icon size="6rem">mdi-factory</v-icon>
      </v-img>
    </v-card>
    <div>
      <v-row>
        <v-spacer />
        <v-col cols="12" sm="6" md="6" lg="3" xl="3" align="center">
          <div class="icon">
<!--            <v-btn elevation="1" v-on:click="save" v-bind:class="saveClass">-->
<!--              <v-icon>{{ saveIcon }}</v-icon>-->
<!--            </v-btn>-->
            <v-btn icon @click="save">
              <v-icon size="64px" :color="saved ? 'black' : ''">mdi-bookmark</v-icon>
            </v-btn>
          </div>
        </v-col>
        <v-col cols="12" sm="6" md="6" lg="3" xl="3" align="center">
          <h1>
            {{ companyName }}
          </h1>
        </v-col>
        <v-col cols="12" sm="6" md="6" lg="3" xl="3" align="center">
          <v-rating
              :empty-icon="ImdiStarOutline"
              :full-icon="ImdiStar"
              :half-icon="ImdiStarHalf"
              hover
              length="5"
              readonly
              value="3"
              color="yellow"
              background-color="gray"
              style="direction: ltr;"
          ></v-rating>
        </v-col>
        <v-col cols="12" sm="6" md="6" lg="3" xl="3" align="center">
          <v-chip color="#ffeb3b">
            {{ companyCategory }}
          </v-chip>
        </v-col>
<!--        <v-col cols="3">-->
<!--          <v-icon small>-->
<!--            {{ ImdiPin }}-->
<!--          </v-icon>-->
<!--          {{ companyLocation }}-->
<!--        </v-col>-->
      </v-row>
    </div>
  </v-card>
</template>

<script>
import { mdiMapMarker, mdiStar, mdiStarHalfFull, mdiStarOutline } from '@mdi/js'
import FormData from "form-data";
import axios from "axios";
export default {
  props: [
      'companyName',
      'companyCategory',
      'companyLocation',
  ],
  data () {
    return {
      // companyName: 'کاله',
      // companyCategory: 'دسته بندی',
      // companyLocation: 'تهران',
      ImdiPin: mdiMapMarker,
      ImdiStar: mdiStar,
      ImdiStarHalf: mdiStarHalfFull,
      ImdiStarOutline: mdiStarOutline,
      saved: false
    }
  },

  methods: {
    async save () {
      if (this.saved == true)
      {
        this.saved = false;
        let data = new FormData();
        data.append('id', this.id);

        var config = {
          method: 'post',
          url: this.$store.state.host + 'c/:id',
          headers: {
            'Accept': 'application/json',
          },
          data : data
        };
        // var errorToaster = (msg) => {
        //   this.$toast.open({
        //     message: msg,
        //     type: 'error',
        //   });
        // };
        let that = this;
        await axios(config)
            .then(function (response) {
              var result=response.data;
              that.$cookies.set('token', result.token);
              that.$cookies.set('user', result.user);
              console.log(result);
            })
            .catch(function (error) {
              console.log(error);
            });
      }
      else {
        this.saved = true;
      }

    },
  },
}
</script>

<style scoped>
.main{
  width: 100%;
  margin-top: 3rem;
  padding-top: 5rem;
}
.img{
  position: absolute;
  right: 3%;
  top: -3rem;
  width: 100px;
}
</style>
