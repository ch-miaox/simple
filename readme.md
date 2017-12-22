
<p align="center">
此项目为Laravel学习的第一个入门项目

教程：<a href="https://fsdhub.com/books/laravel-essential-training-5.5">Laravel 教程 - Web 开发实战入门 ( Laravel 5.5 )</a>

开发平台：Mac

运行环境：Homestead
</p>

通过本项目的学习，主要对Laravel项目开发流程有了熟悉，以下是个人对Laravel项目最简开发过程的总结：

##1.New project
```java
composer create-project laravel/laravel sample --prefer-dist -vvv
```
##2.Set project
Edit .env file
```java
DB_DATABASE=db_name
```
Edit hosts file
```java
Add 192.168.10.10   project_name.test to etc\hosts
```
Edit Homestead.yaml file
```java
Add
  - map: project_name.test
  to: /home/vagrant/Code/project_name/public
to sites
Add
  - project_db_name
to databases
```
Then reload Homestead
```java
vagrant provision && vagrant reload
```
##3.Generate app key(Sometimes it will be init auto)
```java
php artisan key:generate
```
##4.Create db
```java
//database/migrations/[timestamp]_create_users_table.php
php artisan make:migration create_users_table --create="users"
php artisan migrate
```
##5.Create Model
```java
mkdir app/Models
php artisan make:model Models/User
```
##6.Create controller and create route action
```java
php artisan make:controller UsersController

public function index()
{
  $users = User::paginate(10);
  //跳转到resources/views/users/index_blade.php,参数users
  return view('users.index', compact('users'));
}
```
##7.Create route view
```java
resources/views/users/create.blade.php

@foreach ($users as $user)
  {{ $user->name }}
@endforeach
```
##8.Optimize view style
```java
1.Install bootstrap
yarn install --no-bin-links
2.Edit app.scss
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap";
3.Build app.scss
npm run dev
npm run watch-poll
4.Import style
<link rel="stylesheet" href="/css/app.css">
```
##9.Set routes
```java
//显示所有
//对应controller方法:index()
Route::get('/users', 'UsersController@index')->name('users.index');
//显示详情
//对应controller方法:show(User $user)
Route::get('/users/{user}', 'UsersController@show')->name('users.show');
//创建信息的页面
//对应Controller方法:create()
Route::get('/users/create', 'UsersController@create')->name('users.create');
//创建操作
//对应Controller方法:store(Request $request)
Route::post('/users', 'UsersController@store')->name('users.store');
//编辑信息的页面
//对应Controller方法:edit(User $user)
Route::get('/users/{user}/edit', 'UsersController@edit')->name('users.edit');
//更新操作
//对应Controller方法:update(User $user, Request $request)
Route::patch('/users/{user}', 'UsersController@update')->name('users.update');
//删除操作
//对应Controller方法:destroy(User $user)
Route::delete('/users/{user}', 'UsersController@destroy')->name('users.destroy');
```
##Join it!
