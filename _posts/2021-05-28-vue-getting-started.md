---
layout: post
title:	"Vue.js Getting Started"
date:	 2021-05-28 00:00:00 +0800
categories: vue javascript framework
---

<div class="alert alert-success d-flex align-items-center" role="alert">
  <svg class="bi flex-shrink-0 me-2" width="24" height="24" role="img" aria-label="Success:"><use xlink:href="#check-circle-fill"/></svg>
  <div class="blogalert">
    Note this sample application can be found on my git repository, check it out <a class="alert-link" href="https://github.com/kdefombelle/vuejs-getting-started">here</a>
  </div>
</div>

Beside Angular and React the colossus frameworks for front-end these days, **Vue.js** is the last born and it learnt from its famous predecessors.<br/>

All these frameworks are great and the choice can be based on the purpose (brand new app, demo, refactoring of a legacy app), the context (team size, degree of collaboration/delegation, practices in place) or simply your taste: in any case you will pick a great framework!

Join me in this article to onboard with **Vue.js**; we will explore the cli, the standard sections of a Vue component, how to exchange information between child and parent components and finally how to use slots to embed html within another piece of html.

**This article has been written using Vue 2** which has the time of the writing is still mainstream and has the widest ecosystem (especially [vuetify](https://vuetifyjs.com/) is not yet compliant with Vue 3).

## Installation
Given you have `node` already installed, otherwise refer to my [post](../../2021/npm-cheat-sheet)
```sh
npm install -g @vue/cli #cli to scaffold your project
```

You can then create your first project
```sh
vue create my-project && cd my-project #keep default settings, especially Vue 2
npm run serve
```

<div class="alert alert-warning d-flex align-items-center" role="alert">
  <svg class="bi flex-shrink-0 me-2" width="24" height="24" role="img" aria-label="Warning:"><use xlink:href="#exclamation-triangle-fill"/></svg>
  <div class="blogalert">
    <strong>Having connectivity issues with cli?</strong><br/>you may want to change to set `"useTaobaoRegistry": false` in your `~/.vuerc` file
  </div>
</div>

## Development Resources
Install VSCode plugin [Vetur](https://marketplace.visualstudio.com/items?itemName=octref.vetur)

Install [Vue Devtools](https://devtools.vuejs.org/) Browser extension or standalone if you are developing on Electron platform)

## Vue Component
At the core of View.js there is the vue component: the `.vue` file.
It is one file made of 3 parts that will be split by the framework later:
- an HTML template
- a script part
- a style part

Each part is examplified separately, but remember it is one unique file
Finally you can also create pieces of code resuable across your components: they are called **mixins**

### HTML Template
```html
<template>
  <div class="content">

    <div v-once><h1>{% raw %}{{ pageTitle }}{% endraw %}</h1></div>

    <!-- Directives -->
    <div class="top">
      <h2>Core Directives</h2>
      <button @click="toggleDisplay()">@click to toggle display</button>
      <div>
        <span v-if="display">Conditional display with <b>v-if</b></span>
        <span v-show="display">Conditional with <b>v-show</b></span>
      </div>
      <div>
      <div><b>v-for</b> example</div>
        <table>
          <tr>
            <th>index</th>
            <th>value</th>
          </tr>
          <tr v-for="(element, index) in collection" :key="index">
            <td>{% raw %}{{ index }}{% endraw %}</td>
            <td>{% raw %}{{ element }}{% endraw %}</td>
          </tr>
        </table>
      </div>
    </div>

    <!-- Child Component -->
    <div class="middle">
      <h2>Child Component</h2>
      <select v-model="selectedColor">
        <option disabled value="">Please select one color</option>
        <option>red</option>
        <option>green</option>
      </select>
      <br/>
      <br/>
      <ChildComponent :color="selectedColor" @childEvent="v => childCounter=v"/>
      <br/>
      <div>Click number <b>{{ childCounter }}</b>
      </div>
    </div>

    <!-- About Styles -->
    <div class="bottom">
      <div>
        <h2>Style Examples</h2>
      </div>
      <button @click="toggleColors()">Toggle borders</button>
      <div :style="borderCssStyle">title <code>:style</code>
      </div>
      <div :style="[borderCssStyle, 'custom-title']">title <code>:style</code> array</div>
      <div :class="{'green-border': green}">title <code>:class</code>
      </div>
      <div :class="[borderCssClass, 'custom-title']">title <code>:class</code> array</div>
    </div>

  </div>

</template>
```

### Script
The script part is the richest as of course this is were will sit most of the logic.

```javascript
import ChildComponent from './ChildComponent.vue'
import HelperMixin from './HelperMixin'

function functionX(v) {
	console.log("functionX: " + v);
}

export default {
	name: "FirstComponent", //best practice: a 2 words name
	created() { //cf. lifecycle diagram https://vuejs.org/v2/guide/instance.html
		console.log('Component created');
	},
	components: {ChildComponent},
	data() { //note data() is a function
		return {
			pageTitle: "FirstComponent",
			childCounter: 0,
			display: true,
			collection: ['element1','element2'],
			selectedColor: "green",
			green: true,
		};
	},
	mixins: [HelperMixin],
	computed: {
			borderCssStyle() {
				return {
					border: this.green ?
						'1px solid '+this.selectedColor :
						'',
				};
			},
			borderCssClass() {
				return this.green ? this.selectedColor+'-border' : '';
			},
	},
	methods: {
		toggleDisplay() {
			functionX("toggleDisplay");
			this.display = ! this.display;
		},
		toggleColors() {
			functionX("toggleColors");
			this.green = ! this.green;
		},
	}
};
</script>
```

### Styles
```css
/* global styles, usually only in App.vue only */
<style>
button{
	color: white;
	background-color: green;
}
</style>

/* scope your style to be applied only in current component */
/* You can use SAAS simply adding lang attribute */
<style scoped>
.content {
	align-content: center;
	background: linear-gradient(to bottom,white, rgb(203, 235, 203));
	background-attachment: fixed;
}
.top {
	align-content: center;
}
.custom-title {
	font-style: italic;
}
.green-border {
	border: 1px solid darkgreen;
}
.red-border {
	border: 1px solid red;
}
table {
	border: 1px solid black;
	display: inline-table;
}
</style>
```

### Mixins
Simply export the code you want to factorise and import that mixin in every component you need it.
e.g.: a lifecycle hook

```javascript
export default {
	updated() {
		console.log('Component updated');
	}
}
```

## Communicating with Child Component
A Child component _ChildComponent_ can be leveraged in a parent one as <code>&lt;ChildComponent/&gt;</code>.
It can also receive **properties** from its parent <br/>
In the other way around the child component can **emit** event to send back information to its parent.
```html
//in parent component
<ChildComponent :color="colorVariable" alignment="left" @childEvent="v => attributeChild1=v"/>
```

Note that `:color` will be bound to a variable whereas `alignment` is the constant value "left" passed to the child, and so does not have the `:`.

```html
<template>
	<div>
		(...)
		<ChildComponent :color="selectedColor" alignment="center" @childEvent="v => childCounter=v"/>
	</div>
</template>

<script>

export default {
	props: ['color','alignment'],
	data() {
		return { attributeChild1: "any content" };
	},
	methods: {
		sendEvent(){
			this.$emit("childEvent", this.attributeChild1);
		}
	}
};

</script>

<style scoped>
.red div {
	color: red;
}
.blue div {
	color: blue;
}
.centered div {
	text-align: center;
}
.left div {
	text-align: left;
}
</style>
```
You can also add some **validation** changing the way you declate the `props`

```javascript
props: {
		color: {
			type: String,
			required: true,
			validator(value) {
						return ['blue', 'green','red'].includes(value);
				}
		},
		alignment: {
			type: String,
			required: true,
		}
	}
},
```

## CSS inheritance
A parent component can (within _scoped_ CSS) define styles that will be applied to a child component,
```html
<div style="color:red">
	<ChildComponent/>
</div>
```
There is a subtle nuance to understand here: _scoped_ style are not aplied out of the scope but CSS inheritance is still there, for instance here color will be inherited but border style for instance would not be as it is not natively inherited in CSS.

Also the parent component can target CSS classes that are in any level down to it with the **deep selector**

```css
.class1 >>> .class2 {	/* .class1 /deep/ .class2 is the syntax for other CSS pre processors
	color: red;
}
```

## Slots
Vue.js slots enables to embed HTML within anoter HTML: the HTML you will provide in the `slot` tag will be surrounded by existing and resuable HTML.

Let's take an example; let's say I have a nice remark div I want to use at several locations in my app to display some remarks.

I can define a _RemarkFrame_ component as follows inclucing a <code>&lt;slot&gt;</code> balise.
This where the content I want to place will be injected to substitute the slot.

```html
<template>
	<div class="remark">
		<slot>Default content</slot> <!-- content can be injected here from another component -->
	<div>
</template>

<script>
export default ({
		name: "RemarkFrame",
		data() {
		},
})
</script>

<style scoped>
.remark {
	padding: 5px;
	border: 0.1px solid grey;
	background-color: #ddffdd;
}
</style>
```

I can then use this _RemarkFrame_ element from another component as follows
```html
<template>
	<RemarkFrane>
		<div>This div comment will be injected in the slot of RemarkFrame</div>
	<RemarkFrane>
	<RemarkFrane/> <!-- with an empty HTML content the slot will show its default content -->
</template>

<script>
import RemarkFrame from "./RemarkFrame.vue"
export default ({
		name: "MyComponent",
		data() {
		},
		components: {RemarkFrame},
})
</script>

<style/>
```

## Directives

<table class="table">
	<colgroup>
		<col width="10%"/>
		<col width="40%"/>
		<col width="50%"/>
	</colgroup>
	<thead class="thead-light">
				<tr>
						<th>Directive</th>
						<th>Description</th>
						<th>Example</th>
				</tr>
	</thead>
	<tbody>
				<tr>
						<td><code>class</code></td>
						<td>Apply conditionally a CSS class</td>
						<td>
								<code>&lt;div :class=&quot;{&apos;red-border&apos;: display}&quot;&gt;Title&lt;/div&gt;</code><br/>
								<code>&lt;div :class=[borderCssClass, more]&quot;&gt;Title&lt;/div&gt;</code>
						</td>
				</tr>
				<tr>
						<td><code>style</code></td>
						<td>Apply a parameterised style
						</td>
						<td>
								<code>&lt;div :style=&quot;borderStyle&quot;&gt;Title&lt;/div&gt;</code><br/>
								<code>&lt;div :style=&quot;[borderStyle, more]&quot;&gt;Title&lt;/div&gt;</code>
						</td>
				</tr>
				<tr>
						<td><code>v-bind</code></td>
						<td>Bind a <i>variable</i> (similar to interpolation with moustache expressions)</td>
						<td>
								<code>&lt;img v-bind:src=&quot;&quot;&gt;&lt;/image&gt;</code><br/>
								<code>&lt;img :src=&quot;&quot;&gt;&lt;/image&gt;</code>
						</td>
				</tr>
				<tr>
						<td><code>v-for</code></td>
						<td>Iterate over an array<br/>
								Do not combine with <code>v-if</code> on same te same element (perf warning)
						</td>
						<td>
								<code>&lt;tr v-for=&quot;(element, index) in collection&quot; :key=&quot;index&quot;&gt;</code>
						</td>
				</tr>
				<tr>
						<td><code>v-if</code></td>
						<td>Conditional display: adding/removing from DOM</td>
						<td>
								<code>&lt;span v-if=&quot;display&quot;&gt;Conditionallay&lt;/span&gt;</code>
						</td>
				</tr>
				<tr>
						<td><code>v-model</code></td>
						<td>Create a two way bindings between an <i>html element value</i>and a <i>variable</i></td>
						<td>
								<code>&lt;input v-model=&quot;message&quot; placeholder=&quot;edit me&quot;&gt;</code>
						</td>
				</tr>
				<tr>
						<td><code>v-on</code></td>
						<td>Bind to a <i>method</i> or an <i>event</i></td>
						<td>
								<code>&lt;button v-on:click=&quot;method1()&quot;&gt;Click&lt;/button&gt;</code>
								<code>&lt;button @click=&quot;method1()&quot;&gt;Click&lt;/button&gt;</code>
						</td>
				</tr>
				<tr>
						<td><code>v-once</code></td>
						<td>Display once only a value not expected to change</td>
						<td>
								<code>&lt;div v-once class=&quot;title&quot;&gt;{{ pageTitle }}&lt;/div&gt;</code>
						</td>
				</tr>
				<tr>
						<td><code>v-show</code></td>
						<td>Conditional display: via <code>styling display</code><br/>
								To privilege over <code>v-if</code> if the content is costly to render
						</td>
						<td>
								<code>&lt;span v-show=&quot;display&quot;&gt;Conditional&lt;/span&gt;</code>
						</td>
				</tr>
	</tbody>
</table>
