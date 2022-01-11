# Vue内置指令

## v-text
更新元素的内容(文本)，无论元素内是否有内容，都直接覆盖
```html
<h1 v-text="message"></h1>
```
如需局部替换，可使用`{{}}`
```html
<h1>Welcome to {{message}}</h1>
```
这里`v-text`指令只会将纯文本替换上去，即便message中存储着一段HTML语句，Vue也会原样输出；若message中存储着一段HTML语句并且想让浏览器进行解析，那么请参考下方`v-html`

这其中使用到的message需要在Vue实例data中存在(参考上一章写法)，此处省略了

## v-html
`v-html`与`v-text`有所不同的是，`v-html`会将内容按照HTML的形式进行插入，也就是说浏览器会将插入的文本当作HTML文档进行处理(其中的标签等都会被浏览器解析)
```html
<div v-html="html-msg"></div>
```
```javascript
new Vue({
	el: "#root",
	data: {
		html-msg: "<h1>HelloWorld</h1>"
	}
})
```
效果：
<h1>HelloWorld</h1>

这样能很方便的插入一段能让浏览器进行解析的HTML语句，但这也同样存在着风险，比如存在XSS攻击的风险。
**在有需要用户提交的地方不要使用`v-html`**


## v-show
表达式为真，此元素显示，否则隐藏。这里的显示与隐藏是CSS层面的，元素仍然保留在dom树中。频繁显示隐藏时推荐使用。
```html
<h1 v-show="isTrue">测试文字</h1>
```

## v-if
表达式为真(这里的真值可以是其他，不仅仅是true)时，此元素呈现，否则直接清掉，v-if会操作dom树，会触发重排。
```html
<h1 v-if="isTrue">测试文字</h1>
```

## v-else
与前方`v-if`配合使用，前方元素不呈现时，此元素呈现。
```html
<h1 v-if="isTrue">测试文字1</h1>
<h1 v-else>测试文字1未呈现</h1>
```

## v-else-if
与前方`v-if`、`v-else-if`配合使用
```html
<div v-if="msg==='A'">A</div>
<div v-else-if="msg==='B'">B</div>
<div v-else-if="msg==='C'">C</div>
<div v-else>ABC都不是</div>
```

## v-for
可基于源数据渲染出多个模板块

对于数组：
```html
<div v-for="(value, index) in array"></div>
<!-- value:数据值 index:脚标 -->
```

对于对象：
```html
<div v-for="(value, key-name, index) in object"></div>
<!-- value:数据值 key-name:键名 index:脚标 -->
```	

## v-on
用于绑定事件

缩写：@
```html
<!-- 方法处理器 -->
<button v-on:click="doThis"></button>

<!-- 动态事件 (2.6.0+) -->
<button v-on:[event]="doThis"></button>

<!-- 内联语句 -->
<button v-on:click="doThat('hello', $event)"></button>

<!-- 缩写 -->
<button @click="doThis"></button>

<!-- 动态事件缩写 (2.6.0+) -->
<button @[event]="doThis"></button>

<!-- 停止冒泡 -->
<button @click.stop="doThis"></button>

<!-- 阻止默认行为 -->
<button @click.prevent="doThis"></button>

<!-- 阻止默认行为，没有表达式 -->
<form @submit.prevent></form>

<!--  串联修饰符 -->
<button @click.stop.prevent="doThis"></button>

<!-- 键修饰符，键别名 -->
<input @keyup.enter="onEnter">

<!-- 键修饰符，键代码 -->
<input @keyup.13="onEnter">

<!-- 点击回调只会触发一次 -->
<button v-on:click.once="doThis"></button>

<!-- 对象语法 (2.4.0+) -->
<button v-on="{ mousedown: doThis, mouseup: doThat }"></button>
```

## v-bind
通常用来绑定属性

缩写`:`

```html
<!-- 绑定一个 attribute -->
<img v-bind:src="imageSrc">

<!-- 动态 attribute 名 (2.6.0+) -->
<button v-bind:[key]="value"></button>

<!-- 缩写 -->
<img :src="imageSrc">

<!-- 动态 attribute 名缩写 (2.6.0+) -->
<button :[key]="value"></button>

<!-- 内联字符串拼接 -->
<img :src="'/path/to/images/' + fileName">

<!-- class 绑定 -->
<div :class="{ red: isRed }"></div>
<div :class="[classA, classB]"></div>
<div :class="[classA, { classB: isB, classC: isC }]">

<!-- style 绑定 -->
<div :style="{ fontSize: size + 'px' }"></div>
<div :style="[styleObjectA, styleObjectB]"></div>

<!-- 绑定一个全是 attribute 的对象 -->
<div v-bind="{ id: someProp, 'other-attr': otherProp }"></div>

<!-- 通过 prop 修饰符绑定 DOM attribute -->
<div v-bind:text-content.prop="text"></div>

<!-- prop 绑定。“prop”必须在 my-component 中声明。-->
<my-component :prop="someThing"></my-component>

<!-- 通过 $props 将父组件的 props 一起传给子组件 -->
<child-component v-bind="$props"></child-component>

<!-- XLink -->
<svg><a :xlink:special="foo"></a></svg>
```

## v-model
输入类元素数据双向绑定

可用在`<input>` `<select>` `<textarea>`标签上

### 表单输入绑定
**文本**(textarea同理)
```html
<input v-model="textMsg">
<h2>用户输入的是{{textMsg}}</h2>
```

**复选框**

单个复选框：
```html
<input type="checkbox" id="check1" v-model="checkdata">
<label for="check1">{{checkdata}}</label>
```

多个复选框：

应当绑定至数组
```html
<input type="checkbox" id="ck01" v-model="ckArr" value="banana">
<label for="ck01">Banana</label>
<input type="checkbox" id="ck02" v-model="ckArr" value="apple">
<label for="ck02">apple</label>
<input type="checkbox" id="ck03" v-model="ckArr" value="orange">
<label for="ck03">orange</label>
<input type="checkbox" id="ck04" v-model="ckArr" value="lemon">
<label for="ck04">lemon</label>
<hr/>
<p>用户选择的有：{{ckArr}}</p>
```

```javascript
new Vue({
	el: "#root",
	data: {
		ckArr: []
	}
})
```
**单选按钮**
```html
<div>
	<input type="radio" id="male" value="male" v-model="picked">
	<label for="male">male</label>
	<input type="radio" id="female" value="female" v-model="picked"> 
	<label for="female">female</label>
	<h2>picked: {{picked}}</h2>
	</div>
</div>
```

```javascript
new Vue({
	el: "#root",
	data: {
		picked: ""
	}
})
```

**下拉菜单**

单选时：

```html
<div id="root">
  <select v-model="selected">
    <option disabled value="">请选择</option>
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <p>用户选择的有：{{ selected }}</p>
</div>
```

```javascript
new Vue({
	el: "#root",
	data: {
		selected: ""
	}
})
```

多选时：

```html
<div id="root">
  <select v-model="selecteds" multiple style="width: 50px;">
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <br>
  <p>用户选择的有：{{ selecteds }}</p>
</div>
```

```javascript
new Vue({
	el: "#root",
	data: {
		selecteds: []
	}
})
```