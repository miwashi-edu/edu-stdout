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

```bash
cat > ./lib/CMakeLists.txt << EOF
add_library(greetings STATIC greetings.cpp)

# Ensure the header is found by targets using the library
target_include_directories(greetings PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
EOF
```

```bash
cat > CMakeLists.txt << EOF
cmake_minimum_required(VERSION 3.16)
project(myproject LANGUAGES CXX)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY \${CMAKE_SOURCE_DIR}/bin)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_subdirectory(lib)
add_subdirectory(src)

install(TARGETS hello1 hello2 DESTINATION bin) # Added install target
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
