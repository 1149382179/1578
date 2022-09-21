<template>
  <div class="warp listBox" @click.prevent="checked(todo.id)">
    <label>
      <input
        type="checkbox"
        class="selectBox"
        :class="isInputChecked"
        :checked="todo.completed"
      />
      <!-- 如下代码也能实现功能，但是不推荐，因为修改了props -->
      <!-- <input type="checkbox" class="selectBox" v-model="todo.completed" /> -->
      <div class="list" :class="isChecked">{{ todo.title }}</div>
    </label>
    <button class="deleteBtn" @click="handleDelete(todo.id)">x</button>
  </div>
</template>

<script>
export default {
  name: "Item",

  //声明接收todo对象
  props: ["todo"],
  data() {
    return {
      isChecked: "",
      isInputChecked: "",
    };
  },
  methods: {
    checked(id) {
      // this.checkTodo(id);
      this.$bus.$emit("checkTodo", id);
      this.checkedClass();
    },
    handleDelete(id) {
      if (confirm("确定删除吗？")) {
        this.$bus.$emit("deleteTodo", id);
      }
    },
    checkedClass() {
      this.isChecked = this.todo.completed ? "completed" : "";
      this.isInputChecked = this.todo.completed ? "selectBox_checked" : "";
    },
  },
  mounted() {
    this.checkedClass();
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

.list.completed {
  text-decoration: line-through;
  color: #b8bed1;
}

.deleteBtn {
  opacity: 0;
  color: #b8bed1;
  background-color: #b8bed100;
  border: none;
}
</style>
