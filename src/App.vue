<template>
  <section class="todoapp">

    <header class="header">
      <h1>待办事项</h1>
      <input @keyup.enter="addTodo" class="new-todo" placeholder="需要做什么？" autofocus>
    </header>

    <!-- 这个部分应该默认隐藏，并且在有代办事项（todos）时显示 -->
    <section class="main">
      <input id="toggle-all" class="toggle-all" type="checkbox" @click="toggleAll">
      <label for="toggle-all">Mark all as complete</label>
      <ul class="todo-list">
        <!-- 当处于编辑状态时，列表项应该添加 `editing` 类，当被标记为已完成时，应该添加 `completed` 类 -->
        <li v-for="todo in filteredTodos" :key="todo.id" :class="{ completed: todo.completed }">
          <div class="view">
            <!-- 使用 `v-model` 进行双向绑定，搭配动态绑定类名可以达到联动效果 -->
            <input class="toggle" type="checkbox" v-model="todo.completed">
            <label @dblclick="todo.isEdit = true">
              <span v-show="!todo.isEdit">{{ todo.title }}</span>
              <input class="edit-input" v-show="todo.isEdit" type="text" v-model="todo.title"
                @keyup.enter="todo.isEdit = false; todo.title = todo.title ? todo.title : '空'" />
            </label>
            <button class="destroy" @click="removeTodo(todo)"></button>
          </div>
          <input class="edit" value="Create a TodoMVC template">
        </li>
      </ul>
    </section>

    <!-- 这个底部栏应该默认隐藏，并且在有代办事项（todos）时显示 -->
    <footer v-show="todos.length != 0" class="footer">
      <!-- 这里默认应该显示为 `0 项剩余` -->
      <span class="todo-count">
        <strong> {{ remaining }}</strong>
        项剩余
      </span>
      <!-- 如果你没有实现URL的哈希值功能，就移除这部分 -->
      <ul class="filters">
        <li>
          <a :class="{ selected: visibility === '' }" href="#/">所有</a>
        </li>
        <li>
          <a :class="{ selected: visibility === 'active' }" href="#/active">未完成</a>
        </li>
        <li>
          <a :class="{ selected: visibility === 'completed' }" href="#/completed">已完成</a>
        </li>
      </ul>
      <!-- 如果没有已完成的项，则隐藏这部分 ↓ -->
      <button class="clear-completed" @click="clearCompleted">清除已完成</button>
    </footer>

  </section>
</template>

<style scoped>
/* 只是导入外部样式，本项目主要内容不在此 */
@import "https://unpkg.com/todomvc-app-css@2.4.1/index.css";

.edit-input {
  font-size: 24px;
  border: 0;
  padding: 0;
}
</style>

<script>
const hashStatus = [
  '',           // 全部
  'active',     // 未完成
  'completed'   // 已完成
];

export default {
  data() {
    return {
      // 显示在页面中的待办项列表，可以通过 `addTodo` 函数添加项，以及 `removeTodo` 函数删除项，且显示内容会动态变化
      todos: [
        {
          id: 114,
          title: '出发游玩舞萌DX',
          completed: false,
          isEdit: false
        },
        {
          id: 514,
          title: '在家游玩Arcaea',
          completed: true,
          isEdit: false
        }
      ],
      // 表示当前URL的哈希值
      visibility: ''
    }
  },
  methods: {
    // 实现全选/全不选的操作
    toggleAll(event) {
      this.todos.forEach(todo => todo.completed = event.target.checked);
    },
    // 判断URL的哈希值，当URL的哈希值变化时改变需要激活的元素
    onHashChange() {
      const hash = window.location.hash.replace(/#\/?/, '');  // 匹配 `#/` 并置换为空
      if (hashStatus.includes(hash)) {
        this.visibility = hash;
      } else {
        // 默认访问主页
        window.location.hash = ''
        this.visibility = '';
      }
    },
    // 添加todo
    addTodo(event) {
      const title = event.target.value.trim();  // trim() 去除空格
      if (!title) {
        return;
      }
      this.todos.push({
        id: Date.now(),   // 使用时间戳可以保证id不重复，最好使用随机值代替
        title,
        completed: false,
        isEdit: false
      })
      event.target.value = ''
    },
    // 移除单个todo
    removeTodo(todo) {
      this.todos.splice(this.todos.indexOf(todo), 1);
    },
    // 清除已完成
    clearCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed)
    }
  },
  computed: {
    // 根据URL的哈希值提供的筛选条件返回一个被筛选的todos列表
    filteredTodos() {
      switch (this.visibility) {
        case '':
          return this.todos
        case 'active':
          return this.todos.filter(todo => !todo.completed);
        case 'completed':
          return this.todos.filter(todo => todo.completed);
      }
    },
    // 未完成项目的个数
    remaining() {
      return this.todos.filter(todo => !todo.completed).length;
    }
  },
  mounted() {
    // 添加监听器，当URL的哈希值改变时执行特定函数
    window.addEventListener('hashchange', this.onHashChange);
    // 主动调用一次，为了避免刷新后的显示问题。
    this.onHashChange();
  }
}
</script>
