<template>
  <div
    ref="student"
    class="student"
    :contenteditable="owner"
    @blur="blur"
    @keyup="keyUp"
  >{{student}}</div>
</template>

<script>
export default {
  methods: {
    blur() {
      setTimeout(() => {
        this.$refs.student.innerText = this.student;
      }, 500);
    },
    keyUp() {
      if (this.$refs.student.innerText.length > 1)
        this.$emit("matches", this.$refs.student.innerText);
    }
  },
  props: {
    owner: {
      type: Boolean,
      required: true
    },
    student: {
      type: String,
      required: true
    }
  },
  mounted: function() {
    this.$nextTick(function() {
      if (this.student === "") this.$refs.student.focus();
    });
  }
};
</script>

<style scoped>
.student {
  font-size: 1.3em;
  outline: 0;
  padding: 1vw;
}
.student:focus {
  background: #fefdf9;
  border-radius: 1vw;
}
</style>