<template>
  <div class="warp listBox" @click.prevent="checked(todo.id)">
    <label>
      <input
        type="checkbox"
        class="selectBox"
        :checked="todo.completed"
        :class="isInputChecked"
      />
      <!-- 如下代码也能实现功能，但是不推荐，因为修改了props -->
      <!-- <input type="checkbox" class="selectBox" v-model="todo.completed" /> -->
      <div class="list" :style="isChecked">{{ todo.title }}</div>
    </label>
    <button class="deleteBtn" @click="handleDelete(todo.id)">x</button>
  </div>
</template>

<script>
export default {
  name: "Item",

  //声明接收todo对象
  props: ["todo", "checkTodo", "deleteTodo"],
  data() {
    return {
      isChecked: "",
      notChecked: "#aab4c5",
      Checked: "text-decoration:line-through;color:#b8bed1",
      isInputChecked: "",
      inputChecked: "selectBox_checked",
    };
  },
  methods: {
    checked(id) {
      this.checkTodo(id);
      if (this.todo.completed) {
        this.isChecked = this.Checked;
        this.isInputChecked = this.inputChecked;
        console.log(this.isInputChecked);
      } else {
        this.isChecked = this.notChecked;
        this.isInputChecked = "";
      }
    },
    handleDelete(id) {
      if (confirm("确定删除吗？")) {
        this.deleteTodo(id);
      }
    },
  },
};
</script>

<style lang="less" scoped>
.warp label {
  display: flex;
  flex-wrap: nowrap;
  align-items: center;
}
.listBox {
  margin-top: 24px;
  width: 350px;
  height: 50px;
  font-size: 1.2rem;
  color: #56595e;
  border-radius: 10px;
  background: #e3eaf4;
  box-shadow: 5px 5px 8px #c1c7cf, -5px -5px 8px #ffffff;
  transition: all 3s linear 1s;
}
.selectBox {
  appearance: none;
  width: 1.2rem;
  height: 1.2rem;
  margin-left: 8px;
  border-radius: 100%;
  border: skyblue 1px solid;
  text-align: center;
}
/*判断是否有选择*/
.selectBox_checked:after {
  content: "\2714";
  color: skyblue;
}
/*css 方式实现
input[type="checkbox"]:checked::before {
  content: "\2714";
  color: skyblue;
}*/
.list {
  width: 250px;
  margin-left: 16px;
}
.listBox:hover .deleteBtn {
  opacity: 1;
}
.deleteBtn {
  opacity: 0;
  color: #b8bed1;
  background-color: #b8bed100;
  border: none;
}
</style>
