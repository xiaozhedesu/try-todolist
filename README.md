# try-todolist

## 简介

这是一个vue2练习项目，**并非原创**。

> 视频来源：[20分钟学会Vue综合案例 快速巩固复习Vue语法 大学生暑假必学前端编程项目](https://www.bilibili.com/video/BV1714y167Sn/?spm_id_from=333.1007.top_right_bar_window_history.content.click&vd_source=b9ad880f6d5b2c87a5e2200233689d26)
> 使用资源： [todomvc-app-template](https://github.com/tastejs/todomvc-app-template/tree/main)

## 过程记录

### 步骤

0. 初始化一个vue2项目，然后把关于todoMVC的模版html和css导入`App.vue`文件中，准备编写功能代码。
1. 创建一个响应式数据`todos`，用来存放待办事项。
2. 在html部分中，对`li`使用`v-for`遍历`todos`数据用来展示。
3. 在`methods`里实现`toggleAll(event)`达到点击控件时实现全选/全不选的效果。
4. 对于`footer`中的分类展示，在生命钩子添加一个监听器，当URL的哈希值发生变化时调用`onHashChange()`函数（因为分类是通过URL的哈希值来决定显示的类别的）。
5. 添加一个响应式数据`visibility`，用来储存当前需要展示的待办事项类别，并实现`onHashChange()`函数，这个函数用来根据URL的哈希值改变`visibility`。
6. 添加一个计算属性`filteredTodos`, 保存根据`visibility`的状态筛选后的数组，并将展示页面遍历`todos`改为遍历`filteredTodos`。
7. 在`footer`里的`filters`无序列表，添加动态类名来达到切换页面改变显示样式的效果。
8. 添加一个计算属性`remaining`，表示列表中未勾选的项目个数，然后在页面中显示。
9. 实现一个函数`addTodo(event)`，实现通过输入框新增数据，然后在输入框上使用`keyup.enter`监听回车来决定添加文件的时机。
10. 实现一个函数`removeTodo(todo)`，实现删除某个待办项，然后通过`click`监听实现效果。
11. 实现一个`clearCompleted()`，实现删除所有完成项，然后通过`click`监听实现效果。

### 总结
视频以「保存并显示待办项（1-2） -> 批量改变选择状态（3）-> 分类展示待办项（4-7） -> 其他小功能（8-11）」的顺序带领学习者学习vue2的功能特性。能流畅做完并理解这个项目，或许能说明学习者已经初步掌握vue2的基础知识。