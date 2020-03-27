<template>
  <div class="item">
    <div>
      <span v-if="owner" class="minus" @click="minus">-</span>
      <span class="days">{{item.days}} {{days}}</span>
      <span v-if="owner" class="plus" @click="plus">+</span>
    </div>
    <div
      class="content"
      ref="content"
      :contenteditable="owner"
      @blur="updateContent"
    >{{item.content}}</div>
    <div>
      <span v-if="owner" class="remove" @click="remove">X</span>
      <span class="user">{{item.display_name}}</span>
    </div>
  </div>
</template>

<script>
export default {
  computed: {
    days() {
      return this.item.days === 1 ? "day" : "days";
    },
    owner() {
      return this.item.user === this.user;
    }
  },
  methods: {
    minus() {
      if (this.item.days > 1) {
        this.item.days--;
        this.$emit("updateItem", this.item);
      }
    },
    plus() {
      if (this.item.days < 5) {
        this.item.days++;
        this.$emit("updateItem", this.item);
      }
    },
    remove() {
      this.$emit("removeItem", this.item.id);
    },
    updateContent() {
      // Hack fix to updating content
      this.item.content = this.$refs.content.innerText;
      this.$emit("updateItem", this.item);
      // Hack fix to content doubling
      this.$refs.content.innerText = this.item.content;
    }
  },
  props: {
    item: {
      type: Object,
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
    if (this.item.content === "") {
      this.$refs.content.focus();
    }
  }
};
</script>

<style scoped>
.content {
  font-size: 1.3em;
  outline: 0;
  padding: 1vw;
}
.content:focus {
  background: #fefdf9;
  border-radius: 1vw;
}
.days {
  display: inline-block;
  margin: 1vw;
  white-space: nowrap;
  user-select: none;
}
.item {
  background: #eeceab;
  border: 1px solid #888888;
  border-radius: 1vw;
  display: flex;
  justify-content: space-between;
  margin: 2vw;
  padding: 1vw;
}
.minus,
.plus {
  background: #b1d6e1;
  border: 1px solid #888888;
  border-radius: 10px;
  display: inline-block;
  font-weight: bold;
  padding: 5px;
  user-select: none;
  width: 26px;
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
.minus:hover,
.plus:hover,
.remove:hover {
  cursor: pointer;
}
.user {
  display: block;
  font-size: 0.8em;
  font-style: italic;
}
</style>