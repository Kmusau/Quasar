<template>
  <div class="q-pa-md" style="max-width: 350px">
    <q-toolbar class="bg-primary text-white shadow-2">
      <q-toolbar-title>Contacts</q-toolbar-title>
    </q-toolbar>

    <q-list bordered>
      <q-item
        v-for="contact in contacts"
        :key="contact.id"
        class="q-my-sm"
        clickable
        v-ripple
      >
        <q-item-section avatar>
          <q-avatar color="primary" text-color="white">
            {{ contact.letter }}
          </q-avatar>
        </q-item-section>

        <q-item-section>
          <q-item-label>{{ contact.name }}</q-item-label>
          <q-item-label caption lines="1">{{ contact.email }}</q-item-label>
        </q-item-section>

        <q-item-section side>
          <q-icon name="chat_bubble" color="green" />
        </q-item-section>
      </q-item>

      <!-- <div>
        {{ list }}
      </div> -->
      <q-separator />
      <q-item-label header>From API</q-item-label>

      <q-item
        v-for="unit in list"
        :key="unit.id"
        class="q-mb-sm"
        clickable
        v-ripple
      >
        <!-- <q-item-section avatar>
          <q-avatar>
            <img :src="`https://cdn.quasar.dev/img/${contact.avatar}`" />
          </q-avatar>
        </q-item-section> -->

        <q-item-section>
          <q-item-label>{{ unit.name }}</q-item-label>
          <q-item-label caption lines="1">{{ unit.CreatedAt }}</q-item-label>
        </q-item-section>

        <q-item-section side>
          <q-icon name="chat_bubble" color="grey" />
        </q-item-section>
      </q-item>
    </q-list>
  </div>
  <div class="q-pa-md">
    <q-table :data="list" row-key="ID" />
  </div>
</template>

<script>
import axios from "axios";

const contacts = [
  {
    id: 1,
    name: "Ruddy Jedrzej",
    email: "rjedrzej0@discuz.net",
    letter: "R",
  },
  {
    id: 2,
    name: "Mallorie Alessandrini",
    email: "malessandrini1@marketwatch.com",
    letter: "M",
  },
  {
    id: 3,
    name: "Elisabetta Wicklen",
    email: "ewicklen2@microsoft.com",
    letter: "E",
  },
  {
    id: 4,
    name: "Seka Fawdrey",
    email: "sfawdrey3@wired.com",
    letter: "S",
  },
];

export default {
  data() {
    return {
      contacts,
      list: undefined,
    };
  },
  mounted() {
    axios.get("http://localhost:3000/fetch/units").then((resp) => {
      this.list = resp.data;
      console.log(resp.data);
    });
  },
};
</script>
