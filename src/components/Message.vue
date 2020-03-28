<template>
  <div ref="message" class="message" :contenteditable="owner" @blur="blur">{{message}}</div>
</template>

<script>
export default {
  methods: {
    blur() {
      // setTimeout(() => {
      //   this.$refs.message.innerText = this.message;
      // }, 500);
      this.$emit("update", this.$refs.message.innerText);
    }
  },
  props: {
    message: {
      type: String,
      required: true
    },
    owner: {
      type: Boolean,
      required: true
    }
  }
};
</script>

<style scoped>
.message {
  font-size: 1.3em;
  outline: 0;
  padding: 1vw;
}
.message:focus {
  background: #fefdf9;
  border-radius: 1vw;
}
</style>