## External Capture Modal

To set the onSaveClick event to a modal instead of a green banner notification (v5.x app):
- In the file `public/views/external-capture/external-capture-details-view.js` add the `modal: true` attribute to the `notify.display(...)` method. The `notify.js` file is a utility found in the platform under the `public/lib/` directory.

See [here](https://github.com/i-Sight/config_centracare_v5/blob/e78d3fe2ac9d73d9dda163f70932c6dbad90dd8a/public/views/external-capture/external-capture-details-view.js?ts=2) for an example.


***
[Table of Contents](../README.md)