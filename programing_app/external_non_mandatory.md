## External Non-Mandatory Fields

In order to allow saving an external capture form that has non mandatory fields that are mandatory in the system, add the following code to the `lib/services/business-logic/external-capture/workflow/external-capture-save.js` file.

```
const systemParty = await seneca.delegate({
		user$: { id: '0' },
		perm$: null,
		bypassRules$: true,
	});
```

In order to not create a user if no fields are filled out, add the following:

```
const containsData = externalChildDoc.fields
	.some(field => !_.isEmpty(result[field.external]));

if (containsData)	{
	for (const field of externalChildDoc.fields) {
		let value = field.default;
		if (field.external) {
			value = result[field.external];
		}
		if (value !== undefined) {
			data[field.internal] = value;
		}
	}
	await systemParty
		.make(
			`${externalChildDoc.entity.base}/${externalChildDoc.entity.name}`,
			data,
		)
		.save$();
}
```

See [this](https://github.com/i-Sight/config_cwcpcn_v5/commit/045345f88ca34eb8da5457c92779e91c9efd167d) commit for an example.

***
[Table of Contents](../README.md)