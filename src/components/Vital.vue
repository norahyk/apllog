<template>
  <v-card class="mx-auto" max-width="1000" outlined>
    <v-simple-table>
      <tbody>
        <tr v-for="e in vital" :key="e.name">
          <td>{{ e.name }}</td>
          <td>{{ e.value }}</td>
          <td>
            <v-tooltip right>
              <template #activator="{ on, attrs }">
                <v-icon v-if="e.diff > 0" color="green" v-bind="attrs" v-on="on"
                  >mdi-arrow-top-right-thick</v-icon
                >
                <v-icon
                  v-else-if="e.diff < 0"
                  color="blue"
                  v-bind="attrs"
                  v-on="on"
                  >mdi-arrow-bottom-right-thick</v-icon
                >
                <v-icon v-else color="gray" v-bind="attrs" v-on="on"
                  >mdi-arrow-right-thick</v-icon
                >
              </template>
              <span>{{ e.diff }}</span>
            </v-tooltip>
          </td>
        </tr>
      </tbody>
    </v-simple-table>
  </v-card>
</template>

<script>
export default {
  name: "Vital",
  props: {
    latestTest: {
      type: Object,
      require: true,
    },
    previousTest: {
      type: Object,
      require: true,
    },
  },
  computed: {
    vital() {
      return [
        {
          name: "好中球/白血球",
          value: this.latestTest.neutrophil + "/" + this.latestTest.wbc,
          diff: this.round2(
            this.latestTest.neutrophil - this.previousTest.neutrophil
          ),
        },
        {
          name: "網状赤血球",
          value: this.latestTest.retic,
          diff: this.round2(this.latestTest.retic - this.previousTest.retic),
        },
        {
          name: "血小板",
          value: this.latestTest.plt,
          diff: this.round2(this.latestTest.plt - this.previousTest.plt),
        },
        {
          name: "ヘモグロビン",
          value: this.latestTest.hb,
          diff: this.latestTest.hb - this.previousTest.hb,
        },
        {
          name: "CRP",
          value: this.latestTest.crp,
          diff: this.round2(this.latestTest.crp - this.previousTest.crp),
        },
      ];
    },
  },
  methods: {
    round2(d) {
      return Math.round(d * 100) / 100;
    },
  },
};
</script>
