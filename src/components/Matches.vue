<template>
  <div>
    <div class="match" v-for="m in matches" :key="m">{{m}}</div>
  </div>
</template>

<script>
export default {
  props: {
    matches: {
      type: Array,
      required: true
    }
  }
};
</script>

<style scoped>
.match {
  background: #b1d6e1;
  border: 1px dashed #888888;
  border-radius: 4px;
  margin: 4px;
  padding: 6px;
}
.match:hover {
  cursor: pointer;
}
</style>