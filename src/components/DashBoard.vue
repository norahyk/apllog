<template>
  <v-main>
    <v-container fluid>
      <v-row dense>
        <v-col>
          <v-card class="mx-auto" max-width="1000" outlined>
            <LineChart
              v-if="chartdata"
              :chart-data="chartdata"
              :options="options"
            />
          </v-card>
        </v-col>
      </v-row>
      <v-row>
        <v-col>
          <Vital
            v-if="latestTest"
            :latestTest="latestTest"
            :previousTest="previosTest"
        /></v-col>
      </v-row>
    </v-container>
  </v-main>
</template>

<script>
import { db } from "../plugins/firebase";

import LineChart from "./LineChart.vue";
import Vital from "./Vital.vue";

export default {
  name: "DashBoard",
  components: { LineChart, Vital },
  data: () => {
    return {
      tests: [],
      options: {
        responsive: true,
        maintainAspectRatio: false,
        scales: {
          xAxes: [
            {
              type: "time",
              distribution: "linear",
              time: {
                unit: "day",
              },
              scaleLabel: {
                display: true,
                labelString: "Date",
              },
            },
          ],
        },
      },
    };
  },
  firestore() {
    return {
      tests: db.collection("blood_test").orderBy("tested"),
    };
  },
  computed: {
    chartdata() {
      if (this.tests.length === 0) {
        return null;
      }
      var chartdata = {
        datasets: [
          {
            label: "白血球数",
            data: this.tests.map((e) => {
              return { t: e.tested.toDate(), y: e.wbc };
            }),
          },
          {
            label: "好中球数",
            backgroundColor: "#00002222",
            data: this.tests.map((e) => {
              return { t: e.tested.toDate(), y: e.wbc * e.seg_band };
            }),
          },
          {
            label: "網状赤血球",
            borderDash: [5, 5],
            fill: false,
            borderColor: "#ff000055",
            data: this.tests.map((e) => {
              return { t: e.tested.toDate(), y: e.retic };
            }),
          },
          {
            label: "血小板",
            borderDash: [5, 5],
            fill: false,
            borderColor: "#99990055",
            data: this.tests.map((e) => {
              return { t: e.tested.toDate(), y: e.plt };
            }),
          },
        ],
      };
      return chartdata;
    },
    latestTest() {
      return this.getLastNTest(1);
    },
    previosTest() {
      return this.getLastNTest(2);
    },
  },
  methods: {
    getLastNTest(n) {
      var test = null;
      if (this.tests.length > n) {
        test = this.tests[this.tests.length - n];
        return test;
      }
    },
  },
};
</script>
