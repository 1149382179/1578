<template>
  <div>
    <div class="title">今日任务</div>
    <input
      class="textBox"
      type="text"
      placeholder="输入任务后按Enter添加"
      v-model="title"
      @keyup.enter="add"
    />
  </div>
</template>

<script>
import { nanoid } from "nanoid";
export default {
  name: "UserHeader",
  data() {
    return {
      title: "",
    };
  },
  methods: {
    add() {
      //检查是否为空
      if (!this.title.trim()) {
        return;
      }
      //将用户的输入包装成一个todo对象
      const todoObj = { id: nanoid(), title: this.title, completed: false };
      this.$emit("addTodo", todoObj);
      this.title = "";
    },
  },
  // props: ["addTodo"],
};
</script>

<style lang="css" scoped>
.title {
  font: bold;
  font-size: 30px;
  color: cornflowerblue;
  text-align: center;
  margin-bottom: 30px;
}

.textBox {
  border: none;
  outline: none;
  width: 350px;
  height: 50px;
  padding-left: 10px;
  margin-bottom: 30px;
  font-size: 1rem;
  border-radius: 10px;
  background: #e3eaf4;
  box-shadow: inset 5px 5px 10px #c1c7cf, inset -5px -5px 10px #ffffff;
}
</style>
