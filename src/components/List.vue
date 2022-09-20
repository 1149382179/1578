<template>
  <div>
    <label>
      <span class="action">
        <input type="checkbox" />
        <i></i>
      </span>
    </label>
    <Item
      v-for="todoObj in todoList"
      :key="todoObj.id"
      :todo="todoObj"
      :checkTodo="checkTodo"
      :deleteTodo="deleteTodo"
    />
  </div>
</template>

<script>
import Item from "./Item.vue";

export default {
  name: "List",
  components: { Item },
  props: ["todoList", "checkTodo", "deleteTodo"],
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
  display: flex;
  justify-content: center;
  align-items: center;
  user-select: none;
  cursor: pointer;
  color: #777;
  position: relative;
  width: 350px;
  height: 20px;
  background: #ddd;
  border-radius: 20px;
  transform: 1s;
}

.action i {
  position: relative;
  width: 350px;
  height: 20px;
  background: #ddd;
  border-radius: 15px;
  transition: 0.5s;
}

.action input:checked ~ i {
  background: #2aa0ff;
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
  transition: 0.5s;
}

.action input:checked ~ i::before {
  left: calc(100% - 20px);
}
</style>
