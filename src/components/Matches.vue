<template>
  <div>
    <div class="match" @click="chooseMatch" v-for="m in matches" :key="m">{{m}}</div>
  </div>
</template>

<script>
export default {
  methods: {
    chooseMatch(e) {
      this.$emit("selection", e.target.innerText);
    },
  },
  props: {
    matches: {
      type: Array,
      required: true,
    },
  },
};
</script>

<style scoped>
.match {
  background: #686868;
  border: 1px dashed #888888;
  border-radius: 4px;
  margin: 4px;
  padding: 6px;
}
.match:hover {
  cursor: pointer;
}
</style>