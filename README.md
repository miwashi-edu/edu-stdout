# edu-stdout

### src/main.cpp !heredoc

```bash
cat > ./src/main.cpp << EOF
#include <iostream>
#include <string>
#include <sstream>
#include <vector>
#include <unistd.h> // for isatty, fileno
#include "../lib/greetings.h"
#include "CLI/CLI.hpp"

int run_with_args(int argc, char* argv[]) {
    CLI::App app{"Greeter App"};

    bool uppercase = false;
    bool verbose = false;
    std::string name;

    app.add_flag("-u,--uppercase", uppercase, "Print the greeting in uppercase");
    app.add_flag("-v,--verbose", verbose, "Print a verbose greeting");
    app.add_option("name", name, "Name to greet")->required();

    CLI11_PARSE(app, argc, argv);

    if (uppercase) {
        uppercase_greet_user(name);
    } else if (verbose) {
        verbose_greet_user(name);
    } else {
        greet_user(name);
    }

    return 0;
}

int main(int argc, char* argv[]) {
    if (isatty(fileno(stdin))) {
        // Interactive or command-line mode
        return run_with_args(argc, argv);
    } else {
        // Piped mode: process each line as a separate command
        std::string line;
        while (std::getline(std::cin, line)) {
            std::istringstream iss(line);
            std::vector<std::string> tokens;
            std::string token;
            while (iss >> token) {
                tokens.push_back(token);
            }
            if (tokens.empty()) continue;

            std::vector<char*> argv_line;
            argv_line.push_back((char*)"greet"); // fake argv[0]
            for (auto& str : tokens) {
                argv_line.push_back(const_cast<char*>(str.c_str()));
            }

            // Call parser on this synthetic argc/argv
            run_with_args(static_cast<int>(argv_line.size()), argv_line.data());
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
