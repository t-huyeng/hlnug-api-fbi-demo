<template>
  <v-list v-if="data" dense>
    <v-subheader v-html="data.paramname"></v-subheader>
    <v-list-item>
      <v-list-item-content>
        <v-list-item-title> Value </v-list-item-title>
        <v-list-item-subtitle v-text="data.value"></v-list-item-subtitle>
      </v-list-item-content>
    </v-list-item>
    <v-list-item>
      <v-list-item-content>
        <v-list-item-title> Unit </v-list-item-title>
        <v-list-item-subtitle v-html="data.unit"></v-list-item-subtitle>
      </v-list-item-content>
    </v-list-item>

    <v-list-item>
      <v-list-item-content>
        <v-list-item-title> Timestamp </v-list-item-title>
        <v-list-item-subtitle>{{ d }}</v-list-item-subtitle>
      </v-list-item-content>
    </v-list-item>
  </v-list>
</template>

<script>
export default {
  props: {
    data: null,
  },

  data: () => ({
    d: "",
  }),

  fetch() {
    this.d = new Date(0);
    this.d.setUTCSeconds(this.data.tstamp).toLocaleString();
  },
};
</script>

<style>
</style>