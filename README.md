# api documentation for  [twitter (v1.7.0)](https://github.com/desmondmorris/node-twitter)  [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-twitter.svg)](https://travis-ci.org/npmdoc/node-npmdoc-twitter)
#### Twitter API client library for node.js

[![NPM](https://nodei.co/npm/twitter.png?downloads=true)](https://www.npmjs.com/package/twitter)

[![apidoc](https://npmdoc.github.io/node-npmdoc-twitter/build/screen-capture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-twitter_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-twitter/build..beta..travis-ci.org/apidoc.html)

![package-listing](https://npmdoc.github.io/node-npmdoc-twitter/build/screen-capture.npmPackageListing.svg)



# package.json

```json

{
    "author": {
        "name": "Desmond Morris",
        "email": "hi@desmondmorris.com"
    },
    "bugs": {
        "url": "https://github.com/desmondmorris/node-twitter/issues"
    },
    "dependencies": {
        "deep-extend": "^0.4.1",
        "request": "^2.72.0"
    },
    "description": "Twitter API client library for node.js",
    "devDependencies": {
        "eslint": "^2.10.0",
        "mocha": "^2.4.5",
        "nock": "^8.0.0"
    },
    "directories": {},
    "dist": {
        "shasum": "2882e208e0ad83bb226528613561f1dd233ac94b",
        "tarball": "https://registry.npmjs.org/twitter/-/twitter-1.7.0.tgz"
    },
    "gitHead": "87225a043db8f318fa1a05399638e8f106ea43a9",
    "homepage": "https://github.com/desmondmorris/node-twitter",
    "keywords": [
        "twitter",
        "streaming",
        "oauth"
    ],
    "license": "MIT",
    "main": "./lib/twitter",
    "maintainers": [
        {
            "name": "desmondmorris",
            "email": "hi@desmondmorris.com"
        }
    ],
    "name": "twitter",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+https://github.com/desmondmorris/node-twitter.git"
    },
    "scripts": {
        "lint": "eslint test/*.js lib/*.js",
        "test": "npm run lint && mocha"
    },
    "version": "1.7.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module twitter](#apidoc.module.twitter)
1.  [function <span class="apidocSignatureSpan">twitter.</span>parser ()](#apidoc.element.twitter.parser)
1.  object <span class="apidocSignatureSpan">twitter.</span>parser.prototype

#### [module twitter.parser](#apidoc.module.twitter.parser)
1.  [function <span class="apidocSignatureSpan">twitter.</span>parser ()](#apidoc.element.twitter.parser.parser)
1.  number <span class="apidocSignatureSpan">twitter.parser.</span>END_LENGTH
1.  string <span class="apidocSignatureSpan">twitter.parser.</span>END

#### [module twitter.parser.prototype](#apidoc.module.twitter.parser.prototype)
1.  [function <span class="apidocSignatureSpan">twitter.parser.prototype.</span>receive (buffer)](#apidoc.element.twitter.parser.prototype.receive)



# <a name="apidoc.module.twitter"></a>[module twitter](#apidoc.module.twitter)

#### <a name="apidoc.element.twitter.parser"></a>[function <span class="apidocSignatureSpan">twitter.</span>parser ()](#apidoc.element.twitter.parser)
- description and source-code
```javascript
function Parser() {
  // Make sure we call our parents constructor
  EventEmitter.call(this);
  this.buffer = '';
  return this;
}
```
- example usage
```shell
n/a
```



# <a name="apidoc.module.twitter.parser"></a>[module twitter.parser](#apidoc.module.twitter.parser)

#### <a name="apidoc.element.twitter.parser.parser"></a>[function <span class="apidocSignatureSpan">twitter.</span>parser ()](#apidoc.element.twitter.parser.parser)
- description and source-code
```javascript
function Parser() {
  // Make sure we call our parents constructor
  EventEmitter.call(this);
  this.buffer = '';
  return this;
}
```
- example usage
```shell
n/a
```



# <a name="apidoc.module.twitter.parser.prototype"></a>[module twitter.parser.prototype](#apidoc.module.twitter.parser.prototype)

#### <a name="apidoc.element.twitter.parser.prototype.receive"></a>[function <span class="apidocSignatureSpan">twitter.parser.prototype.</span>receive (buffer)](#apidoc.element.twitter.parser.prototype.receive)
- description and source-code
```javascript
function receive(buffer) {
  this.buffer += buffer.toString('utf8');
  var index, json;

  // We have END?
  while ((index = this.buffer.indexOf(Parser.END)) > -1) {
    json = this.buffer.slice(0, index);
    this.buffer = this.buffer.slice(index + Parser.END_LENGTH);
    if (json.length > 0) {
      try {
        json = JSON.parse(json);
        // Event message
        if (json.event !== undefined) {
          // First emit specific event
          this.emit(json.event, json);
          // Now emit catch-all event
          this.emit('event', json);
        }
        // Delete message
        else if (json.delete !== undefined) {
          this.emit('delete', json);
        }
        // Friends message (beginning of stream)
        else if (json.friends !== undefined || json.friends_str !== undefined) {
          this.emit('friends', json);
        }
        // Any other message
        else {
          this.emit('data', json);
        }
      }
      catch (error) {
        error.source = json;
        this.emit('error', error);
      }
    }
    else {
      // Keep Alive
      this.emit('ping');
    }
  }
}
```
- example usage
```shell
...
// assumptions:
//   1) ninjas are mammals
//   2) tweets come in chunks of text, surrounded by {}'s, separated by line breaks
//   3) only one tweet per chunk
//
//   p = new parser.instance()
//   p.addListener('object', function...)
//   p.receive(data)
//   p.receive(data)
//   ...

var EventEmitter = require('events').EventEmitter;

var Parser = module.exports = function Parser() {
// Make sure we call our parents constructor
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
