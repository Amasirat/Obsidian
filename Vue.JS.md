Vue is a component-based and declarative [[Javascript]] framework used for rapid frontend development.

In order to start using Vue, you can use a CDN:

```HTML
<script src="https://unpkg.com/vue@3"></script>
```

In order to do anything with it, Vue has to be mounted onto a particular html tag. Once it is mounted, it can only have influence on that particular tag, nothing outside of it.


```HTML
<div id="app">

</div>

<script>
	Vue.createApp({}).mount("#app");
</script>
```

An object is passed into the createApp function of Vue, almost all the magic happens inside there. You can define many functions inside that object.

```Javascript
Vue.createApp({
	data() {
		return {
			greeting: "Hello, World!"
		};
	},
}).mount("#app");
```

```HTML
<div id="app">

{{ greeting }} //innerHTML will become "Hello, World!"

</div>
```

The variables returned by data() are tracked throughout the DOM and once they are changed, Vue will automatically re-render the DOM to display that change.

```HTML
<input type="text" v-model="greeting">
```

The above code will enter the greeting variable's data inside the input tag's value. By changing that value, the value of greeting will also change **in real time**.

Vue has **reactivity** which means it constantly keeps track of the object returned by data() (**the source of truth**), and once it changes it re-renders the page.

mounted() function will be called once it has been mounted.

```Javascript
Vue.createApp({
	data() {
		return {
			greeting: "Hello, World!"
		};
	},

	mounted() {
	//Changes the value of greeting after 3 seconds once it is mounted
		setTimeOut(() => {
			this.greeting = "changed";
		}, 3000);
	}
}).mount("#app");
```

Once greeting changes, the source of truth changes, therefore Vue automatically re-renders the page to reflect that fact.

## Attribute Binding

We can bind source of truth variables to html attributes by the keyword v-bind

```Html
<button v-bind:class="buttonClasses">Click Me</button>

<button :class="buttonClasses">Click Me</button> 
<!-- Or use the short form with a single colon like above -->
<script>

Vue.createApp({
	data() {
		return {
			buttonClasses: "text-green"
		};
	}
	}).mount("#app");

</script>
```

For event-handling, for example when you click on the button the text-green should turn into text-red.

```HTML
<button :class="active ? 'text-red' : 'text-green'" v-on:click="toggle"></button>
<button :class="active ? 'text-red' : 'text-green'" @click="toggle"></button>
<!-- Or use the short form with a single @ like above -->
<script>
Vue.createApp({
	data() {
		return {
			active: false
		};
	},

	methods: {
		toggle() {
			this.active = ! this.active;
		}
	}
	}).mount("#app");

</script>
```

if we have a list in our view, we can dynamically print them using v-for
```HTML
<li v-for="assignment in assignments">{{ assignment.name }}</li>

return {
	assignments: [
		{name: "finish project", complete: false },
		...
	]
}
```

This method is an old way of doing this called the **options API**. It existed from Vue version 1. In Version 3 a **Composition API** is used instead.
## Components




