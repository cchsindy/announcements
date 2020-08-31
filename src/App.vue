<template>
  <div id="app">
    <h1>Announcement Manager</h1>
    <div v-if="user" class="wrapper">
      <div class="announcements">
        <h2>Announcements</h2>
        <Announcement
          v-for="item in announcements"
          :key="item.id"
          :item="item"
          :user="user"
          @removeItem="removeAnnouncement"
          @updateItem="updateAnnouncement"
        />
        <button @click="addAnnouncement">Add Announcement</button>
      </div>
      <div class="notifications">
        <h2>Notifications</h2>
        <Notification
          v-for="item in notifications"
          :key="item.id"
          :item="item"
          :students="students"
          :user="user"
          @removeItem="removeNotification"
          @updateItem="updateNotification"
        />
        <button @click="addNotification">Add Notification</button>
      </div>
    </div>
    <div v-else>
      <p>You must login to access this site.</p>
      <button @click="google">Login with Google</button>
    </div>
  </div>
</template>

<script>
import Announcement from "@/components/Announcement";
import Notification from "@/components/Notification";
import * as firebase from "firebase/app";
import "firebase/auth";
import "firebase/firestore";
import "firebase/functions";

const config = {
  apiKey: "AIzaSyCg_FVHdzP3kvRAyrnpE2bJUsQfMRUgFW4",
  authDomain: "my-covenant.firebaseapp.com",
  databaseURL: "https://my-covenant.firebaseio.com",
  projectId: "my-covenant",
  storageBucket: "my-covenant.appspot.com",
  messagingSenderId: "945207168321",
  appId: "1:945207168321:web:e42d0845df84c8c24e65c0",
};
firebase.initializeApp(config);
const provider = new firebase.auth.GoogleAuthProvider();
const db = firebase.firestore();
const fn = firebase.functions().httpsCallable("skyapi");

export default {
  name: "App",
  components: {
    Announcement,
    Notification,
  },
  data: () => {
    return {
      announcements: [],
      notifications: [],
      students: [],
      user: null,
      displayName: "",
      title: "",
      last: "",
    };
  },
  methods: {
    addAnnouncement() {
      db.collection("announcements").add({
        category: "general-academics",
        content: "",
        days: 1,
        display_name: this.displayName,
        hidden: false,
        user: this.user,
      });
    },
    addNotification() {
      db.collection("notifications").add({
        student: "",
        faculty: `${this.title} ${this.last}`,
        display_name: this.displayName,
        user: this.user,
      });
    },
    google() {
      firebase.auth().signInWithPopup(provider);
    },
    removeAnnouncement(id) {
      db.collection("announcements").doc(id).delete();
    },
    removeNotification(id) {
      db.collection("notifications").doc(id).delete();
    },
    updateAnnouncement(item) {
      db.collection("announcements").doc(item.id).set({
        category: item.category,
        content: item.content,
        days: item.days,
        display_name: this.displayName,
        hidden: item.hidden,
        user: item.user,
      });
    },
    updateNotification(item) {
      db.collection("notifications").doc(item.id).set({
        student: item.student,
        faculty: item.faculty,
        display_name: this.displayName,
        user: item.user,
      });
    },
  },
  mounted() {
    firebase.auth().onAuthStateChanged((user) => {
      if (
        user &&
        user.email.substring(user.email.length - 21) === "covenantchristian.org"
      ) {
        this.user = user.uid;
        this.displayName = user.displayName;
        getStudents(this, 1);
        const ref = db.collection("users").doc(user.uid);
        ref.get().then((doc) => {
          const d = doc.data();
          this.title = d.title;
          this.last = d.last;
        });
        db.collection("announcements")
          .where("hidden", "==", false)
          .onSnapshot((snapshot) => {
            this.announcements = [];
            snapshot.forEach((doc) => {
              this.announcements.push({ id: doc.id, ...doc.data() });
            });
          });
        db.collection("notifications").onSnapshot((snapshot) => {
          this.notifications = [];
          snapshot.forEach((doc) => {
            this.notifications.push({ id: doc.id, ...doc.data() });
          });
        });
      }
    });
  },
};

function getStudents(context, index) {
  fn({
    product: "school",
    url: "users",
    params: {
      roles: "62830",
      marker: index,
    },
  }).then((result) => {
    for (const s of result.data.value) {
      let first = s.nick_name ? s.nick_name : s.first_name;
      context.students.push(`${s.last_name}, ${first}`);
    }
    if (result.data.next_link) getStudents(context, index + 100);
  });
}
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Work+Sans:wght@200&display=swap");

body {
  background: #929292;
  font-family: "Work Sans", sans-serif;
  margin: 0;
  padding: 0 0 4vh 0;
}

button {
  background: #bdbdbd;
  border: 2px solid #646464;
  border-radius: 1vw;
  font-size: 1.5em;
  outline: 0;
  padding: 4px 12px;
  text-transform: uppercase;
}

button:hover {
  cursor: pointer;
}

h1 {
  text-transform: uppercase;
}

select {
  background: #878787;
  border: 1px sold #636363;
  border-radius: 1vw;
  padding: 1vh;
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #1f1f1f;
}

.announcements {
  width: 60vw;
}

.notifications {
  width: 40vw;
}

.wrapper {
  display: flex;
}

@media screen and (max-width: 950px) {
  .announcements {
    width: 100vw;
  }

  .notifications {
    width: 100vw;
  }

  .wrapper {
    display: inline;
  }
}
</style>
