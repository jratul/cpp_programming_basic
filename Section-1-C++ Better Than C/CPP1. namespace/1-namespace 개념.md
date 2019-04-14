# namespace

## namespace의 필요성

namespace1.cpp  
```
#include <stdio.h>

void init() {
  printf("Audio init\n");
}

void init() {
  printf("Video init\n");
}

void init() {
  printf("System init\n");
}

int main() {
  init();
}
```

#### 1. 하나의 프로그램은 '수 십 ~ 수 백 개의 파일'로 구성될 수 있고, '수 십 명의 개발자가' 같이 작업할 수도 있다.
#### 2. 함수 및 구조체 등의 '이름 충돌' 이 발생할 수 있다.

namespace1.cpp(2)
```
#include <stdio.h>

namespace Audio {
  void init() {
    printf("Audio init\n");
  }
}

namespace Video {
  void init() {
    printf("Video init\n");
  }
}

//global namespace
void init() {
  printf("System init\n");
}

int main() {
  init();
  Audio::init();
  Video::init();
}
```
* * *

## namespace의 장점
### 1. 프로그램의 '다양한 요소(함수, 구조체 등)을 연관된 요소끼리 묶어서 관리'할 수 있다.
### 2. 기능별로 다른 이름 공간을 사용함으로써 '함수/구조체 등의 이름 충돌을 막을 수' 있다.


* * * 
## namespace에 있는 요소에 접근하는 3가지 방법

namespace2.cpp
```
#include <stdio.h>

namespace Audio {
  void init() { printf("Audio init\n"); }
  void reset() { printf("Audio reset\n"); }
}

void init() { printf("global init\n"); }

int main() {
  Audio::init();
  
  using Audio::init;  //using 선언
  init();
  reset();  //error
  
  using namespace Audio;  //using directive
  init();
  reset();
  
  ::init();   //global 
}
```

### 1. '한정된 이름'을 사용한 접근
Audio::init();

### 2. 'using 선언(declaration)'을 사용한 접근
using Audio::init;  
init 함수는 Audio 이름 없이 사용 가능

### 3. 'using 지시어(directive)'를 사용한 접근
using namespace Audio;
Audio namespace의 모든 요소를 Audio 이름 없이 사용 가능

-> using 선언이나 지시어는 '함수 안 또는 밖'에 만들 수 있다.  
-> global namespace에 접근하려면 "::"를 사용한다.  

* * *
## 함수를 선언 파일과 구현 파일로 분리할 때
### 1. 함수의 '선언부와 구현부를 모두 namespace로 묶어야' 한다.
### 2. 함수 구현부를 만드는 '2가지 방법'



Audio.h
```
namespace Audio {
  void init();
  void reset();
}
```

Audio.cpp
```
#include "Audio.h"

namespace Audio {
  void init() {
  }

  void reset() {
  }
}

/*
void Audio::init() {
}

void Audio::reset() {
}
*/
```

namespace3.cpp
```
#include "Audio.h"

int main() {
  Audio::init();
}
```
