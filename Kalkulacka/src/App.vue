<template>
  <v-app>
    <v-main class="cyan lighten-4">
      <v-container fill-height>
        <Calculator />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import Calculator from "./components/Calculator"

export default {
  name: "App",
  components: {
    Calculator,
  },

  data: () => ({
    //
  }),
};
</script>
