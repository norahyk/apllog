<template>
  <div>
    <v-app-bar dense dark>
      <v-toolbar-title>Page title</v-toolbar-title>

      <v-spacer></v-spacer>

      <v-tabs v-model="tab" align-with-title grow>
        <v-tab>Tab 1</v-tab>
        <v-tab>Tab 2</v-tab>
        <v-tab>治療履歴</v-tab>
      </v-tabs>
    </v-app-bar>
      <v-tabs-items v-model="tab">
      <v-tab-item></v-tab-item>
      </v-tabs-item>
  </div>
</template>
