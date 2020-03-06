## Yarn Link Explanation

The purpose of using yarn link is to add the platform as a node module in your app.

#### Link to Project
- In the project you want to link, create a symlink by running `yarn link`.
- The name of the package will be the name specified in `package.json`
- Links are registered in `~/.config/yarn/link`

#### Link in App
- In the app directory, run `yarn link <name>` in order to link the project at the symlink to this project.
- Yarn creates the symlink in your project's `node_modules` directory.

#### Symlink
"A symbolic link contains a text string that is automatically interpreted and followed by the operating system as a path to another file or directory."
- [Symlink at wikipedia](https://en.wikipedia.org/wiki/Symbolic_link)

***
[Table of Contents](../README.md)