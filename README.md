# paged-request [![NPM version](https://img.shields.io/npm/v/paged-request.svg?style=flat)](https://www.npmjs.com/package/paged-request) [![NPM monthly downloads](https://img.shields.io/npm/dm/paged-request.svg?style=flat)](https://npmjs.org/package/paged-request) [![NPM total downloads](https://img.shields.io/npm/dt/paged-request.svg?style=flat)](https://npmjs.org/package/paged-request) [![Linux Build Status](https://img.shields.io/travis/jonschlinkert/paged-request.svg?style=flat&label=Travis)](https://travis-ci.org/jonschlinkert/paged-request)

> Simplified requests for paged (paginated) content.

Please consider following this project's author, [Jon Schlinkert](https://github.com/jonschlinkert), and consider starring the project to show your :heart: and support.

## Install

Install with [npm](https://www.npmjs.com/):

```sh
$ npm install --save paged-request
```

## Heads up!

See the [release notes](#release-notes) for information about changes made in v2.0.

## Usage

This library recursively calls [needle's](https://github.com/tomas/needle#needlemethod-url-data-options-callback--20x) `.get` method as long as the user-provided `next()` function returns a string (the next url to get). See [an example](#example).

**Example**

```js
const request = require('paged-request');

request(url, options, next)
  .then(acc => console.log(acc.pages.length))
  .catch(console.error);
```

### Params

* `url` **{string}** - (required) the initial url to get
* `options` **{object}** - (optional) options object to pass to [needle](https://github.com/tomas/needle)
* `next` **{function}** - (required) function that returns the next url to get, a promise or undefined.

### `next` function params

* `url` **{string}** - the original (base) user-provided url
* `resp` **{object}** - [needle](https://github.com/tomas/needle) response object
* `acc` **{object}** - accumulator object with the following properties:
  - `options` **{object}** - user-provided options object
  - `pages` **{array}** - array of responses
  - `urls` **{array}** - array of requested urls

The `next` function should return a string (the next url to get), promise or undefined.

## Example

The following example shows how to loop over pages of `CSS` posts on [smashingmagazine.com](https://www.smashingmagazine.com/category/css) (an arbitrary example, but they have great content!).

```js
const request = require('paged-request');

async function next(url, resp, acc) {
  // do stuff to check response first if necessary
  const regex = /href="\/.*?\/(\d+)\/"/;
  const num = (regex.exec(resp.data) || [])[1];

  if (num && /^[0-9]+$/.test(num) && +num <= n) {
    // use the "original" url to avoid having to reparse
    // and recreate the url each time
    return `${acc.orig}/page/${num}/`;
  }
}

request('https://www.smashingmagazine.com/category/css', {}, next)
  .then(acc => console.log(acc.pages.length))
  .catch(console.error);
```

## Release notes

### v2.0

* renamed `.hrefs` to `.urls` in response object
* now using [axios](https://github.com/axios/axios) instead of [needle](https://github.com/tomas/needle). Please see the axios documentation for API information.

## About

<details>
<summary><strong>Contributing</strong></summary>

Pull requests and stars are always welcome. For bugs and feature requests, [please create an issue](../../issues/new).

Please read the [contributing guide](.github/contributing.md) for advice on opening issues, pull requests, and coding standards.

</details>

<details>
<summary><strong>Running Tests</strong></summary>

Running and reviewing unit tests is a great way to get familiarized with a library and its API. You can install dependencies and run tests with the following command:

```sh
$ npm install && npm test
```

</details>

<details>
<summary><strong>Building docs</strong></summary>

_(This project's readme.md is generated by [verb](https://github.com/verbose/verb-generate-readme), please don't edit the readme directly. Any changes to the readme must be made in the [.verb.md](.verb.md) readme template.)_

To generate the readme, run the following command:

```sh
$ npm install -g verbose/verb#dev verb-generate-readme && verb
```

</details>

### Related projects

You might also be interested in these projects:

* [gists](https://www.npmjs.com/package/gists): Methods for working with the GitHub Gist API. Node.js/JavaScript | [homepage](https://github.com/jonschlinkert/gists "Methods for working with the GitHub Gist API. Node.js/JavaScript")
* [github-base](https://www.npmjs.com/package/github-base): JavaScript wrapper that greatly simplifies working with GitHub's API. | [homepage](https://github.com/jonschlinkert/github-base "JavaScript wrapper that greatly simplifies working with GitHub's API.")
* [repos](https://www.npmjs.com/package/repos): List all repositories for one or more users or orgs. | [homepage](https://github.com/jonschlinkert/repos "List all repositories for one or more users or orgs.")

### Contributors

| **Commits** | **Contributor** | 
| --- | --- |
| 8 | [jonschlinkert](https://github.com/jonschlinkert) |
| 8 | [doowb](https://github.com/doowb) |

### Author

**Jon Schlinkert**

* [LinkedIn Profile](https://linkedin.com/in/jonschlinkert)
* [GitHub Profile](https://github.com/jonschlinkert)
* [Twitter Profile](https://twitter.com/jonschlinkert)

### License

Copyright © 2018, [Jon Schlinkert](https://github.com/jonschlinkert).
Released under the [MIT License](LICENSE).

***

_This file was generated by [verb-generate-readme](https://github.com/verbose/verb-generate-readme), v0.6.0, on May 29, 2018._