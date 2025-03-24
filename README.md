# edu-stdout

### CMakeLists.txt (Project Structure) !heredoc

```bash
cat > CMakeLists.txt << EOF
cmake_minimum_required(VERSION 3.16)
project(myproject LANGUAGES CXX)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Add CLI11 library for argument parsing
include(FetchContent)
FetchContent_Declare(
    CLI11
    GIT_REPOSITORY https://github.com/CLIUtils/CLI11.git
    GIT_TAG v2.3.2
)
FetchContent_MakeAvailable(CLI11)

# Add the greetings library
add_subdirectory(lib)

# Add the executable target (greeter)
add_subdirectory(src)

# Install the greeter binary (must come after add_subdirectory(src))
install(TARGETS greeter DESTINATION bin)
EOF
```

### src/CMakeLists.txt (Executable) !heredoc

```bash
cat > ./src/CMakeLists.txt << EOF
add_executable(greeter main.cpp)
target_link_libraries(greeter PRIVATE greetings CLI11::CLI11)
target_include_directories(greeter PRIVATE ${CMAKE_INSTALL_PREFIX}/include)
EOF
```

### src/main.cpp !heredoc

```bash
cat > ./src/main.cpp << EOF
#include <iostream>
#include <string>
#include "./lib/greetings.h"
#include "CLI/CLI.hpp"

int main(int argc, char* argv[]) {
    CLI::App app{"Greeter App"};

    bool uppercase = false;
    bool verbose = false;

    // Add short and long command-line options
    app.add_flag("-u,--uppercase", uppercase, "Print the greeting in uppercase");
    app.add_flag("-v,--verbose", verbose, "Print a verbose greeting");

    // Parse the command-line arguments
    CLI11_PARSE(app, argc, argv);

    std::string name;
    std::cout << "What is your name? ";
    std::getline(std::cin, name);

    // Handle the options
    if (uppercase) {
        uppercase_greet_user(name);
    } else if (verbose) {
        verbose_greet_user(name);
    } else {
        greet_user(name);
    }

    return 0;
}

EOF
```

### Build the project

```bash
cmake -B build
make -C build
./bin/greeter
sudo make -C build install
greeter --uppercase
greeter --verbose
greeter --help
```
