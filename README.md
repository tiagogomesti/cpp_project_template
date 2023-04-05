# Template to create a cpp project with GTest, GCOV and LCOV configured

## How to build
```shell
mkdir build
cmake ..
make
make coverage
```

The coverage report will be created on `build/coverage/index.html`