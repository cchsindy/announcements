<template>
  <div ref="message" class="message" :contenteditable="owner" @blur="blur">{{message}}</div>
</template>

<script>
export default {
  methods: {
    blur() {
      this.$emit("update", this.$refs.message.innerText);
      // Hack fix for resetting text
      setTimeout(() => {
        this.$refs.message.innerText = this.message;
      }, 500);
    },
  },
  props: {
    message: {
      type: String,
      required: true,
    },
    owner: {
      type: Boolean,
      required: true,
    },
  },
  mounted: function () {
    this.$nextTick(function () {
      if (this.message === "") this.$refs.message.focus();
    });
  },
};
</script>

<style scoped>
.message {
  font-size: 1.3em;
  outline: 0;
  padding: 1vw;
}
.message:focus {
  background: #202020;
  border-radius: 1vw;
}
</style>