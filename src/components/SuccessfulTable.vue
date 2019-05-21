<template>
  <div>
    <e-table :title="title" :subtitle="subtitle" :endtitle="endtitle">
      <template v-slot:rows>
        <v-expansion-panel-content v-for="row in data" :key="row.id" expand-icon="mdi-menu-down">
          <template v-slot:header>
            <v-layout row>
              <cloud-piece :title="row.title" :cstyle="'green--text'"/>

              <v-flex d-flex xs12>
                <v-layout row wrap>
                  <v-flex d-flex>
                    <v-layout row wrap>
                      <v-flex xs12 subtitle-1 text-xs-center shrink>
                        <div
                          v-if="row.displayValue != null"
                          style="text-align: right;"
                          class="green--text"
                        >{{row.displayValue}}</div>
                      </v-flex>
                    </v-layout>
                  </v-flex>
                </v-layout>
              </v-flex>
            </v-layout>
          </template>
          <v-card>
            <v-card-text class="grey lighten-3">{{row.description}}</v-card-text>
          </v-card>
        </v-expansion-panel-content>
      </template>
    </e-table>
  </div>
</template>

<script>
import ETable from "@/components/ETable.vue";
import TableMixin from "@/mixins/TableMixin.vue";
import CloudPiece from "@/components/CloudPiece.vue";

export default {
  mixins: [TableMixin],
  components: {
    ETable, CloudPiece
  }
};
</script>

<style>
</style>
