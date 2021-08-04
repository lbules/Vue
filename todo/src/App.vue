<template>
  <div id="app">
    <div class="container">
      <h1>My TODO</h1>
      <form class="add-task">
        <input
          type="text"
          placeholder="请输入"
          class="input-content"
          v-model="content"
        />
        <button type="submit" class="add-btn" @click="addtask">添加</button>
      </form>

      <todo-list :tasklist="tasklist" @select="select" />

      <!-- 蒙层 -->
      <div class="mask" v-if="showDelete"></div>

      <div class="alert" v-if="showDelete">
        <p>是否删除？</p>
        <button class="comfirm" @click="delComfirm">确定</button>
        <button class="cancel" @click="delCancel">取消</button>
      </div>
    </div>
  </div>
</template>

<script>
import TodoList from "./components/TodoList.vue";

export default {
  name: "App",
  components: { TodoList },

  data() {
    return {
      tasklist: [], // 任务列表
      content: "", // 输入的内容
      showDelete: false, // 显示弹出框
      comfirm: false,
      index: null,
    };
  },

  methods: {
    addtask() {
      // 先放到任务列表的数组里
      this.tasklist.push({
        id: this.tasklist.length,
        isDone: false,
        content: this.content,
      });
      // 保存到本地
      this.$storeLocal.set("local_task", this.tasklist);
      // 清空
      this.content = "";
      this.init()
    },

    select(id, type) {
      // 获取id相等的索引值
      let index = this.tasklist.indexOf(
        this.tasklist.find((item) => item.id === id)
      );

      if (type === "deleteItem") {
        this.showDelete = true; //显示弹出窗口
        this.index = index;
      } else if (type === "finish") {
        // 完成任务
        this.tasklist[index].isDone = true;
        this.$storeLocal.set("local_task", this.tasklist);
        // 更新完成任务后的排序
        this.init()
      } else {
        // 查看任务详情
      }
    },

    // 弹出框
    delComfirm() {
      //删除指定下标的元素
      this.tasklist.splice(this.index, 1);
      this.$storeLocal.set("local_task", this.tasklist);
      this.showDelete = false;
    },

    delCancel() {
      this.showDelete = false;
    },

    init() {
      this.tasklist = this.initOrder(this.$storeLocal.get("local_task") || []);
    },

    // 对任务进行排序，已完成的在后面
    initOrder(list) {
      console.log("开始排序了！");
      const finish = list.filter((item) => item.isDone);
      const unfinish = list.filter((item) => !item.isDone);
      return unfinish.concat(finish);
    },
  },

  mounted() {
    this.init();
  },
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
}

ul {
  list-style: none;
}
h1 {
  text-align: center;
  color: #fff;
}
.container {
  position: relative;
  max-width: 900px;
  margin: 50px auto;
  padding-bottom: 10px;
  padding-top: 10px;
  background-color: rgb(250, 121, 121);
}

/* 输入和添加任务 */
.add-task {
  overflow: hidden;
  margin-top: 10px;
}

.input-content {
  float: left;
  margin-left: 10px;
  width: 82%;
  margin-right: 5px;
  border: 0;
  outline: none;
  padding: 10px;
  border-radius: 3px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.2);
}
.add-btn {
  float: left;
  box-sizing: border-box;
  width: 13%;
  padding: 10px;
  color: #333;
  background: #fff;
  border: 0;
  border-radius: 3px;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.2);
  transition: background 0.4s;
}

.add-btn:hover {
  background-color: #ddd;
}

.alert {
  position: absolute;
  z-index: 101;
  box-sizing: border-box;
  top: 50%;
  left: 50%;
  text-align: center;
  padding-top: 10px;
  transform: translate(-50%, -50%);
  border-radius: 5px;
  width: 200px;
  height: 100px;
  box-shadow: 2px 2px 3px rgba(0, 0, 0, 0.2);
  background-color: #fff;
}

.alert button {
  margin-top: 30px;
  padding: 5px;
  color: #fff;
  border: 0;
  border-radius: 2px;
}

.comfirm {
  background-color: rgb(111, 157, 241);
  margin-right: 50px;
}

.cancel {
  background-color: rgb(241, 74, 74);
}

.mask {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 100;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.2);
}
</style>
