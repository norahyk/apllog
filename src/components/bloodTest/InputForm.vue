<template>
  <v-form>
    <v-container>
      <v-row>
        <v-col v-for="(field, name) in doc" v-bind:key="name" cols="8" sm="1"
          >{{ name }}
          <v-text-field label="name" filled></v-text-field>
        </v-col>
      </v-row>
    </v-container>
  </v-form>
</template>

<script>
export default {
  name: "inputForm",
  data: function() {
    return {
      doc: {
        tested: new Date(),
        wbc: "",
        seg_band: "",
        rbc: "",
        hb: "",
        retic: "",
        plt: "",
        crp: "",
      },
    };
  },
};
</script>
