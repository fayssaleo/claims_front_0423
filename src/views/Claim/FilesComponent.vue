<template>
  <v-col cols="4" class="d-flex">
    <span class="fileNameEstimate">
      {{ fileItem.filename }}
    </span>
    <div class="d-flex flex-column">
      <a
        class="downloadpicture bg-teal btnFile"
        :href="`${URL}/${fileItem.filename}`"
        download
        target="_blank"
      >
        <v-btn class="white--text" color="teal">
          <v-icon color="white" medium> mdi-download-circle-outline </v-icon>
        </v-btn>
      </a>

      <v-btn
        class="btnFile white--text"
        color="red"
        @click="removefileEstimate(fileItem)"
      >
        <v-icon medium> mdi-delete </v-icon>
      </v-btn>
    </div>
  </v-col>
</template>

<script>
export default {
  name: "ClaimFrontendInputs",
  props: ["fileItem", "deleteClaimFile"],
  data() {
    return {
      inputModel: {
        id: null,
        name: "",
        value: "",
      },
      URL: process.env.VUE_APP_API_URL+"/storage/cdn/claim/incident_report/",
    };
  },

  mounted() {
    //this.addToListTextFields(this.input_item);
  },

  methods: {
    initialize() {},
    removefileEstimate(item) {
      this.deleteClaimFile(item);
    },
  },
};
</script>

<style lang="scss" scoped></style>
