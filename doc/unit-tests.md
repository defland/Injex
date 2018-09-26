Compiling/running unit tests
------------------------------------

Unit tests will be automatically compiled if dependencies were met in `./configure`
and tests weren't explicitly disabled.

After configuring, they can be run with `make check`.

To run the injexd tests manually, launch `src/test/test_injex`.

To add more injexd tests, add `BOOST_AUTO_TEST_CASE` functions to the existing
.cpp files in the `test/` directory or add new .cpp files that
implement new BOOST_AUTO_TEST_SUITE sections.

To run the injex-qt tests manually, launch `src/qt/test/test_injex-qt`

To add more injex-qt tests, add them to the `src/qt/test/` directory and
the `src/qt/test/test_main.cpp` file.
