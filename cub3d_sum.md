## ✓ 과제 설명
- Mandatory part - cub 3d
  - external func
  : open, close, read, write, printf, malloc, free, perror, strerror, exit
  : math library
  : minilibx
  - libft ok
	- 창 변경, 최소화 등 관리가 원활
	- 벽의 동서남북이 다른 texture를 가져야한다.
	- 벽 대신 스프라이트(아이템)을 표시할 수 있어야 한다.
	- 바닥과 천장 색상을 2개로 설정할 수 있어야 한다
	- 프로그램에 두번 째 인수가 "--save"일 경우 렌더링된 이미지를 bmp 형식으로 저장해야한다.
	- 두번째 인수가 없을 경우 프로그램은 창에 이미지를 표시하고 다음 규칙 준수
    	- 키보드의 왼쪽과 오른쪽 화살표 키는 미로에서 좌우를 볼 수 있도록한다
    	- WASD 키를 사용하여 미로를 통해 시야를 이동할 수 있어야 한다
    	- ESC 창종료
    	- 창틀의 닫기 버튼 클릭시 프로그램이 깨끗이 종료될 것
    	- 지도에서 선언된 화면 크기가 디스플레이 해상도보다 크면 현재 디스플레이 해상도에 따라 윈도우 크기가 설정된다.
	- .cub인 맵 설명 파일이 첫 번째 인수가 된다
    	- 빈칸은 0, 벽면 1, 아이템 2, 플레이어의 사작위치와 산란 방향의 경우를 NSEW로
  	- 지도는 벽으로 둘러싸거나 닫혀있어야 한다. 그렇지 않으면 프로그램 오류를 반환
  	- 지도 내용 외에는 각 요소의 유형을 하나 이상의 빈 줄로 구분할 수 있다
  	- 지도를 제외한 요소별 정보 유형별 구분은 1개 이상의 공백으로 할 수 있다.
  	- 항상 지도는 마지막 콘텐츠, 다른 것들은 순서 상관 x
  	- 지도에서 공백은 유효한 부분으로 처리하기 나름
  	- 각 요소(지도 제외) 첫 번째 정보는 유형 식별자(하나 또는 두개의 문자로 구성)이며, 그 두에 각 객체에 대한 모든 특정 정보가 다음과 같이 엄힐한 순서로 표시된다.
  	- 파일에서 잘못된 구성이 발견되면 프로그램을 올바르게 종료하고 "Error\n"을 반환한 후 선택한 명시적 오류 메세지를 반환한다!


## mlx 예제정리
####1. 소프트웨어와 디스플레이 연결
 - ```c
	void *mlx_init()
   ```
    - 모든 것 이전에 필요한 함수.
    - 내 소프트웨어와 디스플레이를 연결해준다.
    - 연결 실패시 NULL 리턴 혹은 (void *)0 리턴
	- 전반적인 프레임워크 지식없이 그래픽 소프트웨어를 쉽게 만들 수 있음
___
####2. window
  - ```c
  	void *mlx_new_indow(void *mlx_ptr, int size_x, int size_y, char *title)
	```
    - 새 창을 스크린에 띄운다.
    - size_x, size_y = 창 사이즈
    - title = 창의 타이틀 바에 표시된다.
    - mlx_ptr = mlx_init이 반환한 연결 식별자
    - 창 생성 실패시 NULL(void *0)
    - 성공시 새로운 윈도우 식별자인 void *를 준다.
  - ```c
  	int mlx_clear_window(void *mlx_ptr, void *win_ptr)
	```
	- 매개변수로 받은 win_ptr을 검은색으로 clear
  - ```c
  	int mlx_destroy_window(void *mlx_ptr, void *win_ptr)
	``` 
	- 매개변수로 받은 win_ptr로 윈도우를 destroy
___
####3. image
- ```c
	void *mlx_new_image(void *mlx_ptr, int width, int height)
  ```
	- 새 이미지를 메모리에 생성
	- 에러 발생시 NULL 리턴
	- 나중에 이 이미지를 조작할 때 필요한 이미지 식별자인 void *를 리턴한다.
	- 이미지 사이즈와 mlx_ptr연결 식별자만 있으면 된다.
- ```c
	char *mlx_get_data_addr(void *img_ptr, int *bits_per_pixel, int *size_line, int *endian)
	```
	- __반환값이 문자열인데 왜 배열로 형변환?__
		- 메모리크기, 바이트 등과 관련있는 듯. void * 로 캐스팅하면 밑에 참조할때 4를 곱해야한다는데 왜지?(keuhdall's github가면 더 자세한 정보)
	- 생성된 이미지에 대한 정보를 리턴해서 사용자가 나중에 이미지를 수정할 수 있도록하는 함수
	- img_ptr는 사용할 이미지
	- bits_per_pixel = 픽셀 색상 (이미지의 깊이)을 나타내는 데 필요한 비트 수
	- size_line = 이미지의 한 줄을 메모리에 저장하는 데 사용되는 바이트 수. 이 정보는 이미지에서 한 줄에서 다른 줄로 이동하는 데 필요
	- ___endian = 이미지의 픽셀 색상을 리틀 엔디안 또는 빅엔디안으로 저장해야하는 지 알려줍니다___ 
		- 리틀 엔디언(0) : 앞 주소에 작은 바이트붵 기록. 인텔계열의 디폴트
		- 빅 엔디언(1) : 앞 주소에 큰 바이트부터 기록. 사람 생각과 비슷
	- 이미지가 저장된 메모리 영역의 시작을 나타내는 char *주소를 리턴한다.
	- bits_per_pixel, size_line, endian 은 저장할 변수만 잘 넣어주면 알아서 정보를 잘 받아온다고 함.
	- height * width 만큼의 배열로 이미지를 표현하는 듯.
- ```c
	int mlx_put_image_to_window(void *mlx_ptr, void *win_ptr, void *img_ptr, int x, int y)
	```
	- 디스플레이 연결(mlx_ptr), 사용할 창(win_ptr) 및 이미지(img_ptr), 창에서 이미지를 배치할 위치(x,y)
- ```c
	void *mlx_xpm_file_to_image (void *mlx_ptr, char *filename, int *width, int *height);
	```
	- 에러 발생시 null 리턴
	- mlx_A_to_image 함수들은 각각 xpm 배열데이터, xpm파일, png파일의 종류 별 이미지 변환.
	- width와 hegith에는 너비와 높이를 저장할 int 형 변수의 주소를 넣어둔다.
- ```c
	void *mlx_destroy_image(void *mlx_ptr, void *img_ptr)
	```
___
####4. loop & hook
- ```c
  int mlx_loop(void *mlx_ptr)
  ```
    - 이벤트를 받기위해 필요한 함수
	- 그래픽 시스템은 두방향 (1. screen dispaly, 2. 키보드 마우스 정보 얻어오기)
    - 리턴 X, 키보드나 마우스로부터 받은 이벤트를 기다리는 무한루프, 이벤트에 연결되는 사용자정의 함수를 호출
    - mlx_ptr 이 피라미터
	- __마지막에 이걸 쳐줘야 프로그램이 종료되지 않고 계속돌아감__
<br>
-	```c
	int mlx_hook(void *win_ptr, int x_event, int x_mask, int (*funct)(), void *param)
	```
	- funct_ptr는 이벤트 발생시 호출하고 싶은 함수를 가리키는 포인터
	- 키도 입력 등의 이벤트를 감지해서 작동
	- funct는 win_ptr에 의해 특정된 윈도우에만 적용
	- x_event에 정의해놓은 X11 이벤트(ex. KEY press = 2)를 넣음
	- __x_mask?__
	- 예시. funct에서 key_press에서 int로 입력받은 이벤트에 따라 함수 동작
<br>
- mlx_key_hook, mlx_mouse_hook, mlx_expose_hook 함수는 같은 방식으로 동작
<br>

- ```c
	int mlx_loop_hook(void *mlx_ptr, int (*funct_ptr)(), void *param)
	```
	- 아무 이벤트도 일어나지 않을 경우 인자로 받았던 함수가 호출된다.
	- 레이캐스팅의 출력값을 통해 화면 뿌리기 용으로 사용
	- 이벤트를 포착했을 경우, Minilibx는 아래와 같이 해당 함수를 고정 피라미터로 호출한다. (임의 이름)
		- expose_hook(void *param);
		- key_hook(int keycode, void *param);
		- mouse_hook(int button, int x, int y, void *param);
		- loop_hook(void *param);

## 프로그램 코드 정리
### MAP
__- 맵 정보 읽기__
: GNL을 통해 한줄씩 읽으면서 에러처리와 맵 정보 저장을 병행
: 문자열로 개행을 포함시켜 맵을 전부 저장한후 개행을 기준으로 2차원 배열에 할당하여 맵을 생성하였다
__- 색 읽기__
: 10진수로 들어오는 3개의 16진수 인자를 10진수로 변환하여 계산했다

### 🗺 MAP PASING
__- 이어지지 않는 외벽의 확인__
: 플레이어를 감싸고 있는 외벽이 이어져 있는 가를 확인
: 처음에는 시계방향으로 외벽을 따라갈 생각을 했지만 직진해서 막히는 벽이 있으면 실패함, 그리하여 재귀함수를 사용하여 빈 공간을 사방으로 채워가는 형태로 확인하였다.
: char로 만들어진 공간을 지나가면서 정수형으로 바꾸기로함 (후에 맵핑을 위해)


### 🖼 BMP SAVE
__- 비트맵__
: 14 바이트는 비트맵 파일 헤더
: 40 바이트는 비트맵 정보 헤더

__- 이미지데이터__
: 이미지 데이터는 픽셀이라고하는 작은 이미지를 직사각형 형태로 모은것. 픽셀은 단섹의 직사각형.

__- RGB__
: 픽셀 데이터는 스칼라가 아닌 벡터
: 세로픽셀수 * 가로픽셀수 * 색채널(RGB)의 3차원 배열로 저장
: 여기서 색채널은 한 픽셀한에 들어가는 세개의 색 조합, 비트 연산을 통해서 RGB의 색으로 0,1,2 인덱스에 저장한다


### 🧱벽 레이캐스팅
___- x는 지도에서 row, y는 지도에서 column___
___- 시작 부분에 화면 width까지의 while문은 화면의 수직선 위치를 가리키는 x (x축)___

#### 🛠변수 설정
: 벽 레이캐스팅에 사용되는 각종 변수
: x값에 따라 초기화
- posx, posy 
: 플레이어의 초기 위치벡터로 지도에서 위치한 플레이어의 좌표값
- planex, planey
: 플레이어의 카메라 평면으로 플레이어의 방햑벡터와 수직을 이룬다
- camerax
: 카메라평면에서 차지하는 x좌표로 스크린에서 화면의 수직선 위치 (x축)
- raydirx, raydiry
: 광선의 방향벡터
- mapx, mapy
: 현재 플레이어의 위치의 소수점 버림으로 현재 광선의 위치, 광선의 한칸
- sidedistx, y
: 시작점 x, y~ 첫번째 x, y 면을 만나는 점까지의 광선거리
: deltadist는 1블럭당 고정수치이므로 플레이어의 위치부터 축까지의 길이에 비례곱을 통해 구한다
- deltadist x, y
: 첫번째 x,y ~ 바로 다음 x, y 면까지의 이동거리로 항상 일정함

#### 🧮 DDA 알고리즘
: x or y 방향으로 딱 한칸씩만 움직여서 그 칸이 벽인지 아닌지 확인하기
: x축에 부딪히는 면이 있는 지 보고, y 축에 부딪히는 면이 있는 지 확인
- stepx, stepy
: 어느방향으로 뛰어갈지 광선의 방향벡터를 통해 결정된다!
- hit
: 벽과의 부딪힘을 나타낸다
- side
: x면에 부딪혔다면 0, y면에 부딪혔다면 1
- perpwalldist
: 광선의 수직거리로 카메라평면에서 벽까지의 수직거리를 계산함으로써 벽이 굴곡되어 보이지 않게 계산(어안렌즈 해결)

#### 🎨 벽그리기
: 화면에서 벽을 그려낼 시작점과 끝점을 정한후 그린다
: x축에 따라 그려냄
- linehieght
: 그려낼 벽의 길이이다. 화면 전체 높이에서 수직거리를 나눠서 원근법을 표현
- drawstart, drawend
: 화면에서 벽을 그려낼 시작점과 끝점.
- wallx
: 정확히 부딪힌 벽의 위치를 나타낸다 (side에 따라 x좌표, y좌표)
- texX
: 텍스처의 x좌표
- step
: 텍서처의 좌표를
- zbuffer
: 각 수직선의 거리

### 스프라이트 레이캐스팅
1. 맵 전체의 스프라이트의 위치를 스프라이트 구조체 배열에 기록한다
2. 각 스프라이트에서 플레이어의 위치까지의 거리를 계산하고 거리순으로 정렬하여 거리가 먼 스프라이트부터 가까운 순으로 화면에 그려낸다

___- 스프라이트 벡터에 카메라 역행렬을 곱해주는 이유___
: 스프라이트만 고정되지 않기 위함. 스프라이트의 위치는 플레이어의 위치에 따라 상대적 위치. 스프라이트의 상대적 위치에 카메라 역행렬을 곱하면 플레이어의 회전에 영향을 받지 않고 고정된 것처럼 보이게 된다.

- vMove
: 스프라이트의 위치 (높이)를 결정할 수 있다.
- uDiv, vDiv
: vDiv 는 스프라이트의 길이(세로), uDive는 스프라이트의 폭(가로)를 조절한다.

- spritetransformy
: 스프라이트가 화면 안쪽으로 들어간 깊이(x는 화면 안쪽에 들어가 있는 스프라이틔 위치). x를 y로 나눠서 화면위의 좌표를 구하는 과정이라고 한다.

- invdet
: 역행렬 계산을 위해 존재하는 행렬식
- spritescreenx
: spriteScreenx 는 스프라이트가 그려지는 중간 지점.(가로 중간 값), 

## 코드 분할 내용 (수정중)
  ### game
  #### game.c
  - game_init(t_info *info);
  : mlx_init 과 창띄우기 까지로 스크린과 텍스쳐 배열, 이미지 로드, 스프라이트 준비 작업
    - __game_ready.c__
		- ready_screen
		: 스크린 배열 할당 및 초기화
		- ready_texture
		: 텍스처 배열 할당 및 초기화
		- ready_image
		: 이미지 구조체에 이미지 생성 및 데이터화(__추가 공부 필요__)
		- set_etc
		: 각종 인덱스 0 초기화
    - __ready_sprite.c__
      - check_sprite
		: 스프라이트 구초제 배열 할당 후 맵을 읽으며 스프라이트 입력

  #### texture_image.c
  - load_texutre(t_info *info);
	: xpm 파일을 이미지 포인터에 저장 후 get_data_addr을 통해 생성된 이미지의 정보를 리턴해서 수정 등에 용의하게 data 배열에 저장한다. 저장된 data 배열은 텍스처 배열에 저장된다.
	- load_image