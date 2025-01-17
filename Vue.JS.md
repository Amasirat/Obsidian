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

mounted() function will also do some things, once view is mounted on an html tag.

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