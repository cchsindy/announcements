<template>
  <div class="item">
    <div>
      <Student
        :owner="owner"
        :student="item.student"
        @matches="getMatches"
        @update="updateStudent"
      />
      <Matches :matches="matches" @selection="selectedMatch" />
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
import Student from "@/components/Student";

export default {
  components: {
    Matches,
    Student
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
    getMatches(student) {
      this.matches = this.students.filter(s => s.includes(student));
    },
    selectedMatch(student) {
      this.item.student = student;
      this.updateStudent();
    },
    updateStudent() {
      this.$emit("updateItem", this.item);
      this.matches = [];
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
.user {
  display: block;
  font-size: 0.8em;
  font-style: italic;
}
</style>