# Custom Actions

The Custom Actions lets you define your own predefined api methods. You can define any number of custom actions through your Model configurations through `actions` option.

```js
class User extends Model {
  static entity = 'users'

  static fields () {
    return {
      id: this.attr(null),
      name: this.attr('')
    }
  }

  static apiConfig = {
    actions: {
      fetch: {
        method: 'get',
        url: '/api/users'
      }
    }
  }
}
```

You can see that in the above example, we have defined `fetch` action with the option that defines the `method` and `url`.

Now you may call this action as if it was a predefined method.

```js
User.api().fetch()
```

The above method will perform api call to `/api/users` with `GET` method. Now the value for the action (in the example, which is the object that defines `method` and `url`) is the request configuration. Now you see that the above example is equivalent to calling:

```js
User.api().request({
  method: 'get',
  url: '/api/users'
})
```

Actions can also be defined as a function. In this case, just call the desired method with in action. With this approach, you can configure the convenience argument to the action, and gives you more powerful control.

Remember that inside the function, `this` is bind to the Request object, not the Model where the actions are defined.

```js
class User extends Model {
  static apiConfig = {
    actions: {
      fetchById (id) {
        return this.get(`/api/users/${id}`)
      }
    }
  }
}
```

And you can call that action like so.

```js
User.api().fetchById(1)
```

## When to Use Custom Actions?

While the custom actions are convenient and easy to set up, you can always define methods to the Model directly to get pretty much the same result.

```js
class User extends Model {
  static fetchById (id) {
    return this.api().get(`/api/users/${id}`)
  }
}
```

Well of crouse in this case, you must call that method from Model and not from `api()` method.

```js
User.fetchById(1)
```

To be honest, this is a much better way to define custom methods in terms of simplicity and also better with type safety when using TypeScript.

The benefits of defining custom actions inside the configuration are that you can put those methods under Request object, so it becomes more consistent when calling it from the Model. Also, it could be easier to share custom actions between different Models.

It's up to you how to define custom actions. Though if you have any ideas or feedback, we're more than happy to hear it from you!
