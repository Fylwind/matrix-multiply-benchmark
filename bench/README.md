# Instructions

The program might crash if there is insufficient stack space.  To avoid this, run:

~~~sh
ulimit -s 1000000 # allow ~1GB of stack space
~~~

To build the program only:

~~~sh
make build
~~~

To build and generate all the reports:

~~~sh
make
~~~

To build and run with a specific compiler such as `clang`, specify:

~~~sh
make CC=clang
~~~

To test a BLAS implementation, specify:

~~~sh
OMP_NUM_THREADS=1 make CPPFLAGS=-DUSE_BLAS LIBS="-l<blas> -lm"
~~~

where `<blas>` is a placeholder for the name of the BLAS library (e.g. for
OpenBLAS the flag would be written `-lopenblas`).  The `OMP_NUM_THREADS=1`
disables OpenMP for the BLAS implementation so we can test single-threaded
performance.

To test a different implementation, you will need to compile a BLAS-like
wrapper and then link with that.  For example, this is how you can test Eigen:

~~~sh
(cd wrapper && make)
make CPPFLAGS="-DUSE_BLAS -Iwrapper" LIBS="wrapper/eigen.o -lstdc++ -lm"
~~~

Note that since we are wrapping a C++ library, `-lstdc++` is required.

To clear all the data gathered and other autogenerated files, use:

~~~sh
make clean
~~~