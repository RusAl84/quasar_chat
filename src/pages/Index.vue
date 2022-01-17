<template>
  <q-page class="flex flex-center">
    <q-card flat bordered class="full-width">
      <q-card-section>
        <q-chat-message
          v-for="(message, i) in messages"
          :key="i"
          :name="message.UserName"
          :text="[message.MessageText]"
          :sent="message.UserName == userName"
          avatar="https://cdn.quasar.dev/img/avatar1.jpg"
        />
      </q-card-section>
    </q-card>
    <!-- Панель действий -->
    <q-card class="full-width">
      <q-card-section>
        <q-input
          style="width: 40%; min-width: 200px"
          outlined
          dense
          v-model="userName"
          label="UserName"
        />
      </q-card-section>
      <q-card-section class="flex">
        <q-input
          class="mr-10"
          style="flex-grow: 1"
          outlined
          dense
          v-model="messageText"
          label="MessageText"
        />
        <q-btn
          depressed
          color="primary"
          @click="onSendClick"
          :disabled="!messageText.length"
        >
          <q-icon :name="icons.mdiSend" />
        </q-btn>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
import { у, ref, reactive, defineComponent } from "vue";
import * as icons from "@quasar/extras/mdi-v6";
import API from "../api/api.js";

export default defineComponent({
  name: "PageIndex",

  data() {
    return {
      icons,
      // messages: [
      //   {
      //     UserName: "RusAl",
      //     MessageText: "Privet na sto let!!!",
      //     TimeStamp: "12:40",
      //   },
      //   {
      //     UserName: "RusAl",
      //     MessageText: "Privet na sto let!!!",
      //     TimeStamp: "12:40",
      //   },
      //   {
      //     UserName: "RusAl",
      //     MessageText: "Privet na sto let!!!",
      //     TimeStamp: "12:40",
      //   },
      //   {
      //     UserName: "RusAl",
      //     MessageText: "Privet na sto let!!!",
      //     TimeStamp: "12:40",
      //   },
      //   {
      //     UserName: "RusAl",
      //     MessageText: "Privet na sto let!!!",
      //     TimeStamp: "12:40",
      //   },
      // ],
      messages: [],
      userName: "",
      messageText: "",
      intervalCtx: null,
      lastMsgID: 0,
    };
  },
  // Хук который сработает когда страница создасться
  mounted() {
    this.userName = "Login";
    this.intervalCtx = setInterval(async () => {
      try {
        const msg = await API.getMessage(this.lastMsgID);
        if (typeof msg === typeof {}) {
          this.messages.push(msg);
          this.lastMsgID++;
        }
      } catch (e) {}
    }, 1000);
  },
  unmounted() {
    clearInterval(this.intervalCtx);
  },
  methods: {
    // Реакция на кнопку отправки
    async onSendClick() {
      try {
        await API.sendMessage(this.userName, this.messageText);
        console.log("cleared");
        this.message = "";
      } catch (e) {
        console.error(e);
      }
    },
  },

});
</script>
