<template>
  <div id="app">
    <v-app id="inspire">
      <v-layout row justify-center>
        <v-dialog v-model="isLoading" persistent fullscreen content-class="loading-dialog">
          <v-container fill-height>
            <v-layout row justify-center align-center>
              <v-progress-circular indeterminate :size="70" :width="7" color="green"></v-progress-circular>
            </v-layout>
          </v-container>
        </v-dialog>
      </v-layout>

      <div v-for="table in tables" v-bind:key="table.id">
        <component :is="table.type" v-bind="table"/>
      </div>

      <v-snackbar
        v-model="isSnackbar"
        :bottom="true"
        :multi-line="true"
        :right="true"
        :timeout="100000"
        :color="'error'"
      >
        {{ errorBuffer }}
        <v-btn flat @click="isSnackbar = false">Close</v-btn>
      </v-snackbar>
    </v-app>
  </div>
</template>

<script>
import OpportunityTable from "@/components/OpportunityTable.vue";
import DiagnosticTable from "@/components/DiagnosticTable.vue";
import SuccessfulTable from "@/components/SuccessfulTable.vue";

const utils = require("@/utils.js");
export default {
  name: "app",
  components: { OpportunityTable, DiagnosticTable, SuccessfulTable },
  data: () => {
    return {
      isLoading: false,
      isSnackbar: false,
      errorBuffer: "",
      tables: null
    };
  },
  beforeMount: function() {
    this.load("https://habr.com/");
  },
  methods: {
    load: function(url) {
      let self = this;
      self.isLoading = true;

      this.axios({
        method: "get",
        url: "https://www.googleapis.com/pagespeedonline/v5/runPagespeed",
        params: {
          url: url
        }
      })
        .then(r => {
          let welded = this._.merge(
            r.data.lighthouseResult.audits,
            r.data.lighthouseResult.categories.performance.auditRefs.reduce(
              (result, item) => {
                result[item.id] = item;
                return result;
              },
              {}
            )
          );

          self.processData(Object.values(welded));
        })
        .catch(e => {
          self.errorBuffer = e.toString();
          self.isSnackbar = true;
        })
        .then(() => {
          self.isLoading = false;
        });
    },
    processData: function(auditRefs) {
      this.tables = [
        {
          type: "OpportunityTable",
          title: "Opportunities",
          endtitle: "Approximate savings",
          data: auditRefs
            .filter(
              audit =>
                audit.group === "load-opportunities" &&
                !utils.showAsPassed(audit)
            )
            .sort(
              (auditA, auditB) =>
                utils.getWastedMs(auditB) - utils.getWastedMs(auditA)
            )
        },
        {
          type: "DiagnosticTable",
          title: "Diagnostics",
          subtitle: "- Details of the performance of your application",
          data: auditRefs
            .filter(
              audit =>
                audit.group === "diagnostics" && !utils.showAsPassed(audit)
            )
            .sort((a, b) => {
              const scoreA =
                a.scoreDisplayMode === "informative" ? 100 : Number(a.score);
              const scoreB =
                b.scoreDisplayMode === "informative" ? 100 : Number(b.score);
              return scoreA - scoreB;
            })
        },
        {
          type: "SuccessfulTable",
          title: "Successful Audits",
          data: auditRefs.filter(
            audit =>
              (audit.group === "load-opportunities" ||
                audit.group === "diagnostics") &&
              utils.showAsPassed(audit)
          )
        }
      ];
    }
  }
};
</script>

<style>
.loading-dialog {
  background-color: #f5f6fa;
}
</style>
