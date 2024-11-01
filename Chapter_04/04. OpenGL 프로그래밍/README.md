## OpenGL 프로그래밍

</br>
</br>

### 4.4.1 명령어

</br>

프로그램 언어를 구성하는 것은 명령어다. OpenGL은 프로그램 언어가 아닌 API이므로 **OpenGL을 구성하는 것은 함수**다.

그렇지만 편의상 OpenGL 함수를 명령어라고도 하므로 앞으로 혼용하도록 하겠다.

</br>

![제목 없는 다이어그램 drawio](https://github.com/user-attachments/assets/167ff90e-aae8-42c6-9f41-aa0e9a6b11da)

</br>

앞쪽의 **gl은 해당 함수가 GL 라이브러리 함수**임을 나타낸다. 만약 이게 **glu이면 GLU 유틸리티 함수**고, **glut이면 GLUT 툴킷 함수**다.

3은 괄호 내부의 매개변수의 개수를 의미한다. 이 경우에는 x, y, z로 총 3개의 매개변수를 가지므로 3이다.

f는 각 매개변수가 부동 소수점 타입임을 나타낸다. 만약 이 값이 정수라면 i(Integer)로 표시한다.

</br>

|접미사|데이터 타입|C/C++ 타입명|OpenGL 타입명|
|------|--------|------------|------------|
|f|32bit floating point|float|GLfloat|
|d|64bit floating point|double|GLdouble|
|b|8bit integer|signed char|GLbyte|
|ub|8bit unsigned integer|unsigned char|GLubyte, GLboolean|
|i|32bit integer|int, long|GLint|
|ui|32bit unsigned integer|unsigned long|GLuint, GLenum, GLbitfield|
|s|16bit integer|short|GLshort|

</br>

OpenGL의 타입명은 C/C++ 타입명 앞에 **GL을 단순히 추가**한다. GL 타입은 GL 자체의 필요에 의해 **C/C++ 타입명을 재정의한 것에 불과**하다.

또, **OpenGL 자체가 C 함수로 구현**되어 있으므로, 어떤 변수를 사용하고자 할 때는 굳이 O**penGL 타입 말고 C/C++ 타입으로 선언하여 사용해도 무방**하다.

</br>

![제목 없는 다이어그램 drawio (1)](https://github.com/user-attachments/assets/b3bf9459-a419-4486-aba3-456fb837b969)

</br>

**v는 벡터**를 의미한다. 여기서 **벡터는 배열**을 말하는데, 매개변수 리스트를 각각 별도로 작성하는 것이 번거로울 경우 **배열 자체를 전달**할 수 있다.

물론 매개변수로 전달되는 p는 배열명으로, **배열을 가리키는 포인터**에 해당한다.

</br>

**OpenGL은 객체지향적이지 않다**. GL이 객체지향적이라면 객체지향의 특성 중 하나인 다형성을 지원해야 한다.

즉, 동일한 이름의 glVertex() 하나로 3D, 2D 정점 모두를 정의할 수 있어야 한다.

</br>

OpenGL은 **절차지향 언어인 C로 작성**되었기 때문에 필요에 따라 **함수명을 달리** 해주어야 한다.

3D 정점이면 glVertex3f(), 2D 정점이면 glVertex2f()라는 별도의 함수로 나타내어야 한다.

C++의 함수 오버로딩을 사용하여 새로운 함수를 정의해서 해결할 수 있겠지만 프로그램의 실행 속도가 느려진다.

OpenGL이 객체지향을 채택하지 않은 이유는 순전히 **처리 속도를 높이기 위함**이다.

</br>
</br>

### 4.4.2 프로그램 구성 요소

</br>

GL은 함수만으로는 GL 프로그램을 작성할 수 없다. GL은 렌더링 기능만 수행할 뿐, 사용자 입력을 받아들이는 기능은 없다.

또 렌더링 결과를 출력하기 위해 화면에 윈도우를 생성하는 것도 GL이 할 수 없다. 이러한 기능은 유틸리티에 의해 이루어진다.

</br>

#### GL의 구성 요소

1. GL(OpenGL Core Library)
    + **렌더링 기능을 제공**하는 함수 라이브러리다.

2. GLU(OpenGL Utility Library)
    + GLU는 GL 라이브러리의 도우미 역할을 한다.
    + 다각형 분할, 투상, 2차원 곡면 등 **고급 기능을 제공**한다.

3. GLUT(OpenGL Utility Toolkit)
    + **사용자 입력**을 받거나 화면 **윈도우를 제어**하기 위한 함수다.
    + 윈도우 운영체제가 실행하는 기능들이다.

</br>

![img](https://github.com/user-attachments/assets/2dd24291-e7f3-4cb3-bcfa-970f7dbc6e2a)

</br>

프로그래머는 C/C++ 프로그램을 통해 각 구성 요소를 호출한다.

GLU 라이브러리는 GL 라이브러리 위에 구축된 것으로 **GLU 함수 호출은 결국 GL 함수 호출로 바뀐다**.

**GLUT** 라이브러리는 I/O 및 윈도우 제어를 하기 위한 것으로 **운영체제를 호출**한다. 또한 **일부는 GLU 또는 GL 함수를 호출**한다.

물론 **모든 명령어는 최종적으로 디바이스 드라이버에 의해 어셈블리 명령어로** 바뀐다.

</br>
</br>

### 4.4.3 GLUT와 Windows

</br>

GLUT는 GL의 일부가 아니라 **GL과는 별도로 작성된 프리웨어**다.

GL과 마찬가지로 **GLUT API도 자체 상태 변수 및 상태 변수 테이블을 사용**한다.

GLUT 명령어에 의해 **자체 기본 상태 변수 값을 변경하거나 검색**할 수 있다.

</br>

프로그램 실행 시 프로그램을 제어하는 것은 운영체제다.

GL 프로그램 실해의 시작 명령을 내리는 것도 운영체제고, GL의 요청에 따라 새로운 윈도우를 만드는 것도 운영체제다.

GL의 렌더링 작업이 끝났을 때 윈도우 안에 그 결과를 보여주는 함수를 호출하는 것도 운영체제다.

또, 사용자가 마우스를 클릭하였을 때 상호작용에 필요한 함수를 호출하는 것도 운영체제다.

하지만 **일반적으로 운영체제는 사용 환경에 따라 다르다**.

</br>

**GL 프로그램과 Windows 사이의 인터페이스** 역할을 하는 것이 **GLUT**이다. GLUT은 두 가지 역할을 한다.

1. 동일한 GLUT 함수라도 명령어의 번역이 운영체제마다 다르다.
    + 프로그래머는 프로그램 환경을 신경 쓸 필요가 없다.

2. GLUT 함수는 상대적으로 간단한 추상적 명령으로 구성되어 있어 프로그래밍이 쉽다.
    + 명령들의 구현부에 대해서는 전혀 신경 쓸 필요가 없다.

</br>

그러나 **GLUT로는 매우 제한된 GUI 인터페이스만 가능**하다.

만약 더 복잡한 GUI를 원하는 경우, **각 운영체제에서 제공하는 라이브러리를 추가적으로 사용**할 수 있다.

이 라이브러리들은 GLUT와 해당 운영체제를 연결하는 가교 역할로, 필요 시 직접 호출하여 사용할 수 있다.

하지만 그렇게 되면 **호환성 측면에서 해당 운영체제에서만 작동하게 되는 단점**이 있다.

</br>

운영체제 기능 중 GL 프로그램 실행에 필요한 것은 윈도우 기능과 콜백 기능이다.

**윈도우** 기능은 **프로그램의 실행에 필요한 창을 관리하는 기능**을 말한다.

**콜백** 기능은 **GL 프로그램 실행 중에 발생하는 사용자 입력을 처리하는 기능**을 말한다.

GLUT은 이러한 기능에 대한 인터페이스를 제공한다.

</br>

|분류|함수명|설명|
|----|-----|----|
|윈도우 초기화|gluInit()|윈도우 운영체제와 세션 연결|
|윈도우 초기화|gluInitWindowPosition()|윈도우 위치 설정|
|윈도우 초기화|gluInitWindowSize()|윈도우 크기 설정|
|윈도우 초기화|gluInitDisplayMode()|디스플레이 모드 설정|
|윈도우 관리|glutSetWindowTitle()|윈도우 타이틀 설정|
|윈도우 관리|glutCreateWindow()|새로운 윈도우 생성|
|윈도우 관리|glutReshapeWindow()|크기 변경에 따른 윈도우 조정|
|윈도우 관리|glutPostRedisplay()|현 윈도우가 재생되어야 함을 표시|
|윈도우 관리|glutSwapBuffers()|현 프레임 버퍼 변경|

</br>

1. gluInit(int* argcp, char** argv)
    + GLUT 라이브러리를 초기화한 후 윈도우 시스템과 세션을 연다.
    + 라인 명령어가 아니면 일반적으로 argcp, argv는 사용하지 않는다.

2. gluInitWindowPosition(int x, int y)
    + **윈도우 좌상단을 화면 좌표 (x, y)에 위치**시킨다. 화면 좌표는 화소 단위로 측정되어 **정수 타입**이다.
    + GLUT의 화면 좌표계는 **좌상단을 원점**으로 하고, 우측을 +x축으로, 하단을 +y축으로 정의한다.

3. gluInitDisplayMode(unsigned int mode)
    + **생성될 윈도우의 디스플레이 모드를 설정**한다.
    + 서로 상충되지 않는 **2가지 이상의 모드를 한번에 명시하려면 비트 OR 연산자를 사용**할 수 있다.

    |모드|종류|설명|
    |----|----|---|
    |컬러 모드|GLUT_RGB|컬러 모드를 RGB 모드로 설정한다.|
    |컬러 모드|GLUT_INDEX|컬러 모드를 인덱스 모드로 설정한다.|
    |프레임 모드|GLUT_SINGLE|프레임 모드를 단일 버퍼로 설정한다.|
    |프레임 모드|GLUT_DOUBLE|프레임 모드를 이중 버퍼로 설정한다.|

4. glutReshapeWindow(int width, int height)
    + 프로그램 실행 중 사용자가 **윈도우 크기를 재조정했을 때 호출**된다.
    + Windows는 사용자가 변경한 윈도우의 폭과 높이를 이 변수를 통해 GLUT에 전달한다.

5. glutPostRedisplay()
    + 현재의 윈도우를 재생하도록 요구하는 함수다.

6. glutSwapBuffers()
    + **백 버퍼와 프론트 버퍼를 스위칭**한다.
    + 프론트 버퍼의 내용이 뿌려지는 동안 백 버퍼는 새로운 내용을 준비한다.
    + 프론트 버퍼의 내용이 다 뿌려지면 백 버퍼가 프론트 버퍼가 된다.

</br>

이처럼 **컬러 모드 설정, 버퍼 활성화, 윈도우 초기화 및 관리** 등 모두 GLUT의 기능이다.

이 박에도 팝업 메뉴를 그려내거나, 비트맵 또는 스트로크 폰트를 그려내거나, 일부 3D 물체를 그려내는 것도 GLUT이다.

</br>

그러나 GLUT의 가장 중요한 기능 중 하나가 **콜백 함수**다.

윈도우 관리와 마찬가지로 콜백 기능 역시 윈도우 운영체제의 본질적인 기능에 속한다.

</br>
</br>

### 4.4.4 GLUT 상태 변수

</br>

GLUT가 Windows 운영체제와 인터페이스 역할을 수행하기 위해서는 자체의 상태 변수가 필요하다.

GLUT 상태 변수는 **프로그램으로 제어할 수 있는 상태 변수**와 **시스템별로 고정된 상태 변수**로 나뉜다.

</br>

#### 프로그램 제어 상태 변수

|상태 변수|타입|설정 함수|초기값|
|--------|----|---------|-----|
|GLUT_INIT_WINDOW_X|Integer|gluInitWindowPosition|-1|
|GLUT_INIT_WINDOW_Y|Integer|gluInitWindowPosition|-1|
|GLUT_INIT_WINDOW_WIDTH|Integer|gluInitWindowSize|300|
|GLUT_INIT_WINDOW_HEIGHT|Integer|gluInitWindowSize|300|
|GLUT_INIT_DISPLAY_MODE|Bitmask|gluInitDisplayMode|GLUT_RGB \| GLUT_SINGLE \| GLUT_DEPTH|

</br>

#### 시스템 고정 상태 변수

|상태 변수명|타입|의미|
|----------|----|---|
|GLUT_SCREEN_WIDTH|Integer|모니터 화면 폭|
|GLUT_SCREEN_HEIGHT|Integer|모니터 화면 높이|
|GLUT_HAS_KEYBOARD|Boolean|키보드 유무|
|GLUT_HAS_MOUSE|Boolean|마우스 유무|
|GLUT_NUM_MOUSE_BUTTONS|Integer|마우스 버튼 수|
