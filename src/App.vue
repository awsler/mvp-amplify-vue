<template>
  <div id="app">
    <div v-if="!signedIn">
      <amplify-authenticator></amplify-authenticator>
    </div>
    <div v-if="signedIn">
      <amplify-sign-out class="signout"></amplify-sign-out>
      <div>
        <div class="form">
          <input class="input" v-model="name" :placeholder="$Amplify.I18n.get('Name')">
          <input
            class="input"
            v-model="description"
            :placeholder="$Amplify.I18n.get('Description')"
          >
          <button class="button" v-on:click="createEvent">{{ $Amplify.I18n.get('Create Event') }}</button>
        </div>
        <div v-for="item in events" :key="item.key" class="list-item">
          <hr>
          <p class="name">{{ $Amplify.I18n.get('Event') }}: {{ item.name }}</p>
          <p class="description">{{ $Amplify.I18n.get('Description') }}: {{ item.description }}</p>
          <hr>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { AmplifyEventBus } from "aws-amplify-vue";
import { Auth, API, I18n, Logger, graphqlOperation } from "aws-amplify";
import { listEvents } from "./graphql/queries";
import { createEvent } from "./graphql/mutations";

const logger = new Logger("app");

const dict = {
  de: {
    Event: "Veranstaltung",
    Name: "Name",
    Description: "Beschreibung",
    "Create Event": "Veranstaltung erstellen"
  }
};
I18n.putVocabularies(dict);

export default {
  name: "app",
  data() {
    return {
      name: "",
      description: "",
      events: [],
      signedIn: false
    };
  },
  async beforeCreate() {
    const {
      data: {
        listEvents: { items }
      }
    } = await API.graphql(graphqlOperation(listEvents));
    this.events = items;

    try {
      await Auth.currentAuthenticatedUser();
      this.signedIn = true;
    } catch (err) {
      this.signedIn = false;
    }

    AmplifyEventBus.$on("authState", info => {
      if (info === "signedIn") {
        this.signedIn = true;
      } else {
        this.signedIn = false;
      }
    });
  },
  methods: {
    async createEvent() {
      if (this.name === "" || this.description === "") {
        return;
      }
      const event = { name: this.name, description: this.description };
      const events = [...this.events, event];
      this.events = events;
      this.name = "";
      this.description = "";
      try {
        await API.graphql(graphqlOperation(createEvent, { input: event }));
        logger.info("Event successfully created!");
      } catch (err) {
        logger.error("Error adding event: ", err);
      }
    }
  }
};
</script>

<style>
body {
  margin: 0;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.container {
  padding: 40px;
}
.signout {
  background-color: #ededed;
  margin: 0;
  padding: 11px 0px 1px;
}
</style>
