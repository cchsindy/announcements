<template>
  <div class="item">
    <div>
      <span v-if="owner" class="minus" @click="minus">-</span>
      <span class="days">{{item.days}} {{days}}</span>
      <span v-if="owner" class="plus" @click="plus">+</span>
    </div>
    <Message :message="item.content" :owner="owner" @update="updateContent" />
    <div>
      <span v-if="owner" class="remove" @click="remove">X</span>
      <span class="user">{{item.display_name}}</span>
    </div>
  </div>
</template>

<script>
import Message from "@/components/Message";

export default {
  components: {
    Message
  },
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
    updateContent(content) {
      this.item.content = content;
      this.$emit("updateItem", this.item);
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
.days {
  display: inline-block;
  margin: 1vw;
  white-space: nowrap;
  user-select: none;
}
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