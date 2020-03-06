## External Capture Modal

To set the onSaveClick event to a modal instead of a green banner notification (v5.x app):
- In the file `public/views/external-capture/external-capture-details-view.js` add the `modal: true` attribute to the `notify.display(...)` method. The `notify.js` file is a utility found in the platform under the `public/lib/` directory.

***
[Table of Contents](../README.md)