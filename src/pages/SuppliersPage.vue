<template>
  <q-page class="flex flex-center">
    <div>
      <q-card-section>
        <div class="q-pa-md" style="max-width: 400px">
          <q-form @submit="createSupplier" @reset="onReset" class="q-gutter-md">
            <q-input
              filled
              v-model="suppliers.name"
              label="Your name *"
              hint="Name and surname"
              lazy-rules
              :rules="[
                (val) => (val && val.length > 0) || 'Please type something',
              ]"
            />

            <q-input
              filled
              v-model="suppliers.location"
              label="Your location *"
              hint="Physical location"
              lazy-rules
              :rules="[
                (val) => (val && val.length > 0) || 'Please type something',
              ]"
            />

            <q-input
              filled
              v-model="suppliers.msisdn"
              label="Your Phone number *"
              hint="format: 2547xxxxxxxx"
              lazy-rules
              :rules="[
                (val) => (val && val.length > 0) || 'Please type something',
              ]"
            />

            <!-- <q-toggle v-model="accept" label="I accept the license and terms" /> -->

            <div>
              <q-btn label="Submit" type="submit" color="primary" />
              <q-btn
                label="Reset"
                type="reset"
                color="primary"
                flat
                class="q-ml-sm"
              />
            </div>
          </q-form>
        </div>
      </q-card-section>

      <q-card dark bordered class="bg-grey-6 my-card">
        <q-card-section>
          <div class="text-h6">Our Suppliers list</div>
          <div class="text-subtitle2">by Dr. Pharmacist</div>
        </q-card-section>

        <q-separator dark inset />

        <q-list bordered>
          <q-item
            v-for="suppliers in allsuppliers"
            :key="suppliers.ID"
            class="q-my-sm"
            clickable
            v-ripple
          >
            <q-item-section avatar>
              <q-avatar color="primary" text-color="white">
                {{ suppliers.ID }}
              </q-avatar>
            </q-item-section>

            <q-item-section @click="getSupplier(suppliers.ID)">
              <q-item-label>{{ suppliers.name }}</q-item-label>
              <q-item-label caption lines="1">{{
                suppliers.location
              }}</q-item-label>
            </q-item-section>
            <q-item-section side>
              <q-icon
                name="delete"
                color="red"
                @click="deleteSupplier(suppliers.ID)"
              />
            </q-item-section>
            <q-item-section side>
              <q-icon
                name="edit"
                color="green"
                @click="editformdialog(suppliers.ID)"
              />
            </q-item-section>
          </q-item>
        </q-list>
      </q-card>
    </div>
    <div>
      <q-dialog v-model="showstock">
        <q-card>
          <q-card-section>
            <div class="text-h6">{{ supplier.name }}</div>
          </q-card-section>

          <q-card-section class="q-pt-none">
            <div class="q-pa-md">
              <q-table :rows="supplier.Stock" :columns="columns" row-key="id" />
            </div>
          </q-card-section>

          <q-card-actions align="right">
            <q-btn flat label="OK" color="primary" v-close-popup />
          </q-card-actions>
        </q-card>
      </q-dialog>
      <q-dialog v-model="editform">
        <q-card>
          <q-card-section>
            <div class="q-pa-md" style="max-width: 400px">
              <q-form
                color="purple-12"
                @submit="updateSupplier(supplier.ID)"
                @reset="onReset"
                class="body-dark"
              >
                <q-input
                  filled
                  v-model="supplier.name"
                  label="your name"
                  lazy-rules
                  :rules="[
                    (val) => (val && val.length > 0) || 'Please type something',
                  ]"
                />

                <q-input
                  filled
                  v-model="supplier.location"
                  label="Your location *"
                  lazy-rules
                  :rules="[
                    (val) => (val && val.length > 0) || 'Please type something',
                  ]"
                />

                <q-input
                  filled
                  v-model="supplier.msisdn"
                  label="Your Phone number *"
                  lazy-rules
                  :rules="[
                    (val) => (val && val.length > 0) || 'Please type something',
                  ]"
                />

                <div>
                  <q-btn label="Submit" type="submit" color="primary" />
                  <q-btn
                    label="Cancel"
                    type="reset"
                    color="primary"
                    flat
                    class="q-ml-sm"
                    v-close-popup
                  />
                </div>
              </q-form>
            </div>
          </q-card-section>
        </q-card>
      </q-dialog>
    </div>
  </q-page>
</template>

<script>
import axios from "axios";
import { useQuasar } from "quasar";
import { ref } from "vue";

export default {
  name: "SuppliersList",
  data() {
    const $q = useQuasar();
    const name = ref(null);
    const msisdn = ref(null);
    const location = ref(null);
    // const accept = ref(false);
    return {
      allsuppliers: undefined,
      supplier: undefined,
      showstock: ref(false),
      editform: ref(false),

      suppliers: {
        name,
        msisdn,
        location,
      },
      columns: [
        {
          name: "Date supplied",
          label: "Date Supplied",
          field: "CreatedAt",
        },
        {
          name: "quantity",
          label: "Quantity",
          field: "quantity",
        },
        {
          name: "status",
          label: "Status",
          field: "status",
        },
        {
          name: "BP per Pack",
          label: "BP per Pack",
          field: "bp_per_pack",
        },
        {
          name: "Total Buying Price",
          label: "Total Buying Price",
          field: "total_buying_price",
        },
      ],

      onReset() {
        name.value = null;
        location.value = null;
        msisdn.value = null;
        // accept.value = false;
      },
    };
  },
  mounted() {
    axios.get("http://localhost:3000/fetch/suppliers").then((resp) => {
      this.allsuppliers = resp.data;
    });
  },
  methods: {
    createSupplier() {
      axios
        .post("http://localhost:3000/create/supplier", this.suppliers)
        .then((resp) => {
          console.log(resp);
        });
    },
    deleteSupplier(id) {
      axios
        .delete("http://localhost:3000/delete/supplier/" + id)
        .then((resp) => {
          console.log(resp.data);
        });
    },
    getSupplier(id) {
      axios.get("http://localhost:3000/fetch/supplier/" + id).then((resp) => {
        this.supplier = resp.data;
        this.showstock = true;
      });
    },
    updateSupplier(id) {
      axios
        .put("http://localhost:3000/update/supplier/" + id, this.supplier)
        .then((resp) => {
          console.log(resp);
        });
    },
    editformdialog(id) {
      axios.get("http://localhost:3000/fetch/supplier/" + id).then((resp) => {
        this.supplier = resp.data;
        this.editform = true;
      });
    },
  },
};
</script>
