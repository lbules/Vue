<template>
  <div class="task-container">
    <ul class="task-list">
      <!-- 任务项 -->
      <li
        :class="{ 'task-item': true, 'finish-item': item.isDone }"
        v-for="item in tasklist"
      >
        <!-- 勾选 -->
        <span
          ><input
            v-show="!item.isDone"
            type="checkbox"
            @click="select(item.id, finish)"
        /></span>
        <!-- 内容 -->
        <span class="task-content">{{ item.content }}</span>
        <!-- 删除 -->
        <!-- <span class="detail" @click="select(item.id, deatil)">详情</span> -->
        <span class="delete" @click="select(item.id, deleteItem)">删除</span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "TodoList",
  props: ["tasklist"],
  data() {
    return {
      finish: "finish",
      deleteItem: "deleteItem",
      deatil: "deatil",
    };
  },
  methods: {
    //   删除当前任务
    select(id, type) {
      this.$emit("select", id, type);
      // 取消checkbox的选择
      event.target.checked = false;
      console.log(id, type);
    },
  },

  mounted() {
    //  console.log(this.tasklist);
  },
};
</script>


<style scoped>
ul {
  padding: 0;
  margin: 0;
  list-style: none;
}

.task-list {
  margin: 10px;
}

.task-item {
  padding: 10px;
  background-color: #fff;
  color: #333;
  margin-bottom: 5px;
  border-radius: 3px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.2);
  cursor: pointer;
  transition: background 0.5s;
}

.task-item:hover {
  background-color: #ddd;
}

.detail,
.delete {
  float: right;
  padding-left: 5px;
  color: #333;
  cursor: pointer;
}

.detail:hover,
.delete:hover {
  color: red;
}

.finish-item {
  text-decoration: line-through;
  background-color: #ddd;
}
</style>