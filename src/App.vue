<template>
  <v-app id="app">
    <v-app-bar dense dark app>
      <v-tabs v-model="tab" grow>
        <v-tab href="#tab-1">ステータス</v-tab>
        <v-tab href="#tab-2">血液検査履歴</v-tab>
        <v-tab href="#tab-3">治療履歴</v-tab>
      </v-tabs>
    </v-app-bar>
    <v-tabs-items v-model="tab">
      <v-tab-item value="tab-1">
        <DashBoard />
      </v-tab-item>
      <v-tab-item value="tab-2">
        <TheBloodTestLogTable />
      </v-tab-item>
      <v-tab-item value="tab-3">
        <v-main>
          <TheTreatmentLogTable />
        </v-main>
      </v-tab-item>
    </v-tabs-items>
  </v-app>
</template>

<script>
import DashBoard from "./components/DashBoard.vue";
import TheBloodTestLogTable from "./components/TheBloodTestLogTable.vue";
import TheTreatmentLogTable from "./components/TheTreatmentLogTable.vue";

export default {
  name: "app",
  components: {
    DashBoard,
    TheBloodTestLogTable,
    TheTreatmentLogTable,
  },
  data: () => {
    return {
      tab: null,
    };
  },
};
</script>
