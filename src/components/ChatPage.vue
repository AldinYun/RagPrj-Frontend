<template>
  <v-layout ref="app" class="rounded rounded-md" style="height: 100vh;">
    <!-- Header -->
    <v-app-bar color="grey-lighten-2" name="app-bar">
      <child v-slot="{ print }">
        <v-btn class="mx-auto" @click="print('app-bar')">header</v-btn>
      </child>
    </v-app-bar>

    <!-- Navigation Drawer -->
    <v-navigation-drawer>
      <v-list>
        <v-list-item title="chat list"></v-list-item>
      </v-list>
    </v-navigation-drawer>

   <!-- Main Content -->
    <v-main style="height: 100%; margin-left:5vh; margin-right:5vh;">
      <!-- Chat History Area -->
      <div>
        <v-list>
          <!-- Iterate through chatList -->
          <v-list-item v-for="(chat, index) in chatList" :key="index">
            <v-list-item-content>
              <!-- Display sender and message -->
              <v-list-item-title class="custom-v-list-item-title">
                <span v-if="chat.sender === 'ai'" style="color: blue;"><div class="circle-container-avt-a">AI</div></span>
                <span v-else-if="chat.sender === 'user'" style="color: red;"><div class="circle-container-avt-u">ME</div></span>
                <!-- Apply word-break CSS style to handle long messages -->
                <br><p style="margin-left:3vh;margin-right:9vh; word-break: break-all;">{{ chat.message }}</p>
              </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </div>
    </v-main>

    <!-- Footer -->
    <v-footer name="footer" app class="footer">
      <!-- Textarea for typing message -->
      <v-textarea
        v-model="newMessage"
        name="input-7-1"
        rows="1"
        variant="filled"
        auto-grow
        style="margin-left:15px;margin-right:15px;"
        placeholder="Type your message here"
      ></v-textarea>

      <!-- Send button -->
      <div class="circle-container">
        <img :src="require('@/assets/sendIcon.svg')" alt="Send Icon" class="send-icon" @click="sendMessage" />
      </div>
    </v-footer>
  </v-layout>
</template>

<script>
import { useLayout } from 'vuetify';

const Child = {
  setup (props, ctx) {
    const { getLayoutItem } = useLayout();

    function print (key) {
      alert(JSON.stringify(getLayoutItem(key), null, 2));
    }

    return () => ctx.slots.default({ print });
  },
};

export default {
  components: { Child },
  data() {
    return {
      chatList: [
        { sender: 'ai', message: 'Hello' },
        { sender: 'user', message: 'Hi there' },
        { sender: 'ai', message: 'How are you? How are you? How are you? How are you? How are you? How are you? How are you? How are you?' },
        // Add more chat messages as needed
      ],
      newMessage: '' // Variable to store new message typed by user
    };
  },
  methods: {
    sendMessage() {
      // Add new message to chatList
      this.chatList.push({ sender: 'User1', message: this.newMessage });
      // Clear the textarea after sending message
      this.newMessage = '';
    }
  }
};
</script>

<style>
.footer {
  position: fixed;
  bottom: 0;
  width: 100%;
  z-index: 999;
}

.circle-container {
  width: 5vh;
  height: 5vh;
  background-color: rgb(227, 227, 227);
  margin-bottom: 2vh;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
}
.circle-container-avt-a {
  width: 5vh;
  height: 5vh;
  background-color: rgb(106, 129, 231);
  margin-top:2vh;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
}
.circle-container-avt-u {
  width: 5vh;
  height: 5vh;
  background-color: rgb(119, 229, 141);
  margin-top:2vh;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
}
.send-icon {
  width: 50%;
  height: auto;
}

/* Custom CSS for v-list-item-title */
.custom-v-list-item-title {
  white-space: normal !important;
}
</style>
