---
layout: post
title: HTML5 Input Types
comments: true
published: false
---

Here is the HTML5 input types: (not all browsers support)

<body ononline="update(true)"
	  onoffline="update(false)"
	  onload="update(navigator.onLine)">

	<article itemscope itemtype="http://schema.org/BlogPosting">
		<section>
			<h1>My Status</h1>
			<p>You are: <span id="status">(Unknown)</span></p>
		</section>
		<section>
			<form>
				<label>Spellcheck: </label>
				<input type="text" spellcheck="true">
				</br>
				</br>
				<label>Date: </label>
				<input type="date" name="date">
				</br>
				<label>Datetime: </label>
				<input type="datetime" name="datetime">
				</br>
				<label>Datetime Local: </label>
				<input type="datetime-local" name="datetime-local">
				</br>
				<label>Email: </label>
				<input type="email" name="email">
				</br>
				<label>Color: </label>
				<input type="color" name="color">
				</br>
				<label>Month: </label>
				<input type="month" name="month">
				</br>
				<label>Number: </label>
				<input type="number" name="number" name="quantity" min="1" max="5">
				</br>
				<label>Range: </label>
				<input type="range" name="range">
				</br>
				<label>Search: </label>
				<input type="search" name="search">
				</br>
				<label>Telephone: </label>
				<input type="tel" name="telephone">
				</br>
				<label>Time: </label>
				<input type="time" name="time">
				</br>
				<label>Url: </label>
				<input type="url" name="url">
				</br>
				<label>Week: </label>
				<input type="week" name="week">
				</br>
			</form>
		</section>
	</article>
		<footer><p>Copyright 2014 Luis</p></footer>

	<script>
		function update(online) {
			document.getElementById('status').textContent =
				online ? 'Online' : 'Offline';
		}
	</script>
	<script src="http://code.jquery.com/jquery-1.10.2.min.js"></script>
</body>

