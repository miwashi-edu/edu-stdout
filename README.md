# edu-stdout

## Premises

## Instructions

### scaffold project

```bash
cd ~
cd ws
cd edu-stdin
touch ./lib/greetings.h
touch ./lib/greetings.c
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
