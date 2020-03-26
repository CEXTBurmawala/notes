## Change Case Number

### [1] Add format `options.global`
In the file `config/options.global.js`, add the format of the case numbering wanted by the customer.
- example: 	`caseNumberFormat: ['YYYY', '-', 'MM', '-', 'NNNN'],`

### [2] Add file in lib/core
Add the following file to the lib/core:  `lib/core/case-number-formatter-ex.js`

In this file, add the number format code. See reference below for examples. Also, see platform `lib/core/case-number-formatter.js` for all the default case numbering formats.

### Reference
See the notes from the platform team on how to customize the case numbering system:
[platform README](https://github.com/i-Sight/isight_main_v5_beta/blob/develop/entities/case/README.md)

***
[Table of Contents](../README.md)