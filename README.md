# pumpify

Combine an array of streams into a single duplex stream using [pump](https://github.com/mafintosh/pump) and [duplexify](https://github.com/mafintosh/duplexify).
If one of the streams closes/errors all streams in the pipeline will be destroyed.

```
npm install pumpify
```

## Usage

Pass the streams you want to pipe together to pumpify `pipeline = pumpify(s1, s2, s3, ...)`.
`pipeline` is a duplex stream that writes to the first streams and reads from the last one.
Streams are piped together using [pump](https://github.com/mafintosh/pump) so if one of them closes
all streams will be destroyed.

``` js
var pumpify = require('pumpify')
var tar = require('tar-fs')
var zlib = require('zlib')
var fs = require('fs')

var untar = pumpify(zlib.createGunzip(), tar.extract('output-folder'))

fs.createReadStream('some-gzipped-tarball.tgz').pipe(untar)
```

If you are pumping object streams together use `pipeline = pumpify.obj(s1, s2, ...)`.
Call `pipeline.destroy()` to destroy the pipeline (including the streams passed to pumpify).

## License

MIT