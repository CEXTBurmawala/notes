## Adding Sub Section

Adding a section in the details tab of the case view. The 'Overview' and 'Related Cases' sub sections are defaults and other sub sections can be added next to them.

- The following files need to be edited. These files need to be edited to add the fields, rules and validations required inside the form.:
  - `entities/case/index.js`
  - `entities/case/rules.js`
  - `entities/case/validation.js`
  - `data/translations/en_US.js.js`
  - `config/options.picklists.js`

- Any picklists need to be added to the data/lists directory.
- Add the form file to `config/form-layouts/case-<name>-form.js`
- Add view file to `public/views/case/case-<name>-view.js`
- Add the sub section to the array of sub sections in the file `public/config/options-tabs-ex.js`
- Add the template to `public/templates/case/case-<name>-tmpl.dust`


***
[Table of Contents](../README.md)