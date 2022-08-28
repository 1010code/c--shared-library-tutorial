## 建立函式庫
建立標頭檔，定義所需的函式。
```c
# mylib.h
#include<string>
#include<vector>
#include<iostream>

using namespace std;

void say(string message);
void say(vector<string> vec);
```

實作函式庫內容。
```c
# mylib.cpp
#include<string>
#include<vector>
#include<iostream>

using namespace std;

void say(string message){
  cout << message << endl;
}
void say(vector<string> vec){
  for(int c=0; c<vec.size(); c++){
    cout << vec[c] << endl;
  }
}
```

產生 Dynamic Link Library `libmylib.dylib`(也可以.so)。

```sh
g++ --verbose -dynamiclib -o libmylib.dylib mylib.cpp
```

main 檔執行編譯時，`-L` 連結 shared libraries 資料夾路徑，編譯時會找所有的 `so, dll,dylib` 檔也。最後要透過 `-l` 連結確切的 `mylib` Shared Library。
```sh
g++ -o main ./main.cpp -L./ -lmylib -std=c++11
```

或是直接連結到 dylib(so) 也行。

```sh
g++ -o main ./main.cpp libmylib.dylib -std=c++11
```


### 不同資料夾
`-I`目的是要讓 `main.cpp` 知道要去哪裡抓標頭檔。`-L` 是連結動態函式庫。執行檔同個資料夾下必須要有`libmylib.dylib`才能正確地被執行。

```sh
g++ -o main ./main.cpp -I./test -L./test -lmylib -std=c++11
```

> -I{include dirs} to inform gcc where to look when you #include something.
> -L tells gcc about paths to look into for libraries, and -l{libname} links against a library.

另外也可以將編譯好的 object file `.o` 檔，轉換成 `.so`:

```sh
gcc hello.o -shared -o libhello.so
```

## 其他方法
### 直接在編譯時給予外部函式庫路徑

```sh
g++ -o main main.cpp ./mylib.cpp -std=c++11 
```

### 或是先編譯成 o 檔
compile those source files to object files, first:

```sh
g++ -c -o mylib.o ./mylib.cpp
```
> -c tells gcc not to try and link things together

```sh
g++ -o main mylib.o main.cpp -std=c++11
```


## 重點整理
Search Path and Library Linking Flags.
- -l[linalg]
  - => Links to shared library or shared object - Specifically, it links to linalg.dll on Windows, liblinalg.so (on Unix-like oses like Linux, BSD, AIX, …) or linalg.dylib on MacOSX.
- -L[/path/to/shared-libraries]
  - => Add search path to shared libraries, directory containing *.so, *.dll or *.dlyb files such as libLinearAlgebra.so depending on the current operating system.
- -I[/path/to/header-files]
  - Add search path to header files (.h) or (.hpp).

## Reference
- [C++ Creating Shared Library on macOS](https://www.youtube.com/watch?v=PRUR_bN3r-E).
- [[stackoverflow] Properly Link Libraries in the Command Line](https://stackoverflow.com/questions/33110118/properly-link-libraries-in-the-command-line)
- [CPP/C++ Compiler Flags and Options](https://caiorss.github.io/C-Cpp-Notes/compiler-flags-options.html)
- [什麼是SO檔案格式?](https://template.city/so/)