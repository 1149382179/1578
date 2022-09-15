<template>
  <div class="warpBody" v-show="total">
    <div class="mission">{{ completedTotal }}/{{ total }}项任务</div>
    <button class="deleteBtn" @click="clearTodo">删除已完成</button>
  </div>
</template>

<script>
export default {
  name: "UserFooter",
  props: ["todoList", "clearTodo"],
  computed: {
    total() {
      return this.todoList.length;
    },
    completedTotal() {
      return this.todoList.reduce(
        (pre, todo) => pre + (todo.completed ? 1 : 0),
        0
      );
      //reduce 方法 pre代表返回的前一个数，current是数组里下一位数字，0表示第一个从0开始算，即第一个pre是0
      // const x = this.todoList.reduce((pre, current) => {
      //   console.log("@", pre, current);
      //   return pre + (current.completed ? 1 : 0);
      // }, 0);
    },
  },
};
</script>

<style scoped>
.warpBody {
  width: 350px;
  display: flex;
  color: #b8bed1;
  margin-top: 50px;
}
.mission {
  flex-grow: 1;
  font-size: 15px;
}
.deleteBtn {
  flex-grow: 1;
  color: #b8bed1;
  background-color: #b8bed100;
  border: none;
  text-align: right;
}
</style>
