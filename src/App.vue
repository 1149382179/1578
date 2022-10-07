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
    //修改一个todo
    updateTodo(id, title) {
      this.todoList.forEach((todo) => {
        if (todo.id == id) todo.title = title.trim();
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
    //清除所有
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
    this.$bus.$on("updateTodo", this.updateTodo);
    // this.$bus.$on("deleteTodo", this.deleteTodo);
    this.pubId = pubsub.subscribe("deleteTodo", this.deleteTodo);
  },
  beforeDestroy() {
    this.$bus.$off("checkTodo");
    this.$bus.$off("updateTodo");
    // this.$bus.$off("deleteTodo");
    pubsub.unsubscribe(this.pubId);
  },
};
</script>

<style lang="css">
html,
body,
#app {
  margin: 0;
  padding: 0;
  height: 100%;
}

body {
  background-color: #e3ebf4;
}

.warp {
  width: 100vw;
  height: 100vh;
  display: flex;
}
.bodyBox {
  /* 水平垂直居中 */
  margin: auto;
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
