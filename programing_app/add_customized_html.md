## Add Customized HTML

If a custom HTML block needs to be added to a form, use the `information-text` element instead of the `html-text` element. This allow you to set `href="mailto:me@example.com"` and inline CSS styling.

Example:

```js
{
	type: 'information-text',
	text: '<p>hello</p>',
	displayRule: 'case1Rule',
},
```

***
[Table of Contents](../README.md)