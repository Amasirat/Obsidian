CSS is made up of different properties that are applied on [[HTML]] tags.


## Box Model

In CSS, the term *Box Model* is basically a box that wraps around every HTML tag. It consists of properties like: content, padding, margin, and borders. Every HTML tag has these properties.

```HTML
<style>
div {
	padding: 10px;
	border-width: 10px
	border-style: solid;
}

</style>
```
## Displays

We can change a displays (block or inline functionality) by specifying the display property.
For example we can change the default block behaviour of the p tag like this:

```HTML
<p style="display: inline;">Hello</p>
```


## Selectors

CSS selectors are used to *find* html tags. We can not write every CSS properties of html tags inline, so there needs to be a mechanism to detach CSS from HTML for readability's sake.

* **Simple Selectors**: select based on name, class, id, etc
* **Combinator selectors**: select elements based on relationships
* **Pseudo-elements selectors**
* **Attribute selectors**
#### Combinator Selectors

* decendant selector:
```CSS
div p{
....
}
```

* child selector:

```CSS
div > p {
....
}
```

* adjacent sibling selector:

```CSS
div + div {
....
}
```

* general sibling selector:
```CSS
div ~ div {
...
}
```



## Positions

