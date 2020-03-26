<template>
  <div id="app">
    <h1>Announcement Manager</h1>
    <Announcement v-for="item in announcements" :key="item.id" :item="item" :user="user" @removeItem="remove" @updateItem="update" />
    <button @click="add">Add Announcement</button>
  </div>
</template>

<script>
import Announcement from '@/components/Announcement'
import * as firebase from 'firebase/app'
import 'firebase/firestore'

const config = {
    apiKey: "AIzaSyCg_FVHdzP3kvRAyrnpE2bJUsQfMRUgFW4",
    authDomain: "my-covenant.firebaseapp.com",
    databaseURL: "https://my-covenant.firebaseio.com",
    projectId: "my-covenant",
    storageBucket: "my-covenant.appspot.com",
    messagingSenderId: "945207168321",
    appId: "1:945207168321:web:e42d0845df84c8c24e65c0"
  }
firebase.initializeApp(config)
const db = firebase.firestore()


export default {
  name: 'App',
  components: {
    Announcement
  },
  data: () => {
    return {
      announcements: [],
      user: 'brad'
    }
  },
  methods: {
    add() {
      db.collection('announcements').add({
        content: '',
        days: 1,
        user: this.user
      })
    },
    remove(id) {
      db.collection('announcements').doc(id).delete()
    },
    update(item) {
      db.collection('announcements').doc(item.id).set({
        content: item.content,
        days: item.days,
        user: item.user
      })
    }
  },
  mounted() {
    db.collection('announcements').onSnapshot((snapshot) => {
      this.announcements = []
      snapshot.forEach((doc) => {
        this.announcements.push({ id: doc.id, ...doc.data() })
      })
    })
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Arvo|Oswald&display=swap');

body {
  background: #b5c2ab;
  margin: 0;
  padding: 0 0 4vh 0;
}

button {
  background: #b1d6e1;
  border: 1px solid #888888;
  border-radius: 1vw;
  font-family: Oswald, sans-serif;
  font-size: 1.5em;
  outline: 0;
  padding: 4px 12px;
  text-transform: uppercase;
}

button:hover {
  cursor: pointer;
}

h1 {
  font-family: Oswald, sans-serif;
  text-transform: uppercase;
}

#app {
  font-family: Arvo, serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
