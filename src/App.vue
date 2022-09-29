<template>
  <div class="warp">
    <div class="bodyBox">
      <UserHeader @addTodo="addTodo" />
      <List :todoList="todoList" />
      <UserFooter :todoList="todoList" @clearTodo="clearTodo" />
    </div>
  </div>
</template>

<script>
import pubsub from "pubsub-js";
import UserFooter from "./components/UserFooter.vue";
import UserHeader from "./components/myHeader.vue";
import List from "./components/List.vue";

export default {
  name: "App",
  data() {
    return {
      todoList: JSON.parse(localStorage.getItem("todos")) || [
        {
          id: "001",
          title: "吃饭",
          completed: false,
        },
        {
          id: "002",
          title: "睡觉",
          completed: false,
        },
        {
          id: "003",
          title: "打代码",
          completed: false,
        },
      ],
      // isChecked: "",
      // isInputChecked: "",
    };
  },
  methods: {
    addTodo(x) {
      this.todoList.unshift(x);
    },
    //勾选一个todo
    checkTodo(id) {
      this.todoList.forEach((todo) => {
        if (todo.id == id) todo.completed = !todo.completed;
      });
    },
    //全选todo
    checkAllTodo(val) {
      this.todoList.forEach((todoList) => {
        todoList.completed = val ? false : true;
      });
    },
    //删除一个Todo
    deleteTodo(_, id) {
      //下划线占位
      this.todoList = this.todoList.filter((todo) => todo.id !== id);
    },
    clearTodo() {
      this.todoList = this.todoList.filter((todo) => !todo.completed);
    },
  },
  watch: {
    todoList: {
      deep: true,
      handler(value) {
        localStorage.setItem("todos", JSON.stringify(value));
      },
    },
  },
  components: { UserFooter, UserHeader, List },
  mounted() {
    this.$bus.$on("checkTodo", this.checkTodo);
    // this.$bus.$on("deleteTodo", this.deleteTodo);
    this.pubId = pubsub.subscribe("deleteTodo", this.deleteTodo);
    this.$bus.$on("checkAllTodo", this.checkAllTodo);
  },
  beforeDestroy() {
    this.$bus.$off("checkTodo");
    // this.$bus.$off("deleteTodo");
    pubsub.unsubscribe(this.pubId);
    this.$bus.$off("checkAllTodo");
  },
};
</script>

<style lang="css">
body {
  background-color: #e3ebf4;
}
.warp {
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 80px;
}
.bodyBox {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 500px;
  height: auto;
  padding: 50px 0 20px 0;
  border-radius: 20px;
  background: #e3ebf4;
  box-shadow: 9px 9px 14px #adb3b9, -9px -9px 14px #ffffff;
}
</style>
