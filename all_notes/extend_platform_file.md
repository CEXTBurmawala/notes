## Extend platform file

- Create a file in the same location in the file structure with the same name + '-ex.js" at the end of the file name.
- Add the key:value file path pair to 'webpack.overrides.js' file
- Require the platform file
	- use camel case or the name
	- "<file_name>.js?original"
- `module.exports = <name>.extend ({ ... });`