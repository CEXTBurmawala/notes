## External 400 Error

If you receive an 400 error when saving the external capture form, it could be due to a mandatory field not being set or captured by the external form. Often, the "Initial Contact Date" is supposed set by default to be the current date. In the file `config/options.external-capture.js` the "initialContactDate" should have a default value of `new Date()`.

If the intent is to make fields that are mandatory in the system not mandatory on external, see the notes [here](./external_non_mandatory.md).


***
[Table of Contents](../README.md)