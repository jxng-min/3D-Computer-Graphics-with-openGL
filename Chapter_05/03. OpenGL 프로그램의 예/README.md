## OpenGL 프로그램의 예

</br>
</br>

### 5.3.1 간단한 OpenGL 프로그램

</br>

간단하게 검은 바탕에 흰 사각형을 그리는 GL 프로그램의 코드를 살펴보자. 다음과 같다.

</br>

```C
#include <GL/glut.h>											// 1
#include <GL/GL.h>											// 2
#include <GL/GLU.h>											// 3

void MyDisplay()											// 4
{
	glClear(GL_COLOR_BUFFER_BIT);									// 5
	glBegin(GL_POLYGON);										// 6

	glVertex3f(-0.5, -0.5, 0.0);
	glVertex3f(0.5, -0.5, 0.0);
	glVertex3f(0.5, 0.5, 0.0);
	glVertex3f(-0.5, 0.5, 0.0);

	glEnd();
	glFlush();											// 7
}

int main(int argc, char* argv[])
{
	glutInit(&argc, argv);
	glutCreateWindow("OpenGL Drawing Example");							// 8
	glutDisplayFunc(MyDisplay);									// 9
	glutMainLoop();											// 10

	return 0;
}
```
</br>

![화면 캡처 2024-11-03 130648](https://github.com/user-attachments/assets/4765fea3-51b6-4b38-ae0e-2a26feee33f9)

</br>

위의 코드는 매우 간단해 보이지만 GL 프로그램의 필수 요소를 모두 갖추고 있다.

먼저 #1, #2, #3의 헤더 파일은 각각 GL, GLU, GLUT 라이브러리 함수를 사용하기 위해 포함시킨 것이다.

</br>

main()은 모두 GLUT 함수로 구성되어 있다. #8의 **glutCreateWindow()**는 GLUT에게 **새로운 윈도우 생성을 요청**하는 명령이다.

이 함수의 매개변수로 작성한 "OpenGL Drawing Example"이 윈도우 상단 타이블 바에 나타나는 것을 볼 수 있다.

#9의 **glutDisplayFunc()**는 **함수를 디스플레이 이벤트에 대한 콜백 함수로 등록 요청**하는 명령이다. 예제에서는 MyDisplay()를 등록했다.

main()의 마지막은 이벤트 루프를 돌려야 하기 때문에 **항상 #10의 glutMainLoop()으로 끝난다**.

</br>

디스플레이 콜백 함수인 MyDisplay()를 살펴보자. **흰 사각형은 glBegin()과 glEnd() 사이에 정의**되어 있다.

glBegin()의 매개변수인 **GL_POLYGON**은 그 **아래에 작성되는 정점들이 다각형(Polygon)을 이루고 있음**을 의미한다.

여기서 다각형은 glVertex3f()에 의해 입력된 4개의 정점들로 구성되며, 윈도우 우상단을 (1.0, 1.0, 0.0)으로 하는 기본 윈도우를 기준으로 정의한 값이다.

</br>

드라이버는 **OpenGL 명령어 하나하나를 받는 즉시 실행하지 않는다**. **GPU는 명령어마다 파이프라인 프로세서와 서로 교신해야 하는 시간적인 부담**이 있어서 그렇다.

따라서 드라이버는 일정한 분량의 명령어를 쌓아두었다가 한꺼번에 파이프라인 프로세서에 전달한다.

</br>

드라이버의 이러한 행위를 원하지 않을 때 사용하는 것이 #7의 **glFlush()**다.

이 함수는 드라이버에 더 이상 명령어를 쌓지 말고 **현재까지 쌓인 명령어 모두를 무조건 프로세서에 전달하도록 강제**하는 명령이다.

이는 렌더링을 위한 모든 함수가 정의되었을 때 호출하는 것으로,  현재 상태 변수를 기준으로 파이프라인 프로세스를 실행하라는 것이다.

</br>

glutDisplayFunc()는 화면 디스플레이 이벤트가 발생하면 MyDisplay라는 콜백 함수를 실행하도록 한다.

다시 말해 **glutDisplayFunc()**는 **화면 디스플레이 이벤트가 발생할 때 실제로 어떤 함수를 호출해야 하는지를 등록**하는 함수다.

이 부분에서는 키보드나 마우스 등 **입력 이벤트별로 콜백 함수를 등록**할 수 있다. 

실행 명령이 아니기 때문에 **콜백 함수는 어떤 순서로 등록되어 있어도 무방**하다.

**디스플레이 콜백 함수는 모든 GL 프로그램에 반드시 등록**되어 있어야 한다. 이는 **윈도우 생성, 윈도우 크기 재조절, 윈도우가 포그라운드에 올라올 때 호출**된다.

</br>

#10의 **glutMainLoop()**은 **이벤트별로 콜백 함수를 등록했으므로 이벤트 루프로 진입하라고 명령**하는 함수다.

따라서 **모든 GL 프로그램은 항상 glutMainLoop()으로 끝난다**.

</br>
</br>

### 5.3.2 입력 콜백과 GLUT

</br>

GL 프로그램은 **윈도우 기능 및 I/O 제어**에 있어서 **GLUT 라이브러리를 이용**한다. 이 경우 GLUT은 **GL 프로그램과 드라이버 사이의 인터페이스** 역할을 한다.

따라서 프로그래머는 드라이버에 직접 접근하는 대신 GLUT에게 필요한 작업을 할 것을 요청하면 된다.

</br>

![제목 없는 다이어그램 drawio](https://github.com/user-attachments/assets/d82f2991-4f20-4e73-97f9-b663bc249dae)

</br>

프로그래머가 필요한 콜백 함수를 등록하고, 해당 콜백 함수에 원하는 내용을 채워넣기만 하면, 호출은 GLUT이 처리한다.

이를 위해 GLUT은 **프로그래머가 등록한 함수를 콜백 테이블 형태로 저장**하며, 이 테이블에는 이벤트 타입별로 불러야 할 **콜백 함수의 이름이 저장**된다.

이후, GLUT은 **드라이버로부터 받은 이벤트 레코드를 참고해 이벤트 타입을 판단**하고 테이블을 검색하여 그에 맞는 **콜백 함수를 호출**한다.

</br>

![제목 없는 다이어그램 drawio (1)](https://github.com/user-attachments/assets/81ce2178-fd9d-4ca5-99fe-fea6c7b866fa)

</br>

결과적으로 GLUT은 프로그램 실행 시, 위의 그림과 같이 루프를 돌고 있는다.

만약 큐에 이벤트가 있으면 첫 번째 이벤트를 가져와서 해당 타입에 맞는 콜백 함수를 호출하고, 다시 루프로 들어가 다음 이벤트가 있는지 검사한다.

일반적으로 큐에 이벤트가 하나도 없으면, 운영체제는 현재 프로그램 외에 다른 일을 실행한다. 

만약 **유휴 콜백 함수**를 정의하면 다른 일을 하는 대신 유휴 콜백 함수가 호출된다. 이 함수는 **모든 이벤트가 없는 시간을 활용하여 필요한 계산을 하는 데 사용**된다.

</br>

#### 이벤트 타입과 콜백 함수

|이벤트 타입|콜백 함수 등록 명령|콜백 함수 프로토타입|
|----------|-----------------|-------------------|
|DISPLAY|glutDisplayFunc(MyDisplay)|void MyDisplay();|
|MOUSE|glutMouseFunc(MyMouse)|void MyMouse(int button, int state, int x, int y);|
|KEYBOARD|glutKeyboardFunc(MyKeyboard)|void MyKeyboard(char key, int x, int y);|
|RESHAPE|glutReshapeFunc(MyReshape)|void MyReshape(int width, int height);|
|IDLE|glutIdleFunc(MyIdle)|void MyIdle();|

</br>

위의 표에는 콜백 함수를 등록하기 위한 명령어와 실제로 그 콜백 함수를 정의하기 위해 사용하는 프로토타입이 정의되어 있다.

</br>

**디스플레이 이벤트**는 **렌더링을 위한 이벤트**로, GL의 **모든 렌더링 명령은 대부분 디스플레이 콜백 함수 내부에 정의**된다.

이 **이벤트를 트리거하는 것은 사용자 입력이 아닌 GLUT**이다. 

경우에 따라서는 한 화면에 여러 개의 윈도우를 띄울 수도 있으며, 이 경우 윈도우마다 별도의 디스플레이 이벤트가 등록되어 있어야 한다.

</br>

마우스 이벤트는 **마우스 버튼을 누르거나 떼었을 때 트리거**된다. glutMouseFunc()로 등록된 함수는 **마우스 버튼을 눌렀을 때 호출**된다.

**버튼을 누른 상태에서 마우스를 움직였을 때 호출**하고자 하는 함수가 있으면 **glutMotionFunc()**로 등록할 수 있다.

또, **버튼 상태와 무관하게 마우스를 움직일 때 호출**하고자 하는 함수가 있으면 **glutPassiveMouseFunc()**로 등록할 수 있다.

</br>

리셰입 이벤트는 사용자가 윈도우 크기를 변경할 때, 키보드 이벤트는 키가 눌려졌을 때, 유휴 이벤트는 큐가 비었을 때 각각 호출된다.

특히, 유휴 이벤트는 애니메이션에서 자주 사용된다. 일반적으로 애니메이션에서는 물체가 움직인 위치를 변수에 저장한다.

큐가 비어 있는 유휴 시간을 이용하여 이 변수의 값을 변경하고 glutPostRedisplay()를 호출하면 나중에 디스플레이 콜백이 실행될 때 바뀐 변수 위치로 이동한다.

</br>
</br>

5.3.3 프로그램 기본 구조

</br>

GLUT의 윈도우 기능과 콜백 기능을 감안하면 OpenGL 프로그램의 기본 구조는 다음과 같다.

</br>

```C
void MyDisplay() {...};
void MyKeyboard(char key, int x, int y) {...};
void MyMouse(int button, int state, int x, int y) {...};

int main(int argc, char* argv[])
{
    Initialize and Open Window;                             # 1
    Initialize OpenGL State;                                # 2
    Register Input Callback Functions;                      # 3
    {
        glutDisplayFunc(MyDisplay);
        glutKeyboardFunc(MyKeyboard);
        glutMouseFunc(MyMouse);
    }

    Enter to Event Processing Loop;                         # 4
}
```
</br>

#1에서는 원하는 **윈도우 타입을 설정하고 초기화**한다. 물론 윈도우 관련이므로 이 부분은 **GLUT 함수로 구성**된다.

GLUT 역시 **자체 상태 변수를 사용**하므로, 이 작업은 **GLUT의 윈도우 관련 상태 변수 값을 설정하는 작업**으로 간주할 수 있다.

</br>

#2에서는 배경화면의 색, 광원의 위치 등 GL의 상태 변수 중 전체 프로그램을 통해 그 값이 **변하지 않을 상태 변수 값을 설정**한다.

당연히 이 부분은 **GL 함수로 구성**되고, **설정할 상태 변수가 많을 경우 InitGL()과 같은 별도 함수를 정의**하기도 한다.

</br>

#3에서는 콜백 함수를 등록하며, #4에서 이벤트 처리 루프로 진입한다. 물론 둘 다 GLUT 함수로 구성된다.

콜백 함수의 선언을 보면 함수가 구체적으로 어떻게 실행되어야 하는지를 선언하고 있다.

**디스플레이 콜백 함수 내부에 대부분의 GL의 렌더링 명령들이 집중적으로 포함**된다.
