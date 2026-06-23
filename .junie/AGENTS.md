# Project Development Guide

This document provides essential information for advanced developers working on the Osiris project.

## Build and Configuration

The project uses CMake (minimum version 3.24) and requires a C++20 compliant compiler.

### Build Instructions

1.  **Configure the project**:
    Tests are optional and disabled by default. Use the `ENABLE_TESTS` cache variable to enable them.
    ```bash
    cmake -B <build-dir> -DENABLE_TESTS="unit;functional"
    ```

2.  **Build the project**:
    To build the main library and tests:
    ```bash
    cmake --build <build-dir> --target Osiris UnitTests
    ```

### Profiles
Existing profiles in this environment:
- **Debug**: `cmake-build-debug`
- **Release**: `cmake-build-release`

## Testing Information

### Configuring and Running Tests

Tests are divided into `unit` and `functional` suites.

1.  **Enable Tests**: Pass `-DENABLE_TESTS=unit` (or `functional`, or both) during configuration.
2.  **Run Tests**:
    After building the `UnitTests` target, the binary is located at `<build-dir>/Tests/UnitTests/UnitTestsBin`.
    ```bash
    ./cmake-build-debug/Tests/UnitTests/UnitTestsBin
    ```
    You can use GTest filters to run specific tests:
    ```bash
    ./cmake-build-debug/Tests/UnitTests/UnitTestsBin --gtest_filter=CountrZero.*
    ```

### Adding New Tests

1.  Create a new `.cpp` file in the appropriate subdirectory of `Tests/UnitTests/`.
2.  Add the file to the corresponding `CMakeLists.txt` using `target_sources(UnitTests PRIVATE ...)`.
3.  Use GoogleTest macros (`TEST`, `TEST_P`, etc.) to define your tests.

### Simple Test Example

To verify the testing setup, you can create `Tests/UnitTests/Utils/DemoTests.cpp`:

```cpp
#include <gtest/gtest.h>

TEST(DemoTest, SimpleAssertion) {
    EXPECT_EQ(1 + 1, 2);
}
```

And update `Tests/UnitTests/Utils/CMakeLists.txt`:

```cmake
target_sources(UnitTests PRIVATE
  ...
  DemoTests.cpp
)
```

## Additional Development Information

- **Code Style**: The project follows C++20 standards. It uses `#pragma once` for header guards and organizes code within relevant namespaces (e.g., `bits`).
- **Static Analysis**: A `.clang-tidy` file is present in the root, primarily enforcing cognitive complexity limits.
- **Platform Specifics**: The project includes platform-specific code (e.g., in `Source/Platform`). Use `IS_WIN64()` and `IS_LINUX()` macros for conditional compilation.
- **Memory Management**: Utilities like `RefCountedHook.h` and `ManuallyDestructible.h` are used for specialized memory handling.
