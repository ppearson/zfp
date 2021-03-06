# zfp Release Notes

## 0.5.4 (October 1, 2018)

- Added support for CUDA fixed-rate compression and decompression.

- Added views into compressed arrays for thread safety, nested array
  indexing, slicing, and array subsetting.

- Added C language bindings for compressed arrays.

- Added support for compressing and decompressing 4D data.

- Changes:
  - Execution policy now applies to both compression and decompression.
  - Compressed array accessors now return Scalar type instead of
    const Scalar& to avoid stale references to evicted cache lines.

- Bug fixes:
  - Handling of negative strides.
  - Command line tool handling of arrays with more than 2^32 elements.
  - bitstream C++ compatibility.  
  - Respect minimum cache size request.


## 0.5.3 (March 28, 2018)

- Added support for OpenMP multithreaded compression (but not decompression).

- Added options for OpenMP execution to zfp command-line tool.

- Changed return value of zfp\_decompress to indicate the number of compressed
  bytes processed so far (now returns same value as zfp\_compress on success).

- Added compressed array support for copy construction and assignment via
  deep copies.

- Added virtual destructors to enable inheritance from zfp arrays.


## 0.5.2 (September 28, 2017)

- Added iterators and proxy objects for pointers and references.

- Added example illustrating how to use iterators and pointers.

- Modified diffusion example to optionally use iterators.

- Moved internal headers under array to array/zfp.

- Modified 64-bit integer typedefs to avoid the C89 non-compliant long long
  and allow for user-supplied types and literal suffixes.

- Renamed compile-time macros that did not have a ZFP prefix.

- Fixed issue with setting stream word type via CMake.

- Rewrote documentation in reStructuredText and added complete
  documentation of all public functions, classes, types, and macros.
  Removed ASCII documentation.


## 0.5.1 (March 28, 2017)

- This release primarily fixes a few minor issues but also includes
  changes in anticipation of a large number of planned future additions
  to the library.  No changes have been made to the compressed format,
  which is backwards compatible with version 0.5.0.

- Added high-level API support for integer types.

- Separated library version from CODEC version and added version string.

- Added example that illustrates in-place compression.

- Added support for CMake builds.

- Corrected inconsistent naming of BIT\_STREAM macros in code and
  documentation.

- Renamed some of the header bit mask macros.

- Added return values to stream\_skip and stream\_flush to indicate the
  number of bits skipped or output.

- Renamed stream\_block and stream\_delta to make it clear that they refer
  to strided streams.  Added missing definition of stream\_stride\_block.

- Changed int/uint types in places to use ptrdiff\_t/size\_t where
  appropriate.

- Changed API for zfp\_set\_precision and zfp\_set\_accuracy to not require
  the scalar type.

- Added missing static keyword in decode\_block.

- Changed testzfp to allow specifying which tests to perform on the
  command line.

- Fixed bug that prevented defining uninitialized arrays.

- Fixed incorrect computation of array sizes in zfp\_field\_size.

- Fixed minor issues that prevented code from compiling on Windows.

- Fixed issue with fixed-accuracy headers that caused unnecessary storage.

- Modified directory structure.

- Added documentation that discusses common issues with using zfp.


## 0.5.0 (February 29, 2016)

- Modified CODEC to more efficiently encode blocks whose values are all
  zero or are smaller in magnitude than the absolute error tolerance.
  This allows representing "empty" blocks using only one bit each.  This
  version is not backwards compatible with prior zfp versions.

- Changed behavior of zfp\_compress and zfp\_decompress to not automatically
  rewind the bit stream.  This makes it easier to concatenate multiple
  compressed bit streams, e.g. when compressing vector fields or multiple
  scalars together.

- Added functions for compactly encoding the compression parameters
  and field meta data, e.g. for producing self-contained compressed
  streams.  Also added functions for reading and writing a header
  containing these parameters.

- Changed the zfp example program interface to allow reading and writing
  compressed streams, optionally with a header.  The zfp tool can now be
  used to compress and decompress files as a stand alone utility.


## 0.4.1 (December 28, 2015)

- Fixed bug that caused segmentation fault when compressing 3D arrays
  whose dimensions are not multiples of four.  Specifically, arrays of
  dimensions nx * ny * nz, with ny not a multiple of four, were not
  handled correctly.

- Modified examples/fields.h to ensure standard compliance.  Previously,
  C99 support was needed to handle the hex float constants, which are
  not supported in C++98.

- Added simple.c as a minimal example of how to call the compressor.

- Changed compilation of diffusion example to output two executables:
  one with and one without compression.


## 0.4.0 (December 5, 2015)

- Substantial changes to the compression algorithm that improve PSNR
  by about 6 dB and speed by a factor of 2-3.  These changes are not
  backward compatible with previous versions of zfp.

- Added support for 31-bit and 63-bit integer data, as well as shorter
  integer types.

- Rewrote compression codec entirely in C to make linking and calling
  easier from other programming languages, and to expose the low-level
  interface through C instead of C++.  This necessitated significant
  changes to the API as well.

- Minor changes to the C++ compressed array API, as well as major
  implementation changes to support the C library.  The namespace and
  public types are now all in lower case.

- Deprecated support for general fixed-point decorrelating transforms
  and slimmed down implementation.

- Added new examples for evaluating the throughput of the (de)compressor
  and for compressing grayscale images in the pgm format.

- Added FAQ.


## 0.3.2 (December 3, 2015)

- Fixed bug in Array::get() that caused the wrong cached block to be
  looked up, thus occasionally copying incorrect values back to parts
  of the array.


## 0.3.1 (May 6, 2015)

- Fixed rare bug caused by exponent underflow in blocks with no normal
  and some denormal numbers.


## 0.3.0 (March 3, 2015)

- Modified the default decorrelating transform to one that uses only
  additions and bit shifts.  This new transform, in addition to being
  faster, also has some theoretical optimality properties and tends to
  improve rate distortion.

- Added compile-time support for parameterized transforms, e.g. to
  support other popular transforms like DCT, HCT, and Walsh-Hadamard.

- Made forward transform range preserving: (-1, 1) is mapped to (-1, 1).
  Consequently Q1.62 fixed point can be used throughout.

- Changed the order in which bits are emitted within each bit plane
  to be more intelligent.  Group tests are now deferred until they
  are needed, i.e. just before the value bits for the group being
  tested.  This improves the quality of fixed-rate encodings, but
  has no impact on compressed size.

- Made several optimizations to improve performance.

- Added floating-point traits to reduce the number of template
  parameters.  It is now possible to declare a 3D array as
  Array3<float>, for example.

- Added functions for setting the array scalar type and dimensions.

- Consolidated several header files.

- Added testzfp for regression testing.


## 0.2.1 (December 12, 2014)

- Added Win64 support via Microsoft Visual Studio compiler.

- Fixed broken support for IBM's xlc compiler.

- Made several minor changes to suppress compiler warnings.

- Documented expected output for the diffusion example.


## 0.2.0 (December 2, 2014)

- The compression interface from zfpcompress was relocated to a
  separate library, called libzfp, and modified to be callable from C.
  This API now uses a parameter object (zfp\_params) to specify array
  type and dimensions as well as compression parameters.

- Several utility functions were added to simplify libzfp usage:

  * Functions for setting the rate, precision, and accuracy.
    Corresponding functions were also added to the Codec class.

  * A function for estimating the buffer size needed for compression.

- The Array class functionality was expanded:

  * Support for accessing the compressed bit stream stored with an
    array, e.g. for offline compressed storage and for initializing
    an already compressed array.

  * Functions for dynamically specifying the cache size.

  * The default cache is now direct-mapped instead of two-way
    associative.

- Minor bug fixes:

  * Corrected the value of the lowest possible bit plane to account for
    both the smallest exponent and the number of bits in the significand.

  * Corrected inconsistent use of rate and precision.  The rate refers
    to the number of compressed bits per floating-point value, while
    the precision refers to the number of uncompressed bits.  The Array
    API was changed accordingly.


## 0.1.0 (November 12, 2014)

- Initial beta release.
