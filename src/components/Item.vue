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
    <button class="editBtn" @click.stop="">
      <MdCreateIcon w="20px" h="20px" />
    </button>
    <button class="deleteBtn" @click.stop="handleDelete(todo.id)">
      <IosCloseIcon w="25px" h="25px" />
    </button>
  </div>
</template>

<script>
import pubsub from "pubsub-js";
import MdCreateIcon from "vue-ionicons/dist/md-create.vue";
import IosCloseIcon from "vue-ionicons/dist/ios-close.vue";

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
        // this.$bus.$emit("deleteTodo", id);
        pubsub.publish("deleteTodo", id);
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
  watch: {
    todo: {
      deep: true,
      handler() {
        this.checkedClass();
      },
    },
  },
  components: { MdCreateIcon, IosCloseIcon },
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
  display: flex;
}

.selectBox {
  appearance: none;
  width: 1.2rem;
  height: 1.2rem;
  margin-left: 8px;
  border-radius: 100%;
  border: skyblue 1px solid;
  text-align: center;
  transition: 1s;
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
  overflow: hidden;
  text-overflow: ellipsis; /*溢出用省略号显示*/
  width: 230px;
  margin-left: 16px;
  transition: 0.3s;
}

.listBox:hover .deleteBtn {
  opacity: 1;
}
.listBox:hover .editBtn {
  opacity: 1;
}

.list.completed {
  text-decoration: line-through;
  color: #b8bed1;
}

.deleteBtn,
.editBtn {
  opacity: 0;
  background-color: #b8bed100;
  border: none;
  filter: invert(51%) sepia(94%) saturate(1681%) hue-rotate(189deg)
    brightness(100%) contrast(103%);
}
</style>
