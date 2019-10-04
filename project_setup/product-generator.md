## Setup New Project

### Install generator on local machine
- In your `~/git/` directory, create a directory called 'product-self-service'
- navigate to <https://github.com/i-Sight/product-self-service>
- Clone the repo to 'product-self-service'
- In terminal, run `yarn`
- Run `yarn link`
- Run `product-generator --help` to ensure that the CLI tool is installed.

### Copy fieldspec to fieldspec directory
- Copy xlsx fieldspec file to `~/git/fieldspecs` directory
- Rename the file to the same <name> as 'config_<name>_v5' (just the name part)

### Generate project files
- In terminal, navigate to the project directory
- Run `product-generator generate --platform ../_platform/5.2.x ../fieldspecs/<name>.xlsx`
Project files should now be generated. Run `git status` to see all the files created.

***
[Table of Contents](../README.md)