---
title: Spring环境搭建
categories:
- Java
- Spring
tags:
- Java
- Spring
---

## Spring环境搭建

<!--more-->

### 1. 环境搭建步骤

1. 创建工程（Project&Module）
2. 导入静态页面
3. 导入所需坐标
4. 创建包结构（controller、service、dao、domain、utils）
5. 导入数据库脚本
6. 创建 POJO 类
7. 创建配置文件

### 2. 角色列表展示的步骤

1. 点击角色管理菜单发送请求到服务端
2. 创建 RoleController 和 showList() 方法
3. 创建 RoleService 和 showList() 方法
4. 创建 RoleDao 和 findAll() 方法
5. 使用 JdbcTemplate 完成查询操作
6. 将查询数据存储到 Model 中
7. 转发到 role-list.jsp 页面进行展示

### 3. 角色添加的步骤

1. 点击列表页面新建按钮跳转到角色添加页面
2. 输入角色信息，剪辑保存按钮，表单数据提交服务器
3. 编写 RoleController 的 save() 方法
4. 编写 RoleService 的 save() 方法
5. 编写 RoleDao 的 save() 方法
6. 使用 JdbcTemplate 保存 Role 数据到 sys_role
7. 跳转回角色列表页面

### 4. 用户登录权限控制

用户没有登录的情况下，不能对后台菜单进行访问操作，点击菜单跳转到登陆页面，

只有用户登录成功后才能进行后台功能的操作。