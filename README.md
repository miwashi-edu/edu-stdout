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

### scaffold project

```bash
cd ~
cd ws
mkdir -p edu-stdin
cd edu-stdin
mkdir src
mkdir include
mkdir tests
mkdir lib
mkdir build
touch CMakeLists.txt
touch ./src/CMakeLists.txt
touch ./src/main.cpp
```

### Set up git

```bash
curl -o .gitignore https://raw.githubusercontent.com/github/gitignore/main/C%2B%2B.gitignore
git init
git branch -m main # if you didn't change git to use main instead of maste
git add .
git commit -m "Initial Commit"
```

### CMakeLists.txt (Project Structure) !heredoc

```bash
cat > CMakeLists.txt << EOF
cmake_minimum_required(VERSION 3.16)
project(edu-stdin LANGUAGES CXX)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY \${CMAKE_SOURCE_DIR}/bin)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_subdirectory(src)

install(TARGETS hello DESTINATION bin)
EOF
```

### src/CMakeLists.txt (Executable) !heredoc

```bash
cat > ./src/CMakeLists.txt << EOF
add_executable(hello main.cpp)
EOF
```

### src/main.cpp !heredoc

```bash
cat > ./src/main.cpp << EOF
#include <iostream>
#include <string>

int main() {
    std::string name;
    while (true) {
        std::cout << "What is your name? ";
        std::getline(std::cin, name);
        if (name == "quit") {
            break;
        }
        std::cout << "Hello, " << name << "!" << std::endl;
    }
    return 0;
}
EOF
```

### src/main.c !heredoc

> The c file isn't used, it is here for comparison with .cpp.

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
