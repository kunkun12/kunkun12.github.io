layout: blog
title: vue 1.0 的主要变化
date: 2016-01-14 17:02:12
tags:
---
目前工作中还在使用 0.12.x版本，突然发现 1.0在使用上有不小的变化，于是参考 官方的更新日志<https://github.com/vuejs/vue/releases/tag/1.0.0>总结如下
- v-repeat  替换为 v-for
使用方法 `v-for="item in items"` 不能使用 `v-for="imtes"` 可以使用$index来获取当前项在数组中的索引


      <ul id='example-1'>
      <li v-for='item in items'>
            $index {{item.message}}
      </li>
      <ul>

或者  给索引取一个别名 如下 v-for  块中的内容可以使用其父作用域中的属性


        <ul id='example-1'>
        <li v-for='(index,item) in items'>
              {{index}} {{item.message}}
        </li>
        <ul>


     var example=new Vue({
        el:'#example',
        data:{
            items:[
                {message:'Fool'},
                {message:"Bar"}
            ]
        }
        })

- v-on='action:fun' 替换为 v-on:action='func' 缩写 @action='func'，新增了两个事件修饰符号 .prevent与stop

        <button v-on:click="doThis"></button>
        <button v-on:click="doThat('hello', $event)"></button>

        <!-- 缩写 -->
        <button @click="doThis"></button>

        <!-- 停止冒泡 -->
        <button @click.stop="doThis"></button>

        <!-- 阻止默认-->
        <button @click.prevent="doThis"></button>

        <!-- 组织默认的提交表单 -->
        <form @submit.prevent></form>

        <!-- 链式执行绑定的方法并阻止冒泡 -->
        <button @click.stop.prevent="doThis"></button>

        <!-- 绑定键盘事件 -->
        <input @keyup.enter="onEnter">

        <!-- 缩写形式 -->
        <input @keyup.13="onEnter">

- v-attr='attr:val' 替换为 v-bind:attr="val" 缩写 :attr="val"
-  新增 v-else 指令 与 v-if / v-show 成对使用
-  v-el 指令，注册指令 可以在VM中通过 $els 来获得注册DOM的哈希对象
        <span v-el:msg>hello</span>
        <span v-el:other-msg>world</span>

        this.$els.msg.textContent // -> "hello"
        this.$els.otherMsg.textContent // -> "world"
- v-pre 跳过VUE的处理 比如使用 直接显示 `{{name}}` 这样的形式
- v-component指令过时了，建议使用is`<component :is="componentId"></component>` 更多关于 组件的内容参考  <http://vuejs.org/guide/components.html>
-  props 的传递变更为

        <!-- 单向绑定 -->
        <child :msg='parentMsg'></child>  
        <!-- 双向绑定 -->
        <child :msg.sync='parentMsg'></child> 
        <!--  单次绑定 -->
        <child :msg.once='parentMsg'></child>



- v-class and v-style 过时了 推荐使用 `v-bind:class`、`v-bind:style`

      <!-- 切换class-->
      <div v-bind:class="{ 'class-a': true, 'class-b': false }"></div>
      <!-- 应用样式列表 -->
      <div v-bind:class="[ dynamicClass, 'literal-class' ]"></div>
      
      <!-- 接受驼峰式属性 -->
      <div v-bind:style="{ fontSize: '14px', color: 'red' }"></div>
      <!--  支持样式的绑定-->
      <div v-bind:style="[ styleObjectA, styleObjectB ]"></div>

- v-model 支持checkbox与数组绑定 (终于支持了 记得以前使用ng的时候用了[一个插件](https://github.com/vitalets/checklist-model)来完成这个功能

      <input type="checkbox" value="Jack" v-model="checkedNames">
      <input type="checkbox" value="John" v-model="checkedNames">
      <input type="checkbox" value="Mike" v-model="checkedNames">
      被选中的项的value值将会与 checkedNames的数组绑定
-  过滤器 
  1、`debounce` 是空闲时间必须大于或等于 一定值的时候，才会执行调用方法。debounce是空闲时间的间隔控制。比如我们做autocomplete，这时需要我们很好的控制输入文字时调用方法时间间隔。一般时第一个输入的字符马上开始调用，根据一定的时间间隔重复调用执行的方法。

          <input @keyup="onKeyup | debounce 500">
