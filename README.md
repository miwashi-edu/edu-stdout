# edu-stdout

## If ssh has stopped

```bash
docker exec -it rpi5-dev bash -c "service ssh restart"
```


### src/main.cpp !heredoc

```bash
cat > ./src/main.cpp << EOF
#include <iostream>
#include <fstream>
#include <sstream>
#include <string>
#include "./lib/greetings.h"

int main(int argc, char* argv[]) {
    if (argc < 2) {
        std::cerr << "Usage: " << argv[0] << " <input_file>" << std::endl;
        return 1;
    }

    std::string filename = argv[1];
    std::ifstream infile(filename);
    if (!infile) {
        std::cerr << "Could not open file: " << filename << std::endl;
        return 1;
    }

    std::string line;
    while (std::getline(infile, line)) {
        std::istringstream iss(line);
        std::string name, flag;
        bool uppercase = false;
        bool verbose = false;

        if (!(iss >> name)) continue;

        while (iss >> flag) {
            if (flag == "-u" || flag == "--uppercase") {
                uppercase = true;
            } else if (flag == "-v" || flag == "--verbose") {
                verbose = true;
            }
        }

        if (uppercase) {
            uppercase_greet_user(name);
        } else if (verbose) {
            verbose_greet_user(name);
        } else {
            greet_user(name);
        }
    }

    return 0;
}
EOF
```

### greets.txt

```
cat > greets.txt << 'EOF'
Alice --uppercase
Bob --verbose
Charlie
Diana -u
Ethan -v
Fiona --verbose --uppercase
George
Hannah -v
Ian --uppercase
Julia
Kevin -u
Laura --verbose
Mike
Nina -u --verbose
Oscar --uppercase
Paula
Quentin --verbose
Rachel --uppercase --verbose
Steve
Tina -v
Uma -u
Victor
Wendy -v --uppercase
Xavier
Yara --verbose
Zach -u
EOF
```

### Build the project

```bash
cmake -B build
sudo make -C build install
greeter greets.txt
```
