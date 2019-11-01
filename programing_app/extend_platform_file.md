## Extend Platform File

- Create a file in the same location in the file structure with the same name + '-ex.js" at the end of the file name.
- Add the key:value file path pair to 'webpack.overrides.js' file
- Require the platform file
	- use camel case or the name
	- "<file_name>.js?original"
- `module.exports = <name>.extend ({ ... });`


### notes:
- Form-layouts don't need to be extended, they are overwriting the platform file.
- When folder contains a file called 'index-ui.js', there is no need to extend files.
- Entities don't need to be extended, they will be overwritten (See uclasdc for example in entities/email).

***
[Table of Contents](../README.md)