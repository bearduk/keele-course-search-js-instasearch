# js-course-search

> Course Search InstantSearch.js

A simple course finder using 3 facets in a refinement list.

Lots of variations on this.
I could just use cssClasses on the standard widget.
The alternative is to use the custom template passing in data [https://jsfiddle.net/bearduk/hb3agn5u/](https://jsfiddle.net/bearduk/hb3agn5u/)

The refinement list needs to be custom html, including a working checkbox:

Required output for html is:

```html
<div class="form__field"><input name="name-study-level" id="id-undergraduate" type="checkbox" value="undergraduate"> <label for="id-undergraduate">Undergraduate</label></div>

<div class="form__field"><input name="name-study-level" id="id-postgraduate-taught" type="checkbox" value="postgraduate-taught"> <label for="id-postgraduate-taught">Postgraduate Taught</label></div>

<div class="form__field"><input name="name-study-level" id="id-postgraduate-research" type="checkbox" value="postgraduate-research"> <label for="id-postgraduate-research">Postgraduate Research</label></div>
```

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```

For detailed explanation on how things work, consult the [docs for vue-loader](http://vuejs.github.io/vue-loader).
