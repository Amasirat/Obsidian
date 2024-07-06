CSS is made up of different properties that are applied on [[HTML]] tags.
## Box Model

In CSS, the term *Box Model* is basically describing a box that wraps around every HTML tag. It consists of properties like: content, padding, margin, and borders. Every HTML tag has these properties.

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

We can change a tag's display (block or inline functionality) by specifying the display property.
For example we can change the default block behavior of the p tag like this:

```HTML
<p style="display: inline;">Hello</p>
```

Inline tags do not accept height or width properties, block tags do
**However, inline-block tags accept both height or width properties.

* block tags have width set to auto which takes up the entire width of the device viewing the web-page.
* Inline-block tags set width to auto too however their width is only as much as their contents.

## Typography

Properties that relate to fromating text
* font-weight: {normal, bold, ...}
* font-style: {italic, normal, }
* text-decoration: {none, line-through, overline, underline, ...}
* font-size: {5px, 10px, ...}
* font-family: {'b nazanin', ubuntu, jetbrains, any font family that exists on the client computer} We can also give multiple font families in-case a font family does not exist on the client computer
* text-align: {left, right, center, justify}

While writing a complex text with a combination of different directional languages like English and Persian, you can give a direction property that gets ltr meaning left to right or rtl meaning right to left. 
For more complex instances this technique is useful:

```HTML
در انگلیسی 
<span style="direction: ltr; display: inline-block;">No pain no gain</span>
ضربالمثل است
```
## CSS Units

We have two types of units in CSS:

* Absolute: Like px, cm, mm, in, pt, ...
* Relative: vw(relative to viewport), %(scale relative to parent), em(relative to font size), rem(relative to font size of root) ....

**Relative height can only be used if the parent's height can be found.**

[More information here](https://www.w3schools.com/cssref/css_units.php)

## Variables in CSS

You can define variables inside **:root**.

```CSS
:root {
	--text-color: blue;
}

h1 {
	color: var(--text-color);
}
```
## Background

You can put background-color, background-image, etc on html tags. especially div tags.

```CSS
div {
background-image: url(images.jpg);
background-repeat: no-repeat;
background-position: center;
background-size: 500px 300px;
background-color: green;
}
```

* **if background-size is contain, the image will scale alongside the viewport when it gets smaller**
* **if background-size is cover, it will cover the entire div background**

the short hand for all of above, is this:

```CSS
div {
background: green url(images.jpg) no-repeat center;
background-size: 500px 300px or {cover, contain};
}
```
## Positions

All tags have the position property that dictates how they should be laid out.
Elements are positioned using the top, bottom, left, right properties, However **these properties won't work unless position is set first.**

There are 5 different positions:
* **static**: is positioned according to the normal flow of the page. Elements positioned with this are not affected by top, left, right,... properties.
* **relative**: is positioned relative to its normal position
* **absolute**: is positioned relative to the nearest positioned ancestor.
* **sticky**: is positioned based on the user's scroll position
* **fixed**: is positioned relative to the viewport
All tags are **static** by default.
## Z-Index

Z-index is a number that a tag can have when if its number is larger than another tag it will appear on top of the other tags.
```CSS
div 
{
z-index: 5;
}
```

**The number of the z-index does not matter, it only matters in relation to z-index of others.**
## Selectors

CSS selectors are used to *find* html tags. We can not write every CSS properties of html tags inline, so there needs to be a mechanism to detach CSS from HTML for readability's sake.

* **Simple Selectors**: select based on name, class, id, etc
* **Combinator selectors**: select elements based on relationships
* **Pseudo-elements selectors**: selecting a specific characteristics of html tags, for example for a tags which have a "hover" characteristic.
* **Attribute selectors**: selecting based on attributes that html tags have
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
all tag elements who are the children of the chosen tag element are selected.

* adjacent sibling selector:

```CSS
div + div {
....
}
```
the tag right after the current tag element is affected

* general sibling selector:
```CSS
div ~ div {
...
}
```
all tag elements after the tag element are affected.

### Attribute Selectors

They're very common while being used with input tags.

```CSS
input[type="text"]
{

}
input[value] {

}
input[title="flower"] {
}

[class^="top"] { //"meaning any tag with a class starting with top"
}
[class*="te"]//"if the class name starts with te"

[class~="flower"]//if the word flower exists in class
```

### Pseudo-Classes

```CSS
a:hover { //"properties for when cursor hovers over element"

}
a:visited {//"when link is already visited"

}
a:active {//"when link is being clicked on"

}
// "to change color of label if input of type='checked' is currently checked"
.aggreement + label {
	color: red;
}
.aggreement:checked + label {
	color: black;
}
...
```

You can look for references for different psuedo-classes to use.

```CSS
.readable-text p:not(.active) { //"if a child p of readable-text class does not have the class active

}

<div class="readable-text">
<p></p>

<p class="active"></p> //"this will not be affected"

<p></p>


</div>
```
### nth-child and etc

```HTML
<style>
p:first-child {

}

p:last-child

p:nth-child(3)//"the third child" {

}

p:nth-child(even)//"even children, second, fourth, etc are affected"

p:nth-child(odd)//"odd children, third, fifth, seventh, are effected"
</style>

<div>
<p></p>//"this is the first child"
.
<p></p> //"this is the third child"
.
<p></p>//"this is the last child"

</div>
```

## CSS Priorities

There are rules as to which duplicate property sets are prioritized and used.

* The property that is set later than the others is prioritized. 
* The more specific the CSS selector is, the more priority its property sets are.
* Inline-style has the most priority, since they have the most specificity.
* You can prioritize a specific property by adding !important at the end of the property value you've set.


## Pseudo-Elements

Using psuedo-elements we can add special tags after or before a tag's contents.

```HTML

<style>
	.nav a::after {
		content: "s";
	}
</style>
```

* "after" means after a tag's contents
* "before" means before a tag's contents
## Responsive Design Rules and Techniques

In order to create a well-designed layout, we use a few techniques

### **Flex** 

Using this property on a tag, will organize its children in an inline fashion. A characteristic of this property is that if one tag's height changes, the others change with it. Here's a few properties that will be activated for its parent and its children.

*  **flex-direction**: its default value is row which aligns the flex of its children in rows.
	* row-reverse: reverses row direction
	* column: is aligned in columns.
	* column-reverse: reverses column direction.
* **flex-wrap**: 



* Grid:
* Flout:



