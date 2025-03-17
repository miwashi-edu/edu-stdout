# edu-stdout

## Instructions

### CMakeLists.txt (Project Structure) !heredoc

```bash
cat > CMakeLists.txt << EOF
cmake_minimum_required(VERSION 3.16)
project(myproject LANGUAGES CXX)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_subdirectory(lib)
add_subdirectory(src)
install(TARGETS greeter DESTINATION bin)
EOF
```

### src/CMakeLists.txt (Executable) !heredoc

```bash
cat > ./src/CMakeLists.txt << EOF
add_executable(greeter main.cpp)
target_link_libraries(greeter PRIVATE greetings)
target_include_directories(greeter PRIVATE ${CMAKE_INSTALL_PREFIX}/include)
EOF
```

### src/main.cpp !heredoc

```bash
cat > ./src/main.cpp << EOF
#include <iostream>
#include <string>
#include <cstdlib>
#include "../lib/greetings.h"

int main(int argc, char* argv[]) {
    std::string name;
    std::cout << "What is your name? ";
    std::getline(std::cin, name);

    if (argc < 2) {
        greet_user(name);
    } else {
        std::string arg = argv[1];
        if (arg == "--uppercase") {
            uppercase_greet_user(name);
        } else if (arg == "--verbose") {
            verbose_greet_user(name);
        } else {
            std::cerr << "Unknown option: " << arg << std::endl;
            std::cerr << "Usage: " << argv[0] << " [--uppercase | --verbose]" << std::endl;
            return EXIT_FAILURE;
        }
    }
    return EXIT_SUCCESS;
}
EOF
```

### src/main.c !heredoc

```bash
cat > ./src/main.c << EOF
#include <stdio.h>
#include <string.h>

int main() {
    char name[100];

    while (1) {
        printf("What is your name? ");
        fgets(name, sizeof(name), stdin);

        name[strcspn(name, "\n")] = 0;

        if (strcmp(name, "quit") == 0) {
            break;
        }
        printf("Hello, %s!\n", name);
    }

    return 0;
}
EOF
```

### Build the project

```bash
cmake -B build
make -C build
./bin/hello
```

### Reset to commit or delete myproject

#### Delete project
```bash
cd ~
cd ws
rm -rf myproject
```

#### Reset to commit
```bash
cd ~
cd ws
cd myproject
git reset --hard
git clean -df
```

# Repeat until dead or know it --> scaffold project
