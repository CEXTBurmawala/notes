## Setup New Project

### Install generator on local machine
- In your `~/git/` directory, create a directory called 'product-self-service'
- navigate to <https://github.com/i-Sight/isight-self-service>
- Clone the repo to 'generator'
- navigate into the directory `cd generator`
- Run `nvm install`
- Run `yarn`
- Run `yarn link`
- Run `product-generator --help` to ensure that the CLI tool is installed.

### Copy fieldspec to fieldspec directory
- Copy xlsx fieldspec file to `~/git/fieldspecs` directory
- Rename the file to the same <name> as 'config_<name>_v5' (just the name part)
- Inside the fieldspec, edit the following items:
  - 'Form - Case' > 'Resolution' remove 'Close Case Reason' and 'Re-open the Case' completely.
  - 'Form - Child Docs' remove 'Note', 'File' & 'Todo' rows completely.
  - Double-check the tiered dropdowns have 0 spacing between rows.
  - Double-check the regular picklists have 1 space between rows.

### Generate project files
- In terminal, navigate to the project directory
- Run `product-generator generate --platform ../_platform/<version> ../fieldspecs/<name>.xlsx`
Project files should now be generated. Run `git status` to see all the files created.

### Update generator
- Navigate to location where the generator repository is on your machine (`~/git/product-self-service`)
- Ensure that you are on the master branch and run `git pull origin master`

***
[Table of Contents](../README.md)
