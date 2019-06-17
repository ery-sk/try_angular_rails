# Railsアプリケーションに後から部分的にAngularを導入する

```sh
ruby -v
# ruby 2.6.3p62 (2019-04-16 revision 67580) [x86_64-darwin17]

rails -v
# Rails 5.2.3

bundler -v
# Bundler version 1.17.2

node -v
# v12.4.0

yarn -v
# 1.16.0
```

## 普通にアプリケーションを作成する

```sh
rails new . -T -O
rails g controller hello index
```
```rb
# config/routes.rb
Rails.application.routes.draw do
  root 'hello#index'
end
```
```sh
# 確認
bin/rails s
```

## 適当なjQueryを使う
```sh
echo "gem 'jquery-rails'" >> Gemfile
bundle
```
```js
//= require jquery
```
```html
<%= button_tag 'Hello', type: 'button', id: 'hello-jquery' %>
```
```coffee
$ ->
  $('#hello-jquery').click ->
    alert('Hello jQuery')
```
```sh
# 確認
bin/rails s
```

## WebpackerとAngularの導入
```sh
echo "gem 'webpacker'" >> Gemfile
bundle
rails webpacker:install
rails webpacker:install:angular
```
```sh
ls bin/
# bundle			setup			webpack
# rails			spring			webpack-dev-server
# rake			update			yarn
```
```diff
"dependencies": {
  - "core-js": "^3.1.4",
  + "core-js": "^2.5.7",
}
```
```sh
yarn install
```

## jQueryとAngularの共存確認

```html
<!-- app/views/layouts/application.html.erb -->
<%= javascript_pack_tag 'application', 'data-turbolinks-track': 'reload' %>
<!-- app/views/hello/index.html.erb -->
<hello-angular>Loading...</hello-angular>
<%= javascript_pack_tag 'hello_angular' %>
```

起動

```sh
$ bin/webpack-dev-server
$ bin/rails s
```
<img width="1280" alt="angure_index" src="https://user-images.githubusercontent.com/38872854/59588063-b51c7480-9121-11e9-8dd8-46a04986df5d.png">

🎉🎉🎉🎉🎉🎉

コンパイル時に警告が出たり、最新のcore-jsが使えなかったり  
使っている内にハマる危険がある
