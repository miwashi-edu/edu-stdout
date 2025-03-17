# edu-stdout

### CMakeLists.txt (Project Structure) !heredoc

```bash
cat > CMakeLists.txt << EOF
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
#include "../lib/greetings.h"
#include "CLI/CLI.hpp"

int main(int argc, char* argv[]) {
    CLI::App app{"Greeter App"};

    bool uppercase = false;
    bool verbose = false;

    app.add_flag("--uppercase", uppercase, "Print the greeting in uppercase");
    app.add_flag("--verbose", verbose, "Print a verbose greeting");

    CLI11_PARSE(app, argc, argv);

    std::string name;
    std::cout << "What is your name? ";
    std::getline(std::cin, name);

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
./bin/greeting
sudo make -C build install
greeting
```
