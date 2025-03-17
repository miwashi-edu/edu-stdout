# edu-stdout

[C++ Wikipedia](https://en.wikipedia.org/wiki/C%2B%2B)  
[C++ History](https://en.cppreference.com/w/cpp/language/history)

## Premises

## Login

```
ssh [user]@localhost -p 2222
# ctrl-d to end session
```

## Instructions

### ./lib/CMakeLists.txt < !heredoc

```bash
cat > ./lib/CMakeLists.txt << EOF
add_executable(hello1 main1.cpp)
add_executable(hello2 main2.cpp)
add_executable(hello3 main3.cpp)

target_link_libraries(hello1 PRIVATE greetings)
target_link_libraries(hello2 PRIVATE greetings)
target_link_libraries(hello3 PRIVATE greetings)

target_include_directories(hello1 PRIVATE ${CMAKE_SOURCE_DIR}/lib)
target_include_directories(hello2 PRIVATE ${CMAKE_SOURCE_DIR}/lib)
target_include_directories(hello3 PRIVATE ${CMAKE_SOURCE_DIR}/lib)
EOF
```

### CMakeLists.txt < !heredoc

```bash
cat > CMakeLists.txt << EOF
cmake_minimum_required(VERSION 3.16)
project(myproject LANGUAGES CXX)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Add the greetings library
add_subdirectory(lib)

# Add the source directory (hello1, hello2, hello3 executables)
add_subdirectory(src)

# Install all three executables
install(TARGETS hello1 hello2 hello3 DESTINATION bin)
EOF
```

### ./src/main1.cpp < !heredoc

```bash
cat > ./src/main1.cpp << EOF
#include <iostream>
#include "greetings.h"

int main() {
    std::string name;
    std::cout << "What is your name? ";
    std::getline(std::cin, name);

    greet_user(name);
    return 0;
}
EOF
```

### ./src/main2.cpp < !heredoc

```bash
cat > ./src/main2.cpp << EOF
#include <iostream>
#include "greetings.h"

int main() {
    std::string name;
    std::cout << "What is your name? ";
    std::getline(std::cin, name);

    verbose_greet_user(name);
    return 0;
}
EOF
```

### ./src/main3.cpp < !heredoc

```bash
cat > ./src/main3.cpp << EOF
#include <iostream>
#include "greetings.h"

int main() {
    std::string name;
    std::cout << "What is your name? ";
    std::getline(std::cin, name);

    uppercase_greet_user(name);
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
