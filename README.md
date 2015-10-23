# vue-form

HTML5 form validation for Vue.js 1.0+

### Install

Available through npm as `vue-form`.

  ``` js
  // es6: import * as vueForm from 'vue-form'; 
  var vueForm = require('vue-form');
  Vue.use(vueForm);
  ```
  
You can also directly include it with a `<script>` tag when you have Vue itself included globally. It will automatically install itself.

### Usage

This plugin registers two global directives, `v-form` and `v-form-ctrl`.  Apply the `v-form` directive to a `form` element, and set the `name` attribute. This `name` will hold the overall form state object and is created on the current vm.

Apply the `v-form-ctrl` directive to each of the form elements. `v-form-ctrl` will watch `v-model` and validate on change. Use static or binding attributes to specify validators (`required`, `maxlength`, `type="email"`, `type="url"`, etc)

```html
<form v-form name="myform" @submit.prevent="onSubmit">
	<div class="errors" v-if="myform.$submitted">
		<p v-if="myform.name.$error.required">Name is required.</p>
		<p v-if="myform.email.$error.email">Email is not valid.</p>
	</div>
	<label>
		<span>Name *</span>
		<input v-model="model.name" v-form-ctrl required name="name" />
	</label>
	<label>
		<span>Email</span>
		<input v-model="model.email" v-form-ctrl name="email" type="email" />
	</label>
	<button type="submit">Submit</button>
</form>
<pre>{{ myform | json }}</pre>
```

`myform` will be an object with the following properties:
```js
{
  "$name": "myform",
  "$dirty": false,
  "$pristine": true,
  "$valid": false,
  "$invalid": true,
  "$submitted": false,
  "$error": {
    "name": {
      "$name": "name",
      "$dirty": false,
      "$pristine": true,
      "$valid": false,
      "$invalid": true,
      "$error": {
        "required": true
      }
    }
  },
  "name": {
    "$name": "name",
    "$dirty": false,
    "$pristine": true,
    "$valid": false,
    "$invalid": true,
    "$error": {
      "required": true
    }
  },
  "email": {
    "$name": "email",
    "$dirty": false,
    "$pristine": true,
    "$valid": true,
    "$invalid": false,
    "$error": {}
  }
}
```

### Validators

Built in validators:

```
type="email"
type="url"
type="number"
required
minlength
maxlength
pattern
min (for type="number")
max (for type="number")

```

You can use static validation attributes or bindings. If it is a binding, the input will be re-validated every binding update meaning you can have inputs which are conditionally required.
```html
  <!-- static validators -->
  <input type="email" v-form-ctrl required />
  <input type="text" v-form-ctrl maxlength="25" minlength="5" />

  <!-- bound validators -->
  <input type="email" v-form-ctrl :required="isRequired" />
  <input type="text" v-form-ctrl :maxlength="maxLen" :minlength="minLen" />
```

