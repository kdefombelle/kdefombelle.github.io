---
layout: post
title:	"Vue.js Routing"
date:	 1999-05-28 00:00:00 +0800
categories: vue javascript framework
---

<div class="alert alert-success d-flex align-items-center" role="alert">
  <svg class="bi flex-shrink-0 me-2" width="24" height="24" role="img" aria-label="Success:"><use xlink:href="#check-circle-fill"/></svg>
  <div class="blogalert">
    Note this sample application can be found on my git repository, check it out <a class="alert-link" href="https://github.com/kdefombelle/vuejs-routing">here</a>
  </div>
</div>

Vue.js is a great and trending framework.
If you followed my first article [Vue.js Getting Started](../../2021/vue-getting-started) you have touched already how it is simple to scaffold and start developping a front end application with rich HTML and styling. 

In this second chapter on Vue.js we will discuss the **routing capability**.

## Installation
You can scaffold a new app to test **Vue router** or use one you already have:

```sh
vue create my-project && cd my-project #keep default settings, especially Vue 2
npm install vue-router --save
npm run serve
```

## Set up you App for Routing
Create a file `router/index.js` in your project
```javascript
import Vue from 'vue';
import Router from 'vue-router';
import HomePage from '../home/HomePage.vue';
import FirstComponent from '../home/FirstComponent.vue';

Vue.use(Router);

export default new Router(
  {
    routes: [
      {
        path: '/',
        name: 'Home',
        component: HomePage,
      },
      {
        path: '/first',
        name: 'First',
        component: FirstComponent,
      }
    ],
  }
);
```
in `App.vue` add the `<router-view>` tag and remove any reference to existing component; you will serve any component leveraging the routing
```html
<template>
  <div>
    <main>
      <router-view />
    </main>
  </div>
</template>
```

Finally in `main.js` reference the `Router`
```javascript
import Vue from 'vue'
import App from './App.vue'
import router from './router'; // here is the import of the router

Vue.config.productionTip = false

new Vue({
  render: h => h(App),
  router, // here is the use of the router
}).$mount('#app')
```
Now you can run your app and it will already route to the home page of your application.

## Navigating
Time now to enrich a tad our routes in `router/index.js`
```javascript
export default new Router(
  {
    routes: [
      {
        path: '/',
        name: 'Home',
        component: HomePage,
      },
      {
        path: '/first',
        name: 'First',
        component: FirstComponent,
      },
      {
        path: '/second/:id',
        name: 'Second',
        component: SecondComponent,
      },
			{
        path: '/third',
        name: 'Third',
        component: ThirdComponent,
        props: true,
      }
    ],
  }
);
```

One can navigate from one page to another with `<router-link>` path as follows
```html
<div class="get-started">
  <router-link to="/first">Go to First Component using its path</router-link>
</div>
```

The same can be achieved **programatically** 
-  using the **route path**

```html
<button @click="goTo()">Go to programatically</button>
```

```javascript
goTo: function(){
	this.$router.push('/first');
}
```

-  passing **parameters**

```html
<button  @click="goToWithParams()">Go to Second Component programatically with params</button>
```

```javascript
goToWithParams: function(){
	this.$router.push({
		name: 'Second', 
		params: {
			id: 2, 
			}
		});
}
```

that you can capture
```javascript
computed: {
    luckyNumber() {
      return this.$route.params.id;
    },
  },
```

- passing **props**

```javascript
goToWithProps: function(){
	this.$router.push({
		name: 'Third', 
		params: {
			callerMessage: "Message from Home", 
			callerId: "3", 
			}
		});
}
```

that you can capture
```javascript
computed: {
	propsMessage() {
		const {callerMessage, callerId} = this;
		return callerMessage + " - " + callerId;
	}
},
```

## Styling a Router Link
```html
<router-link :to="{name: 'Home'}" exact>>
	<img class="logo" src="./assets/logo.png"/>Home
</router-link>
```

using css class
```css
<style>
.router-link-active {
  color: orchid;
}
</style>
```

## Nested Views
Finally you may want to have a display area that is replaced depending your navigation while keeping the overall layout: this is what **nested views** are for.
You will simply insert another `<router-view>` component.
For instance if you are on a page _BrowseViews_ you reached via the usual routing, you can define children pages that you can be displayed within an insert depending on your click on `/innerView1` or `/innerView2` links.

```html
<template>
  <div class="content">
    <div>
      <div>
        <h1>Browse Views</h1>
      </div>
      <div>
        <router-link to="/innerView1">Nice - Negresco</router-link> |
        <router-link to="/innerView2">NIce - Vielle ville</router-link>
      </div>
      <router-view></router-view> <!-- only the content here will be replaced -->
    </div>
  </div>
</template>

<script>
export default {
  name: "BrowseViews",
};
</script>

<style scoped>
.content {
  display: flex;
  justify-content: center;
}
</style>
```

You can define the above links and the corresponding views for, e.g.:
```html
<template>
	<div>
    <h1>Nice - Negresco</h1>
    <img src="../assets/nice_negresco.jpg"/>
  </div>
</template>


<script>
export default {
  name: 'InnerView1',
};
</script>
```

And to activate this routing configure your routing as follows:
```javascript
export default new Router(
  {
    routes: [
      {
        path: '/',
        name: 'Home',
        component: HomePage,
      },
	  {
        path: '/browse',
        name: 'BrowseViews',
        component: BrowseViews,
        children: [
          {
            path: '/innerView1',
            name: 'InnerView1',
            component: InnerView1,
          },
          {
            path: '/innerView2',
            name: 'InnerView2',
            component: InnerView2,
          }
        ]
      }
    ],
  }
);
```
