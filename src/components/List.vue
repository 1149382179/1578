<template>
  <div>
    <span class="action">
      <input type="checkbox" />
      <i></i>
    </span>
    <Item v-for="todoObj in todoList" :key="todoObj.id" :todo="todoObj" :checkTodo="checkTodo"
      :deleteTodo="deleteTodo" />
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

  .inputBox {
    margin-top: 16px;
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 260px;
    height: 40px;
    font-size: 1.3rem;
    color: #b8bed1;
    border-radius: 10px;
    background: #e3eaf4;
    box-shadow: 5px 5px 10px #c1c7cf, -5px -5px 10px #ffffff;
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

  .action input:checked~i {
    background: #2aa0ff;
  }

  .action i::before {
    content: "";
    position: absolute;
    top: 2.5px;
    left: 4px;
    width: 16px;
    height: 16px;
    border-radius: 20px;
    background-color: #fff;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    transition: 0.5s;
  }

  .action input:checked~i::before {
    left: calc(100% - 50px);
  }
</style>