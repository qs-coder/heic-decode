# heic-decode

> Decode HEIC images to extract raw pixel data.

[![CI][ci.svg]][ci.link]
[![npm-downloads][npm-downloads.svg]][npm.link]
[![npm-version][npm-version.svg]][npm.link]

[ci.svg]: https://github.com/qs-coder/heic-decode/actions/workflows/ci.yml/badge.svg
[ci.link]: https://github.com/qs-coder/heic-decode/actions/workflows/ci.yml
[npm-downloads.svg]: https://img.shields.io/npm/dm/@qs-coder/heic-decode.svg
[npm.link]: https://www.npmjs.com/package/@qs-coder/heic-decode
[npm-version.svg]: https://img.shields.io/npm/v/@qs-coder/heic-decode.svg

## Install

```bash
npm install @qs-coder/heic-decode
```

## Usage

Decode the main image in the file:

```javascript
const fs = require('fs');
const { promisify } = require('util');
const decode = require('@qs-coder/heic-decode');

(async () => {
  const buffer = await promisify(fs.readFile)('/path/to/my/image.heic');
  const {
    width,  // integer width of the image
    height, // integer height of the image
    data    // Uint8ClampedArray containing pixel data
  } = await decode({ buffer });
})();
```

Decode all images in the file:

```javascript
const fs = require('fs');
const { promisify } = require('util');
const decode = require('@qs-coder/heic-decode');

(async () => {
  const buffer = await promisify(fs.readFile)('/path/to/my/multi-image.heic');
  const images = await decode.all({ buffer });

  for (let image of images) {
    // decode and use each image individually
    // so you don't run out of memory
    const {
      width,  // integer width of the image
      height, // integer height of the image
      data    // Uint8ClampedArray containing pixel data
    } = await image.decode();
  }
})();
```

When the images are decoded, the return value is a plain object in the format of [`ImageData`](https://developer.mozilla.org/en-US/docs/Web/API/ImageData). You can use this object to integrate with other imaging libraries for processing.

_Note that while the decoder returns a Promise, it does the majority of the work synchronously, so you should consider using a worker thread in order to not block the main thread in highly concurrent production environments._

## Related

* [heic-cli](https://github.com/qs-coder/heic-cli) - convert heic/heif images to jpeg or png from the command line
* [heic-convert](https://github.com/qs-coder/heic-convert) - convert heic/heif images to jpeg and png
* [libheif-js](https://github.com/qs-coder/libheif-js) - libheif as a pure-javascript npm module
