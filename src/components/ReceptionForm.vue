<template>
  <v-form class="main-form mt-5" ref="form" v-model="valid" lazy-validation>
    <v-text-field v-model="name" :counter="10" :rules="nameRules" label="Name" required></v-text-field>

    <v-text-field v-model="message" :rules="messageRules" label="Message" required></v-text-field>

    <v-select
      v-model="select"
      @change="getUserChannel"
      :items="users"
      :rules="[v => !!v || 'Item is required']"
      label="Person Of Contact"
      item-text="username"
      item-value="userId"
      required
    ></v-select>
    <!-- </div> -->
    <v-btn :disabled="!valid" color="success" class="mr-4" @click="validate">Send Message</v-btn>

    <v-btn color="error" class="mr-4" @click="reset">Reset Form</v-btn>
    <v-snackbar v-model="snackbar" :timeout="timeout">
      {{ text }}
      <v-btn color="blue" text @click="snackbar = false">Close</v-btn>
    </v-snackbar>
  </v-form>
</template>

<script>
import axios from "axios";
export default {
  data: () => ({
    snackbar: false,
    text: "Message sent!",
    timeout: 2000,
    valid: true,
    name: "",
    nameRules: [
      v => !!v || "Name is required",
      v => (v && v.length >= 1) || "Name must be greater than 1 character"
    ],
    message: "",
    messageRules: [
      v => !!v || "Message is required",
      v => (v && v.length >= 5) || "Message must be greater than 5 characters"
    ],
    select: null,
    users: [],
    checkbox: false,
    userChannel: ""
  }),

  methods: {
    validate() {
      if (this.$refs.form.validate()) {
        this.snackbar = true;
        let channel = this.userChannel;
        let messageChunk = this.message;
        let senderName = this.name;
        const message = `New message from: ${" "}${senderName} - ${messageChunk}`;
        this.sendNotification(channel, message);
      }
    },
    reset() {
      this.$refs.form.reset();
    },
    resetValidation() {
      this.$refs.form.resetValidation();
    },
    userSelectChange(event) {
      this.userID = event.target.value;
      console.log(this.userID);
    },
    getUserChannel(e) {
      this.userChannel = e;
      console.log(this.userChannel);
    },
    sendNotification(userChannel, message) {
      this.message = event.target.value;
      console.log(this.message);
      axios
        .post(
          `https://slack.com/api/chat.postMessage?token=${process.env.VUE_APP_SLACK_BOT_TOKEN}&channel=${userChannel}&text=${message}`
        )
        .then(res => {
          console.log(res);
        });
    }
  },
  async mounted() {
    const userArray = [];
    const {
      data: { members }
    } = await axios.get(
      `https://slack.com/api/users.list?token=${process.env.VUE_APP_SLACK_BOT_TOKEN}&pretty=1`
    );
    members.filter(x => {
      if (x.deleted !== true) {
        userArray.push({ userId: x.id, username: x.real_name });
      }
    });
    this.users = userArray;
    console.log(this.users);
  }
};
</script>

<style lang="scss" scoped>
.main-form {
  width: 50%;
  margin: 0 auto;
}
</style>