# image-cache
[![Build Status](https://travis-ci.org/nyancodeid/image-cache.svg?branch=master)](https://travis-ci.org/nyancodeid/image-cache) [![npm version](https://badge.fury.io/js/image-cache.svg)](https://badge.fury.io/js/image-cache)

Powerful image cache for NodeJS

## What is image-cache?
`image-cache` is a NodeJS module for caching images and serving them in a base64 format. `image-cache` uses
Asynchronous calls for best Performance.

## New
- Compressed options default is `false`
- Library Core now using Javascript Classes
- New syntax and function
- Now using [google proxy cache](https://gist.github.com/coolaj86/2b2c14b1745028f49207) for best caching.

## Installation
In order to install `image-cache` in your project you can use NPM and Yarn using the following commands:

### NPM
```bash
npm install --save image-cache
```

### Yarn
```bash
yarn add image-cache
```

## Usage
Start using `image-cache`

```javascript
const imageCache = require('image-cache');
```

## API

### setOptions
Used to override default options from the package. The function doesn't return anything and should
be used after requiring `image-cache`. E.g.

```javascript
const imageCache = require('image-cache');

imageCache.setOptions({
   compressed: false

   // write your custom options here
});
```
#### API
| Key          | Data Type    | Default Value    | Description |
| :------------- | :----------- | :------------- | :------------- |
| `dir` | `string` | `path.join(__dirname, 'cache/')` | Root directory for your cached files |
| `compressed` | `boolean` | false | Compress cache output with zlib. Compressing files can make the process of caching your files take a little longer. For example: without compression, saving a file takes 6-7ms but when using compression it takes 150-185ms. Compressed files however take less disk space |
| `extname` | `string` | `.cache` | File extension for your cache files |
| `googleCache` | `boolean` | `true` | Use google cache proxy |

### isCached()
Checks whether an image is already cached or not.

```javascript
imageCache.isCached(url, callback);
```

#### Params
| Key          | Data Type    |
| :------------- | :----------- |
| `url` | `string` |
| `callback` | `function` |

#### Example Using Callback
```javascript
imageCache.isCached(url, (exist) => {
   if (exist) {
      // do something with cached image
   }
});
```
#### Example Using Promise
```javascript
imageCache.isCached(url).then((exist) => {
   if (exist) {
      // do something with cached image
   }
});
```

### isCachedSync()
Synchronously checks whether an image is already cached or not. Returns a `boolean`.

#### Example

```javascript
const exists = imageCache.isCachedSync(url);
```
#### API
| Params         | Data Type    | Description    |
| :------------- | :----------- | :------------- |
| `url` | `string` | The URL of your image |


### ~~getCache()~~
Deprecated

### get()
Get a cached image

#### Example
```javascript
imageCache.get(url, (error, image) => {
   console.log(image);

   // do something with image
});
```
#### API Callback
| Key          | Data Type    | Description    |
| :------------- | :----------- | :------------- |
| `image` | `object` | Single cached image |
| `image.error`  | `boolean`   | Error indicator |
| `image.url`    | `string`   | The original image URL |
| `image.hashFile` | `string` | The filename in the cache directory (`options.dir`) |
| `image.timestamp` | `integer` | Timestamp of the cached file |
| `image.compressed` | `boolean` | Whether the cache was compressed |
| `image.data` | `string` | Base64 encoded version of the image |

### ~~getCacheSync()~~
Deprecated

### getSync
Synchronously get a cached image

#### Example

```javascript
var image = imageCache.getSync('http://domain/path/to/image.png');
```

#### API
API is the same as `get()`

### ~~setCache()~~
Deprecated

### store()
Stores a new image or set of images and writes the cached files into the `options.dir` directory. 

```javascript
imageCache.store(images, callback);
```

#### Params
| Key          | Data Type    |
| :------------- | :----------- |
| `images` | `array`|`string` |
| `callback` | `function` |

#### Example using callback
```javascript
const images = 'https://eladnava.com/content/images/2015/11/js-6.jpg';
imageCache.store(images, function(error) {
   console.log(error);
});
```
#### Example using promise
```javascript
const images = 'https://eladnava.com/content/images/2015/11/js-6.jpg';
imageCache.store(images).then((result) => {
    // Do something with your stored image(s)
}).catch((e) => {
    console.log(e)
});
```

#### API Callback and Promise Catch
| Key          | Data Type    | Description    |
| :------------- | :----------- | :------------- |
| `error` | `object` | This error came from `fs.writeFile`  |

### ~~flushCache()~~
Deprecated

### flush()
Delete all cache files in your `options.dir` with the same extension as your `options.extname`

#### Example using callback
```javascript
imageCache.flush(function(error, results) {
   if (error) {
      console.log(error);
   } else {
      console.log(results);
   }
});
```
#### Example using promise
```javascript
imageCache.flush().then((results) => {
    console.log(results);
}).then((error) => {
    console.log(error);
});
```

#### API Callback
| Key          | Data Type    | Description    |
| :------------- | :----------- | :------------- |
| `results` | `object` |  |
| `results.deleted` | `number` | Total # of deleted files |
| `results.totalFiles` | `number` | Total # files in cache directory (`options.dir`) |
| `results.dir` | `string` | Path to the flushed directory |

### flushSync()
Synchronously flush the cache.

#### Example

```javascript
const results = imageCache.flushSync();
```

#### API
API is the same as `flush()`

| Key          | Data Type    | Description    |
| :------------- | :----------- | :------------- |
| `results.error` | `boolean` | Error indicator |
| `results.message` | `string` | Error message |

### ~~fetchImage()~~
Deprecated

### fetch()
Stores and retrieves data from the cache in one go. This function is asynchronous for best possible performance. 
`fetch()` will check your local cache for the requested image and either serves that image if it exists or 
downloads the image and saves it to the cache before serving it. 

#### Params
| Key          | Data Type    | Description    |
| :------------- | :----------- | :------------- |
| `image` | `string` or `array` | Image or array of images |

#### Example

```javascript
const image = "http://path.to/image.jpg";

imageCache.fetch(image).then((images) => {
   images.forEach((image) => {
      console.log(image);

      // { ... }
   });
   console.log(images);

   // [ { ... } ]
}).catch((error) => {

});
```

#### API
| Key          | Data Type    | Description    |
| :------------- | :----------- | :------------- |
| `images` | `array` | Array of cached images |
| `image` | `object` | Single cached image |
| `image.error`  | `boolean`   | Error indicator |
| `image.url`    | `string`   | The original image URL |
| `image.hashFile` | `string` | The filename in the cache directory (`options.dir`) |
| `image.timestamp` | `integer` | Timestamp of the cached file |
| `image.compressed` | `boolean` | Whether the cache was compressed |
| `image.data` | `string` | Base64 encoded version of the image |
| `image.cache` | `string` | Status of your cache (`MISS` or `HIT`) |

## What do HIT and MISS mean?

### MISS
MISS means that the requested image was not available in the cache directory (`options.dir`). The image was retrieved 
from the given URL and saved to the cache directory before it was returned.

### HIT
HIT means the requested image was available in the cache directory (`options.dir`) and returned immediately from cache.
