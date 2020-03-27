<template>
  <div class="item">
    <div>
      <div
        class="student"
        ref="student"
        :contenteditable="owner"
        @blur="updateStudent"
        @keyup="getMatches"
      >{{item.student}}</div>
      <Matches :matches="matches" />
    </div>
    <div>to see {{item.faculty}}</div>
    <div>
      <span v-if="owner" class="remove" @click="remove">X</span>
      <span class="user">{{item.display_name}}</span>
    </div>
  </div>
</template>

<script>
import Matches from "@/components/Matches";

export default {
  components: {
    Matches
  },
  computed: {
    owner() {
      return this.item.user === this.user;
    }
  },
  data: () => {
    return {
      matches: []
    };
  },
  methods: {
    remove() {
      this.$emit("removeItem", this.item.id);
    },
    getMatches() {
      this.matches = [];
      if (this.$refs.student.innerText.length > 1) {
        this.matches = this.students.filter(s =>
          s.includes(this.$refs.student.innerText)
        );
      }
    },
    updateStudent() {
      // Hack fix to updating content
      this.item.student = this.$refs.student.innerText;
      this.$emit("updateItem", this.item);
      // Hack fix to content doubling
      this.$refs.student.innerText = this.item.student;
    }
  },
  props: {
    item: {
      type: Object,
      required: true
    },
    students: {
      type: Array,
      required: true
    },
    user: {
      type: String,
      required: true
    }
  },
  watch: {
    item: {
      handler: function() {},
      deep: true
    }
  },
  mounted() {
    if (this.item.student === "") {
      this.$refs.student.focus();
    }
  }
};
</script>

<style scoped>
.item {
  align-items: center;
  background: #eeceab;
  border: 1px solid #888888;
  border-radius: 1vw;
  display: flex;
  justify-content: space-between;
  margin: 1vw;
  padding: 1vw;
}
.remove {
  background: #c7a3a1;
  border: 1px solid #888888;
  border-radius: 10px;
  display: inline-block;
  font-weight: bold;
  padding: 5px;
  user-select: none;
  width: 26px;
}
.remove:hover {
  cursor: pointer;
}
.student {
  font-size: 1.3em;
  outline: 0;
  padding: 1vw;
}
.student:focus {
  background: #fefdf9;
  border-radius: 1vw;
}
.user {
  display: block;
  font-size: 0.8em;
  font-style: italic;
}
</style>