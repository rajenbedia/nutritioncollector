<template>
  <h1>HOME PAGE</h1>
  <button @click="showBarcodeScanner">Scan</button>
  <StreamBarcodeReader
    v-if="showScanner"
    @decode="searchFood"
  ></StreamBarcodeReader>
  {{ test }}
</template>

<script>
import axios from "axios";
import { StreamBarcodeReader } from "vue-barcode-reader";

export default {
  name: "HomePage",
  components: {
    StreamBarcodeReader,
  },
  data() {
    return {
      showScanner: false,
      test: "test",
    };
  },
  methods: {
    showBarcodeScanner() {
      this.showScanner = true;
    },
    async searchFood(barcodeNumber) {
      const headers = {
        "User-Agent":
          "nutritioncollector" -
          "PC" -
          "Version 1.0" -
          "http://localhost:5173/",
      };

      const url = `https://world.openfoodfacts.org/api/v2/product/${barcodeNumber}`;

      const response = await axios.get(url, { headers });
      const data = response.data.product;
      console.log("Carbs:" + data.nutriments.carbohydrates);
      console.log("Fats:" + data.nutriments.fat);
      console.log("Fibre:" + data.nutriments.fiber);
      console.log("Proteins:" + data.nutriments.proteins);
      console.log("Kcals: " + data.nutriments["energy-kcal"]);

      console.log("Total Quantity:" + data.quantity);

      this.test = "Carbs:" + data.nutriments.carbohydrates;
      this.showScanner = false;
    },
  },
};
</script>
