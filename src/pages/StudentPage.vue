<template>
  <div class="q-pa-md">
    <q-table
    :rows="stock"
    :columns="columns"
    row-key="id"
    />
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      stock: [],
      columns: [
        {
          name: "quantity",
          label: "Quantity",
          field: "quantity"
        },
        {
          name: "status",
          label: "Status",
          field: "status"
        },
        {
          name: "BP per Pack",
          label: "BP per Pack",
          field: "bp_per_pack"
        }
      ]
    };
  },
  mounted() {
    axios.get("http://localhost:3000/fetch/stock").then((resp) => {
      this.stock = resp.data;
    });
  },
};
</script>
