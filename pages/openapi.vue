<template>
  <div>
    <v-card class="mx-auto mb-4" max-width="700" outlined>
      <v-list-item three-line>
        <v-list-item-content>
          <div class="text-overline mb-4">HLNUG</div>
          <v-list-item-title class="text-h5 mb-1">
            Messdatenportal
          </v-list-item-title>
          <v-list-item-subtitle
            >Hessisches Landesamt für Naturschutz, Umwelt und Geologie
          </v-list-item-subtitle>
        </v-list-item-content>

        <v-list-item-avatar tile size="80"> </v-list-item-avatar>
      </v-list-item>

      <v-card-actions>
        <v-btn
          outlined
          rounded
          text
          href="https://www.hlnug.de/messwerte/datenportal"
          target="_blank"
        >
          Homepage
        </v-btn>
        <v-btn
          outlined
          rounded
          text
          href="https://github.com/t-huyeng/hlnug-api"
          target="_blank"
        >
          <v-icon>mdi-github</v-icon>
        </v-btn>
        <v-spacer />
      </v-card-actions>
    </v-card>
    <Swagger-Ui v-if="show" :url="url"></Swagger-Ui>
  </div>
</template>

<script>
export default {
  data: () => ({
    url: "",
    show: false,
  }),

  methods: {},
  async fetch() {
    this.url =
      "https://raw.githubusercontent.com/t-huyeng/hlnug-api/main/openapi.yaml";
    this.show = true;
  },
};
</script>

<style>
</style>