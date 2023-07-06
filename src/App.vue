<script setup>
import { onMounted, reactive } from "vue";
import { useNativeNotifications } from "vue3-native-notification";

const nativeNotification = useNativeNotifications();

const data = reactive({
  connection: null,
});

onMounted(() => {
  console.log("Starting connection...");
  data.connection = new WebSocket("wss://movies-feed.dicoding.dev");
  // data.connection = new WebSocket(
  //   "wss://demo.piesocket.com/v3/channel_123?api_key=VCXCEuvhGcBDP7XhiJJUDvR1e1D3eiVjgZ9VRiaV&notify_self"
  // );
  console.log("data.connection ", data.connection);

  data.connection.onopen = function (event) {
    console.log(event);
    console.log("Connection established");
  };
  data.connection.onmessage = function (event) {
    console.log(event);
    notify();
  };
  // data.connection.onopen((event) => {
  //   console.log(event);
  //   console.log("Successfully connected to the echo Webscoket Server");
  // });
});

const notify = () => {
  console.log("notify ", nativeNotification);
  // https://developer.mozilla.org/en-US/docs/Web/API/Notification/Notification#Parameters
  try {
    nativeNotification.show(
      "Hello World",
      {
        body: "Hello Tono",
      },
      {
        onerror: function () {
          console.log("Custom error event was called");
        },
        onclick: function () {
          console.log("Custom click event was called");
        },
        onclose: function () {
          console.log("Custom close event was called");
        },
        onshow: function () {
          console.log("Custom show event was called");
        },
      }
    );
  } catch (error) {
    console.log("error ", error);
  }
};

const sendMessage = (msg) => {
  console.log("send msg");
  try {
    notify();
    data.connection.send(msg);
  } catch (error) {
    console.log("error ", error);
  }
};
</script>

<template>
  <h1 class="text-3xl font-bold underline">Hello world!</h1>
  <button class="bg-red-400" @click="sendMessage('Hello Beach')">
    Send Message
  </button>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
