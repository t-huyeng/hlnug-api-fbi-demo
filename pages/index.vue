<template>
  <v-card class="mx-auto mb-4" max-width="700" outlined>
    <v-list-item three-line>
      <v-list-item-content>
        <div class="text-overline mb-4">HLNUG</div>
        <v-list-item-subtitle class="text-h5 mb-1">
          Messdatenportal
        </v-list-item-subtitle>
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
        href="https://www.hlnug.de/messwerte/datenportal"
        target="_blank"
      >
        Homepage
      </v-btn>
      <v-btn outlined rounded to="openapi"> OpenAPI - Spezifikation </v-btn>
      <v-spacer />
    </v-card-actions>
    <v-card-text>
      <v-row align="center" justify="center">
        <v-col md="8" sm="10" xs="12">
          <v-autocomplete
            return-object
            v-model="selectedTheme"
            :items="themes"
            :item-text="(item) => item.text + ' (' + item.id + ')'"
            @change="updateTheme()"
            menu-props="closeOnContentClick"
            clearable
            chips
            label="Themestationen"
          ></v-autocomplete>
        </v-col>
      </v-row>

      <v-row align="center" justify="center">
        <v-col md="8" sm="10" xs="12">
          <v-autocomplete
            return-object
            v-model="selectedStation"
            :items="stations"
            :item-text="
              (item) => item.displayName + ' (' + item.stationId + ')'
            "
            @change="update()"
            menu-props="closeOnContentClick"
            clearable
            chips
            label="Radioaktivität Stationen"
          ></v-autocomplete>
        </v-col>
      </v-row>
      <v-card v-if="selectedStation">
        <v-card-title>
          {{ selectedStation.displayName }}
        </v-card-title>
        <v-row>
          <v-col md="6" sm="6" xs="12">
            <v-list dense>
              <v-subheader>Informationen</v-subheader>
              <v-list-item-group color="primary">
                <v-list-item>
                  <v-list-item-content>
                    <v-list-item-title> ID </v-list-item-title>
                    <v-list-item-subtitle
                      v-text="selectedStation.stationId"
                    ></v-list-item-subtitle>
                  </v-list-item-content>
                </v-list-item>
                <v-list-item>
                  <v-list-item-content>
                    <v-list-item-title> Latitude </v-list-item-title>
                    <v-list-item-subtitle
                      v-text="selectedStation.lat"
                    ></v-list-item-subtitle>
                  </v-list-item-content>
                </v-list-item>

                <v-list-item>
                  <v-list-item-content>
                    <v-list-item-title> Longitude </v-list-item-title>
                    <v-list-item-subtitle
                      v-text="selectedStation.lon"
                    ></v-list-item-subtitle>
                  </v-list-item-content>
                </v-list-item>
                <v-list-item>
                  <v-list-item-content>
                    <v-list-item-title> Standardparameter </v-list-item-title>
                    <v-list-item-subtitle
                      v-text="selectedStation.standardparameter"
                    ></v-list-item-subtitle>
                  </v-list-item-content>
                </v-list-item>
              </v-list-item-group>
            </v-list>
          </v-col>
          <v-col md="6" sm="6" xs="12">
            <template v-if="!loading">
              <v-card class="ma-2" v-if="stationsDaten.length > 0">
                <template v-for="(daten, id) in stationsDaten">
                  <Parameter :key="id" :data="daten" />
                </template>
              </v-card>
              <template v-else> Keine Daten gefunden. </template>
            </template>
            <template v-else>
              <v-progress-linear
                indeterminate
                color="green"
              ></v-progress-linear>
            </template>
          </v-col>
        </v-row>
      </v-card>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  data: () => ({
    url: "",
    radioStations: [],
    stations: [],
    selectedStation: null,
    stationsDaten: null,
    themeNumber: 8,
    theme: "radio",
    themes: null,
    selectedTheme: null,
    loading: false,
  }),

  methods: {
    async updateTheme() {
      if (this.selectedTheme) {
        const data = await this.$axios.$get(
          "https://app.hlnug.de/json/" +
            this.selectedTheme.type +
            "/getThemeStations/" +
            this.selectedTheme.id
        );
        this.stations = data;
        if (this.selectedTheme.type === "wasser") {
          this.selectedTheme.type = "diskw";
        }
      }
    },
    async update() {
      this.stationsDaten = [];
      if (this.selectedStation) {
        this.loading = true;
        const data = await this.$axios.$get(
          "https://app.hlnug.de/json/" +
            this.selectedTheme.type +
            "/getStationLatestData/" +
            this.selectedStation.stationId +
            "/" +
            this.selectedStation.standardparameter
        );
        this.loading = false;
        console.log(this.loading);
        this.stationsDaten = data;
      }
    },
  },
  async fetch() {
    const data = await this.$axios.$get("https://app.hlnug.de/json/");
    this.themes = [];
    for (const entry of data) {
      if (entry["themes"]) {
        for (const theme of entry["themes"]) {
          this.themes.push(theme);
        }
      }
    }
  },
};
</script>

<style>
</style>