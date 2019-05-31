<template>
  <div class="ion-page">
    <ion-header>
      <ion-toolbar>
        <ion-title>ZipInfo</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content class="ion-padding">
      <zip-search
        @get-zip="getZipInfo"
      />
      <zip-info
        :info="info"
      />
      <clear-info
        :info="info"
        @clear-info="clearInfo"
      ></clear-info>
    </ion-content>
  </div>
</template>

<script>
import ZipSearch from "../components/ZipSearch";
import ZipInfo from "../components/ZipInfo";
import ClearInfo from "../components/ClearInfo";

export default {
  name: 'home',
  components: {
    ZipSearch,
    ZipInfo,
    ClearInfo
  },
  data() {
    return {
      info: null
    }
  },
  methods: {
    async getZipInfo(zip) {
      const res = await fetch(`http://api.zippopotam.us/us/${zip}`);

      if (res.status == 404) {
        this.showAlert();
      }

      this.info = await res.json();
    },
    clearInfo() {
      this.info = null;
    },
    showAlert() {
      return this.$ionic.alertController
              .create({
                header: "Not Valid",
                message: "Please enter a valid US zipcode",
                buttons: ["OK"]
              })
              .then(a => a.present());
    }
  }
}
</script>
