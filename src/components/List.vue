<template>
  <div>
    <label>
      <span class="action">
        <input type="checkbox" @click="checkAllTodo(nums)" />
        <i></i>
      </span>
    </label>
    <Item v-for="todoObj in todoList" :key="todoObj.id" :todo="todoObj" />
  </div>
</template>

<script>
import Item from "./Item.vue";

export default {
  data() {
    return {
      nums: 0,
      isChecked: "",
      isInputChecked: "",
    };
  },
  name: "List",
  components: { Item },
  props: ["todoList"],
  methods: {
    checkAllTodo(nums) {
      this.$bus.$emit("checkAllTodo", nums);
      this.nums++;
      this.todoList.forEach(() => {
        this.checkedClass();
      });
    },
    checkedClass() {
      this.isChecked = this.todo.completed ? "completed" : "";
      this.isInputChecked = this.todo.completed ? "selectBox_checked" : "";
    },
    mounted() {
      this.$bus.$on("checkedClass", this.checkedClass);
    },
    beforeDestroy() {
      this.$bus.$off("checkedClass");
    },
    // checkClass(todo) {
    //   this.$bus.$emit("checkClass", todo);
    // },
  },
};
</script>

<style lang="css" scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.action input {
  appearance: none;
}

.action {
  position: relative;
  width: 350px;
  height: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
  user-select: none;
  cursor: pointer;
  border-radius: 20px;
  background: #e0e0e0;
  box-shadow: inset 3px 3px 5px #bebebe, inset -3px -3px 5px #ffffff;
  transform: 1s;
}

.action i {
  position: relative;
  width: 350px;
  /* right: 175px; */
  height: 20px;
  border-radius: 15px;
  transition: 0.5s;
}

.action input:checked ~ i {
  background: #2aa0ff;
  box-shadow: inset 3px 3px 5px #2488d9, inset -3px -3px 5px #30b8ff;
}

.action i::before {
  content: "";
  position: absolute;
  left: 4px;
  top: 2.5px;
  width: 16px;
  height: 16px;
  border-radius: 20px;
  background-color: #fff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: 0.8s;
}

.action input:checked ~ i::before {
  left: calc(100% - 20px);
}
</style>
