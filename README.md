<!--

@license Apache-2.0

Copyright (c) 2024 The Stdlib Authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

-->


<details>
  <summary>
    About stdlib...
  </summary>
  <p>We believe in a future in which the web is a preferred environment for numerical computation. To help realize this future, we've built stdlib. stdlib is a standard library, with an emphasis on numerical and scientific computation, written in JavaScript (and C) for execution in browsers and in Node.js.</p>
  <p>The library is fully decomposable, being architected in such a way that you can swap out and mix and match APIs and functionality to cater to your exact preferences and use cases.</p>
  <p>When you use stdlib, you can be absolutely certain that you are using the most thorough, rigorous, well-written, studied, documented, tested, measured, and high-quality code out there.</p>
  <p>To join us in bringing numerical computing to the web, get started by checking us out on <a href="https://github.com/stdlib-js/stdlib">GitHub</a>, and please consider <a href="https://opencollective.com/stdlib">financially supporting stdlib</a>. We greatly appreciate your continued support!</p>
</details>

# ddot

[![NPM version][npm-image]][npm-url] [![Build Status][test-image]][test-url] [![Coverage Status][coverage-image]][coverage-url] <!-- [![dependencies][dependencies-image]][dependencies-url] -->

> Compute the dot product of `x` and `y`.

<section class="installation">

## Installation

```bash
npm install @stdlib/blas-base-ddot-wasm
```

Alternatively,

-   To load the package in a website via a `script` tag without installation and bundlers, use the [ES Module][es-module] available on the [`esm`][esm-url] branch (see [README][esm-readme]).
-   If you are using Deno, visit the [`deno`][deno-url] branch (see [README][deno-readme] for usage intructions).
-   For use in Observable, or in browser/node environments, use the [Universal Module Definition (UMD)][umd] build available on the [`umd`][umd-url] branch (see [README][umd-readme]).

The [branches.md][branches-url] file summarizes the available branches and displays a diagram illustrating their relationships.

To view installation and usage instructions specific to each branch build, be sure to explicitly navigate to the respective README files on each branch, as linked to above.

</section>

<section class="usage">

## Usage

```javascript
var ddot = require( '@stdlib/blas-base-ddot-wasm' );
```

#### ddot.main( N, x, strideX, y, strideY )

Computes the dot product of `x` and `y`.

```javascript
var Float64Array = require( '@stdlib/array-float64' );

var x = new Float64Array( [ 4.0, 2.0, -3.0, 5.0, -1.0 ] );
var y = new Float64Array( [ 2.0, 6.0, -1.0, -4.0, 8.0 ] );

var z = ddot.main( x.length, x, 1, y, 1 );
// returns -5.0
```

The function has the following parameters:

-   **N**: number of indexed elements.
-   **x**: first input [`Float64Array`][@stdlib/array/float64].
-   **strideX**: index increment for `x`.
-   **y**: second input [`Float64Array`][@stdlib/array/float64].
-   **strideY**: index increment for `y`.

The `N` and stride parameters determine which elements in the strided arrays are accessed at runtime. For example, to calculate the dot product of every other value in `x` and the first `N` elements of `y` in reverse order,

```javascript
var Float64Array = require( '@stdlib/array-float64' );

var x = new Float64Array( [ 1.0, 2.0, 3.0, 4.0, 5.0, 6.0 ] );
var y = new Float64Array( [ 1.0, 1.0, 1.0, 1.0, 1.0, 1.0 ] );

var z = ddot.main( 3, x, 2, y, -1 );
// returns 9.0
```

Note that indexing is relative to the first index. To introduce an offset, use [`typed array`][mdn-typed-array] views.

<!-- eslint-disable stdlib/capitalized-comments -->

```javascript
var Float64Array = require( '@stdlib/array-float64' );

// Initial arrays...
var x0 = new Float64Array( [ 1.0, 2.0, 3.0, 4.0, 5.0, 6.0 ] );
var y0 = new Float64Array( [ 7.0, 8.0, 9.0, 10.0, 11.0, 12.0 ] );

// Create offset views...
var x1 = new Float64Array( x0.buffer, x0.BYTES_PER_ELEMENT*1 ); // start at 2nd element
var y1 = new Float64Array( y0.buffer, y0.BYTES_PER_ELEMENT*3 ); // start at 4th element

var z = ddot.main( 3, x1, -2, y1, 1 );
// returns 128.0
```

#### ddot.ndarray( N, x, strideX, offsetX, y, strideY, offsetY )

Computes the dot product of `x` and `y` using alternative indexing semantics.

```javascript
var Float64Array = require( '@stdlib/array-float64' );

var x = new Float64Array( [ 4.0, 2.0, -3.0, 5.0, -1.0 ] );
var y = new Float64Array( [ 2.0, 6.0, -1.0, -4.0, 8.0 ] );

var z = ddot.ndarray( x.length, x, 1, 0, y, 1, 0 );
// returns -5.0
```

The function has the following additional parameters:

-   **offsetX**: starting index for `x`.
-   **offsetY**: starting index for `y`.

While [`typed array`][mdn-typed-array] views mandate a view offset based on the underlying buffer, the offset parameters support indexing semantics based on starting indices. For example, to calculate the dot product of every other value in `x` starting from the second value with the last 3 elements in `y` in reverse order

```javascript
var Float64Array = require( '@stdlib/array-float64' );

var x = new Float64Array( [ 1.0, 2.0, 3.0, 4.0, 5.0, 6.0 ] );
var y = new Float64Array( [ 7.0, 8.0, 9.0, 10.0, 11.0, 12.0 ] );

var z = ddot.ndarray( 3, x, 2, 1, y, -1, y.length-1 );
// returns 128.0
```

* * *

### Module

#### ddot.Module( memory )

Returns a new WebAssembly [module wrapper][@stdlib/wasm/module-wrapper] instance which uses the provided WebAssembly [memory][@stdlib/wasm/memory] instance as its underlying memory.

<!-- eslint-disable node/no-sync -->

```javascript
var Memory = require( '@stdlib/wasm-memory' );

// Create a new memory instance with an initial size of 10 pages (640KiB) and a maximum size of 100 pages (6.4MiB):
var mem = new Memory({
    'initial': 10,
    'maximum': 100
});

// Create a BLAS routine:
var mod = new ddot.Module( mem );
// returns <Module>

// Initialize the routine:
mod.initializeSync();
```

#### ddot.Module.prototype.main( N, xp, sx, yp, sy )

Computes the dot product of `x` and `y`.

<!-- eslint-disable node/no-sync -->

```javascript
var Memory = require( '@stdlib/wasm-memory' );
var oneTo = require( '@stdlib/array-one-to' );
var ones = require( '@stdlib/array-ones' );
var zeros = require( '@stdlib/array-zeros' );
var bytesPerElement = require( '@stdlib/ndarray-base-bytes-per-element' );

// Create a new memory instance with an initial size of 10 pages (640KiB) and a maximum size of 100 pages (6.4MiB):
var mem = new Memory({
    'initial': 10,
    'maximum': 100
});

// Create a BLAS routine:
var mod = new ddot.Module( mem );
// returns <Module>

// Initialize the routine:
mod.initializeSync();

// Define a vector data type:
var dtype = 'float64';

// Specify a vector length:
var N = 5;

// Define pointers (i.e., byte offsets) for storing two vectors:
var xptr = 0;
var yptr = N * bytesPerElement( dtype );

// Write vector values to module memory:
mod.write( xptr, oneTo( N, dtype ) );
mod.write( yptr, ones( N, dtype ) );

// Perform computation:
var z = mod.main( N, xptr, 1, yptr, 1 );

console.log( z );
```

The function has the following parameters:

-   **N**: number of indexed elements.
-   **xp**: first input [`Float64Array`][@stdlib/array/float64] pointer (i.e., byte offset).
-   **sx**: index increment for `x`.
-   **yp**: second input [`Float64Array`][@stdlib/array/float64] pointer (i.e., byte offset).
-   **sy**: index increment for `y`.

#### ddot.Module.prototype.ndarray( N, xp, sx, ox, yp, sy, oy )

Computes the dot product of `x` and `y` using alternative indexing semantics.

<!-- eslint-disable node/no-sync -->

```javascript
var Memory = require( '@stdlib/wasm-memory' );
var oneTo = require( '@stdlib/array-one-to' );
var ones = require( '@stdlib/array-ones' );
var zeros = require( '@stdlib/array-zeros' );
var bytesPerElement = require( '@stdlib/ndarray-base-bytes-per-element' );

// Create a new memory instance with an initial size of 10 pages (640KiB) and a maximum size of 100 pages (6.4MiB):
var mem = new Memory({
    'initial': 10,
    'maximum': 100
});

// Create a BLAS routine:
var mod = new ddot.Module( mem );
// returns <Module>

// Initialize the routine:
mod.initializeSync();

// Define a vector data type:
var dtype = 'float64';

// Specify a vector length:
var N = 5;

// Define pointers (i.e., byte offsets) for storing two vectors:
var xptr = 0;
var yptr = N * bytesPerElement( dtype );

// Write vector values to module memory:
mod.write( xptr, oneTo( N, dtype ) );
mod.write( yptr, ones( N, dtype ) );

// Perform computation:
var z = mod.ndarray( N, xptr, 1, 0, yptr, 1, 0 );

console.log( z );
```

The function has the following additional parameters:

-   **ox**: starting index for `x`.
-   **oy**: starting index for `y`.

</section>

<!-- /.usage -->

<section class="notes">

* * *

## Notes

-   If `N <= 0`, both `main` and `ndarray` methods return `0.0`.
-   This package implements routines using WebAssembly. When provided arrays which are not allocated on a `ddot` module memory instance, data must be explicitly copied to module memory prior to computation. Data movement may entail a performance cost, and, thus, if you are using arrays external to module memory, you should prefer using [`@stdlib/blas-base/ddot`][@stdlib/blas/base/ddot]. However, if working with arrays which are allocated and explicitly managed on module memory, you can achieve better performance when compared to the pure JavaScript implementations found in [`@stdlib/blas/base/ddot`][@stdlib/blas/base/ddot]. Beware that such performance gains may come at the cost of additional complexity when having to perform manual memory management. Choosing between implementations depends heavily on the particular needs and constraints of your application, with no one choice universally better than the other.
-   `ddot()` corresponds to the [BLAS][blas] level 1 function [`ddot`][ddot].

</section>

<!-- /.notes -->

<section class="examples">

* * *

## Examples

<!-- eslint no-undef: "error" -->

```javascript
var discreteUniform = require( '@stdlib/random-array-discrete-uniform' );
var ddot = require( '@stdlib/blas-base-ddot-wasm' );

var opts = {
    'dtype': 'float64'
};
var x = discreteUniform( 10, 0, 100, opts );
console.log( x );

var y = discreteUniform( x.length, 0, 10, opts );
console.log( y );

var z = ddot.ndarray( x.length, x, 1, 0, y, -1, y.length-1 );
console.log( z );
```

</section>

<!-- /.examples -->

<!-- Section for related `stdlib` packages. Do not manually edit this section, as it is automatically populated. -->

<section class="related">

</section>

<!-- /.related -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->


<section class="main-repo" >

* * *

## Notice

This package is part of [stdlib][stdlib], a standard library for JavaScript and Node.js, with an emphasis on numerical and scientific computing. The library provides a collection of robust, high performance libraries for mathematics, statistics, streams, utilities, and more.

For more information on the project, filing bug reports and feature requests, and guidance on how to develop [stdlib][stdlib], see the main project [repository][stdlib].

#### Community

[![Chat][chat-image]][chat-url]

---

## License

See [LICENSE][stdlib-license].


## Copyright

Copyright &copy; 2016-2024. The Stdlib [Authors][stdlib-authors].

</section>

<!-- /.stdlib -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="links">

[npm-image]: http://img.shields.io/npm/v/@stdlib/blas-base-ddot-wasm.svg
[npm-url]: https://npmjs.org/package/@stdlib/blas-base-ddot-wasm

[test-image]: https://github.com/stdlib-js/blas-base-ddot-wasm/actions/workflows/test.yml/badge.svg?branch=main
[test-url]: https://github.com/stdlib-js/blas-base-ddot-wasm/actions/workflows/test.yml?query=branch:main

[coverage-image]: https://img.shields.io/codecov/c/github/stdlib-js/blas-base-ddot-wasm/main.svg
[coverage-url]: https://codecov.io/github/stdlib-js/blas-base-ddot-wasm?branch=main

<!--

[dependencies-image]: https://img.shields.io/david/stdlib-js/blas-base-ddot-wasm.svg
[dependencies-url]: https://david-dm.org/stdlib-js/blas-base-ddot-wasm/main

-->

[chat-image]: https://img.shields.io/gitter/room/stdlib-js/stdlib.svg
[chat-url]: https://app.gitter.im/#/room/#stdlib-js_stdlib:gitter.im

[stdlib]: https://github.com/stdlib-js/stdlib

[stdlib-authors]: https://github.com/stdlib-js/stdlib/graphs/contributors

[umd]: https://github.com/umdjs/umd
[es-module]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules

[deno-url]: https://github.com/stdlib-js/blas-base-ddot-wasm/tree/deno
[deno-readme]: https://github.com/stdlib-js/blas-base-ddot-wasm/blob/deno/README.md
[umd-url]: https://github.com/stdlib-js/blas-base-ddot-wasm/tree/umd
[umd-readme]: https://github.com/stdlib-js/blas-base-ddot-wasm/blob/umd/README.md
[esm-url]: https://github.com/stdlib-js/blas-base-ddot-wasm/tree/esm
[esm-readme]: https://github.com/stdlib-js/blas-base-ddot-wasm/blob/esm/README.md
[branches-url]: https://github.com/stdlib-js/blas-base-ddot-wasm/blob/main/branches.md

[stdlib-license]: https://raw.githubusercontent.com/stdlib-js/blas-base-ddot-wasm/main/LICENSE

[blas]: http://www.netlib.org/blas

[ddot]: http://www.netlib.org/lapack/explore-html/de/da4/group__double__blas__level1.html

[mdn-typed-array]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypedArray

[@stdlib/array/float64]: https://github.com/stdlib-js/array-float64

[@stdlib/wasm/memory]: https://github.com/stdlib-js/wasm-memory

[@stdlib/wasm/module-wrapper]: https://github.com/stdlib-js/wasm-module-wrapper

[@stdlib/blas/base/ddot]: https://github.com/stdlib-js/blas-base-ddot

</section>

<!-- /.links -->