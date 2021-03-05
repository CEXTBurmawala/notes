## Alow yes/no radio to be unselected

In order for a yes/no radio field to be unselected, add the following to the field in the `index.js` file:
```js
typeOptions: {
	allowNull: true,
},
```

***
[Table of Contents](../README.md)