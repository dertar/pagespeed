<template>
  <v-layout row>
    <v-flex xs12 sm8 offset-sm2>
      <v-card>
        <v-toolbar>
          <v-toolbar-title>{{title}}</v-toolbar-title>
          <v-toolbar-title v-if="subtitle != null" class="body-2 grey--text">{{subtitle}}</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-toolbar-title v-if="endtitle != null">{{endtitle}}</v-toolbar-title>
        </v-toolbar>
        <v-expansion-panel>
          <slot name="rows"></slot>
        </v-expansion-panel>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import TableMixin from "@/mixins/TableMixin.vue";
export default {
  mixins: [TableMixin]
};
</script>

<style>
</style>
