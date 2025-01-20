# The workflow of C++


If you work with C++, don't be overwhelmed, there will be a lot of configurations.



[C/C++ for Visual Studio Code](https://code.visualstudio.com/docs/languages/cpp)


First of all, choose the compiler from 
Windows(MSVC, MinGW)
Linus(GCC, LLVM)
MacOS(LLVM)


### Windows MSVC
I am so sick of configuring C++ for visual studio code on Windows. 


1. **IMPORTANT** Open vs code from Developer Command Prompt, using command line : 
```bash
code . 
```

2. create a c++ hello world file

```c++
#include<iostream>
using namespace std;
int main(){
    cout << "hello world" << endl;
    return 0;
}
```

3. run the file, and let the vscode create the configure file for you.

**Don't** create the folder .vscode and configure files as the [official tutorial](https://code.visualstudio.com/docs/cpp/config-msvc#_add-hello-world-source-code) suggested. 

Simply just Press the play button in the top right corner of the editor.
Choose C/C++: cl.exe build and debug active file from the list of detected compilers on your system.
![run-play-button.png](https://timbrist.github.io/workflow/run-play-button.png.png)


