## OpenGL 설치와 설정

</br>
</br>

### 4.5.1 GLUT

</br>

GLUT은 OpenGL 프로그래밍을 돕기 위한 기능들을 모아놓은 라이브러리다.

일반적으로 많이 알려진 것은 FreeGLUT으로 대부분의 모든 운영체제에서 사용할 수 있다.

Visual Studio 2010부터 사용 가능한 NuGet을 이용하면, 어려운 빌드 과정 없이 GLUT을 적용할 수 있다.

</br>

그러나 FreeGLUT과 GLUT for Windows의 차이가 존재한다.

FreeGLUT에서는 GLUT을 사용할 때 반드시 glutInit()을 호출해서 GLUT을 초기화해야 한다.

</br>
</br>

### 4.5.2 GLUT 설치

</br>

프로젝트 - NuGet 패키지 관리를 누르면 아래의 NuGet 창이 뜨게 된다.

</br>

![이미지_478](https://github.com/user-attachments/assets/2cad955b-a358-4996-a667-4d8d4c6f7efe)

</br>

검색 입력란에 freeglut을 검색하면 freeglut이 나오게 된다.

</br>

![화면 캡처 2024-10-31 112414](https://github.com/user-attachments/assets/d8bbb749-51b0-4cc1-b43b-8f5a4c57205b)

</br>

그리고 이를 설치해주면 바로 glut을 사용할 수 있다.

</br>
</br>

### 4.5.3 OpenGL 예제

</br>

```C
#include <windows.h>
#include <gl/glut.h>

void MyDisplay()
{
	glClear(GL_COLOR_BUFFER_BIT);

	glBegin(GL_POLYGON);
	glVertex3f(-0.5, -0.5, 0.0);
	glVertex3f(0.5, -0.5, 0.0);
	glVertex3f(0.5, 0.5, 0.0);
	glVertex3f(-0.5, 0.5, 0.0);
	glEnd();
	glFlush();
}

int main(int argc, char* argv[])
{
	glutInit(&argc, argv);
	glutCreateWindow("OpenGL Drawing Example");
	glutDisplayFunc(MyDisplay);
	glutMainLoop();

	return 0;
}
```
</br>

![화면 캡처 2024-10-31 110527](https://github.com/user-attachments/assets/b2b83abc-4348-4278-8a41-2add47b0f3da)

</br>

위의 예제 코드를 실행하게 되면 콘솔 창과 프로그램 창이 동시에 뜨는 것을 확인할 수 있다.

이는 glut을 통해 윈도우 운영체제를 제어하도록 함으로써 다른 운영체제에서도 동일하게 실행되게 하기 위함이다.

</br>
</br>

### 4.5.4 콘솔 창 제거

</br>

윈도우 콘솔 앱으로 프로젝트를 생성하여 윈도우 전용으로 실행하게 되면 콘솔 창을 제거할 수 있다.

</br>

```C
#include <windows.h>
#include <gl/glut.h>

void MyDisplay()
{
	glClear(GL_COLOR_BUFFER_BIT);

	glBegin(GL_POLYGON);
	glVertex3f(-0.5, -0.5, 0.0);
	glVertex3f(0.5, -0.5, 0.0);
	glVertex3f(0.5, 0.5, 0.0);
	glVertex3f(-0.5, 0.5, 0.0);
	glEnd();
	glFlush();
}

int APIENTRY wWinMain(_In_ HINSTANCE hInstance,
                     _In_opt_ HINSTANCE hPrevInstance,
                     _In_ LPWSTR    lpCmdLine,
                     _In_ int       nCmdShow)
{
	int argc = 0;
	char* argv[] = { NULL };
	glutInit(&argc, argv);

	glutCreateWindow("OpenGL Drawing Example");
	glutDisplayFunc(MyDisplay);
	glutMainLoop();

	return 0;
}
```
</br>

![화면 캡처 2024-10-31 111552](https://github.com/user-attachments/assets/83890d6e-0a64-4ac2-91b5-b620503892f3)

</br>
