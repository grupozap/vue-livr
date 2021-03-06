# Vue LIVR

## Install

`npm i -S livr`
`npm i -S vue-livr`

## Usage

In your script entry point:

```js
import Vue from 'vue';
import VueLIVR from 'vue-livr';

Vue.use(VueLIVR, {
  extraRules: {}, // Extra rules to be added
  extendedErrors: false, // Patch rules to return extended error codes
  errorHandlers: {}, // Error handler to each error code that LIVR returns, it will run only if extendedErrors = true
  aliasedRules: [], // list of aliasedRules to register
});
```

Now you are all set to use the plugin.

## Basic example

```html
<input :class="{ invalid: errors.hasError('name')}" @blur="validate('name')" v-model="name" />
<span>errors.getError('name')</span>
```

```js
const livrRules = {
  name: ['required', { max_length: 20 }],
};

export default {
  data() {
    return {
      name: '',
    };
  },
  methods: {
    validate(field) {
      const { name } = this;
      this.$livr.validate(livrRules, { name }, field);
    },
  },
};
```

## Array example

```html
<div v-for="(user, index) in users" :key="index">
  <input
    class="listing-units__input"
    v-model="user.name"
    @blur="validate('name', index)"
    :class="{ invalid: errors.hasError('name', index) }"
  />
  <span>{{errors.getError('name', index)}}</span>
</div>

<button @click="validateAll">Validate All</button>
```

```js
const livrRules = {
  users: [
    'required',
    {
      list_of_objects: [
        {
          name: ['required', { min_length: 1 }],
        },
      ],
    },
  ],
};

export default {
  data() {
    return {
      users: [{ name: '' }, { name: '' }],
    };
  },
  methods: {
    validate(field, index) {
      const fieldName = `users[${index}].${field}[0]`;
      this.$livr.validate(livrRules, this.users, fieldName);
    },
    validateAll() {
      this.$livr.validateAll(livrRules, this.users);
    },
  },
};
```

## Credits

- Based on [Vee-validate's](https://github.com/baianat/vee-validate)

## License

MIT
