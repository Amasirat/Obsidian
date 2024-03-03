It's a type of formatting language for text in websites.


A simple HTML DOCTYPE skeleton:

```HTML
<!DOCTYPE html>

<html lang="en" dir="ltr">
<head>
	<title></title>
</head>


<body>

</body>


</html>
```

For styling these HTML text, [[CSS]] is used.
## Parent, Children and Sibling Relationship





## Popular Tags

```HTML
<select>
	<option selected></option> 
<select>
```

giving selected to an option tag, selects that option by default

```HTML
<textarea></textarea>
```

You can use textarea tag to get comments or large amount of text.

```HTML
<video src="address to video" width="" height="" controls muted autoplay loop></video>
```

**Browsers have a fast rule that any webpage is not allowed to be able play any audio without the permission of the user, so you can not make a video autoplay if it is not muted**

and the audio tag is also simillar to the video tag.

MP3, OGG, and WAV are supported by most browsers. MP3 is most commonly supported.
You can indicate multiple sources for the audio and video tags like this:

```HTML
<audio controls>
<source src="music.mp3" type="audio/mp3">
<source src="music.ogg" type="audio/ogg">
Your browser does not support audio element's formats
</audio>
```
## Input and form related tags

**Here's the most common input types:**

```HTML
<input type="text">
<input type="password">
<input type="file">
<input type="text" value="amirhossein">
<input type="text" placeholder="Enter Username">
<input type="number" min="0" max="120" step="0.5">
<label for="terms_agreement">I agree with terms and conditions</label>
<input type="checkbox" id="terms_agreement">
<input type="radio">
<input type="submit">
```

We can relate a label tag to an input tag by giving the input tag's id to the for variable of the label tag.

We can give a name to the variable we use to send the value of input tags into the backend.

```HTML

<input name="random_input">
```

**In order to relate any radio tags together, they need to have the same backend name**

We use a form tag to bundle most input tags and be able to send their data to the backend.

```HTML
<form action="adress of backend server" method="post">
	<label for="user_field">Username</label>
	<input type="text" name="username" id="user_field">
	<label for="pass_field">Password</label>
	<input type="password" name="password" id="pass_field">
	<input type="submit" value="submit">
</form>
```

If a form is not given an action field, the data will be sent to the same html page it exists in. However sending backend data to html pages is pointless as you can imagine.

**Any input tag that does not have an explicit name will not be sent, so every input tag needs to have a unique backend name**

The value parameter for input tags give a default value for the input tag. 

***Although for radio tags, it will give the checked input a different value***.

**Also writing required inside any tag, will make that field mandatory to submit**

A form contains a method parameter that can contain a few values:

* get: which is the default method value and sends data while also showing the data in the current url you are in

* post: sends data but does not show data in url. It's useful for login or sign up pages where important and sensitive data is taken

* dialog: 

