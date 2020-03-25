<template>
  <div id="app">
    <h1>Announcement Manager</h1>
    <Announcement v-for="item in announcements" :key="item.id" :item="item" :user="user" @removeItem="remove" />
    <button @click="add">Add Announcement</button>
  </div>
</template>

<script>
import Announcement from '@/components/Announcement'

export default {
  name: 'App',
  components: {
    Announcement
  },
  data: () => {
    return {
      announcements: [
        { id: 1, content: 'This is a test.', days: 3, user: 'brad' },
        { id: 2, content: 'This is also a test.', days: 1, user: 'bob' }
      ],
      user: 'brad'
    }
  },
  methods: {
    add() {
      this.announcements.push({ id: 3, content: 'Brand new!', days: 5, user: 'brad' })
    },
    remove(id) {
      this.announcements = this.announcements.filter(a => a.id !== id)
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Arvo|Oswald&display=swap');

body {
  background: #b5c2ab;
  margin: 0;
  padding: 0;
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
