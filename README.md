# edu-stdout

### src/main.cpp !heredoc

```bash
cat > ./src/main.cpp << EOF
#include <iostream>
#include <string>
#include <sstream>
#include <unistd.h> // for isatty, fileno
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

    if (isatty(fileno(stdin))) {
        // Interactive mode
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
    } else {
        // Input from pipe or redirected file
        std::string line;
        while (std::getline(std::cin, line)) {
            if (line.empty()) continue;

            if (uppercase) {
                uppercase_greet_user(line);
            } else if (verbose) {
                verbose_greet_user(line);
            } else {
                greet_user(line);
            }
        }
    }

    return 0;
}
EOF
```

### Build the project

```bash
sudo make -C build install
cat greets.txt | greeter
```
