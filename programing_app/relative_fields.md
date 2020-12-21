## Relative Fields Backend

To make fields dynamically generated in both the frontend & the backend, the field kind needs to be 'computed' with kindOptions set to computeFunction with the name of the function. Then a `compute-function.js` file needs to be created in the entity folder. This file contains functions that will calculated/generate field content before being saved to the database.

If the fields need to be dynamically set on the frontend as the user inputs data, the view also needs to have functions that set values based on other fields changing.

Examples:
- [UCRCC example code](https://github.com/i-Sight/config_ucrcc_v5/pull/33/commits/f91e6c3a627629f892e0f6b562bd8b06c4a8402e)
- [UBC example code](https://github.com/i-Sight/config_ubc_v5/pull/1)


***
[Table of Contents](../README.md)