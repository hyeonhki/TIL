### mlx 예제 정리
 - ```c
	void *mlx_init()
   ```
    - 모든 것 이전에 필요한 함수.
    - 내 소프트웨어와 디스플레이를 연결해준다.
    - 연결 실패시 NULL 리턴 혹은 (void *)0 리턴

  - ```c
  	void *mlx_new_indow(void *mlx_ptr, int size_x, int size_y, char *title)
	```
    - 새 창을 스크린에 띄운다.
    - size_x, size_y = 창 사이즈
    - title = 창의 타이틀 바에 표시된다.
    - mlx_ptr = mlx_init이 반환한 연결 식별자
    - 창 생성 실패시 NULL(void *0)

- ```c
  int mlx_loop(void *mlx_ptr)
  ```
    - 이벤트를 받기위해 필요한 함수
    - 리턴 X, 키보드나 마우스로부터 받은 이벤트를 기다리는 무한루프, 이벤트에 연결되는 사용자정의 함수를 호출
    - mlx_ptr 이 피라미터
	- __마지막에 이걸 쳐줘야 프로그램이 종료되지 않고 계속돌아감__

  - ```c
  	int mlx_hook(void *win_ptr, int x_event, int x_mask, int (*funct)(), void *param)
	```
	- funct_ptr는 이벤트 발생시 호출하고 싶은 함수를 가리키는 포인터
	- funct는 win_ptr에 의해 특정된 윈도우에만 적용
	- __x_event와 x_maks는 뭘까?__
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
	int mlx_loop_hook(void *mlx_ptr, int (*funct_ptr)(), void *param)
	```
	- 아무 이벤트도 일어나지 않을 경우 인자로 받았던 함수가 호출된다.
	- 이벤트를 포착했을 경우, Minilibx는 아래와 같이 해당 함수를 고정 피라미터로 호출한다. (임의 이름)
		- expose_hook(void *param);
		- key_hook(int keycode, void *param);
		- mouse_hook(int button, int x, int y, void *param);
		- loop_hook(void *param);
