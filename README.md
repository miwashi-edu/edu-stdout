# edu-stdout

## Premises

## Instructions

### scaffold project

```bash
cd ~
cd ws
cd edu-stdin
touch ./lib/greetings.h
touch ./lib/greetings.cpp
```

### lib/CmakeLists.txt !heredoc

```bash
cat > ./lib/greetings.h << EOF
cmake_minimum_required(VERSION 3.16)
project(myproject LANGUAGES CXX)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_subdirectory(lib)
add_subdirectory(src)  # ✅ Ensure `greeter` is defined before install()
install(TARGETS greeter DESTINATION bin)
EOF
```

### lib/greetings.h !heredoc

```bash
cat > ./lib/greetings.h << EOF
#ifndef GREETINGS_H
#define GREETINGS_H

#include <string>

void greet_user(const std::string& name);
void verbose_greet_user(const std::string& name);
void uppercase_greet_user(const std::string& name);

#endif // GREETINGS_H
EOF
```


### lib/greetings.cpp !heredoc

```bash
cat > ./lib/greetings.cpp << EOF
#include <iostream>
#include "greetings.h"

void greet_user(const std::string& name) {
    std::cout << "Hello, " << name << "!" << std::endl;
}

void verbose_greet_user(const std::string& name) {
    std::cout << "Greetings, esteemed " << name << "! It is a pleasure to present you with this message: Hello!" << std::endl;
}

void uppercase_greet_user(const std::string& name) {
    std::cout << "HELLO, " << name << "!" << std::endl;
}
EOF
```

### Build the project

```bash
cmake -B build
make -C build
./build/hello
```

### Reset to commit or delete myproject

#### Delete project
```bash
cd ~
cd ws
rm -rf edu-stdin
```

#### Reset to commit
```bash
cd ~
cd ws
cd edu-stdin
git reset --hard
git clean -df
```

# Repeat until dead or know it --> scaffold project
