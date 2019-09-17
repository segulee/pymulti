# pymulti
python multiprocessing module note


- 미립자팁
1. python multiprocessing에서는 PIPE 통신시에 PIPE object를 가진 Main process, 혹은 PIPE object를 관리하는 프로세스(이하 PIPE 관리 프로세스)가 죽을 경우, PIPE가 삭제된다. 하지만 Ubuntu 서버에서는 어떤 이유 때문인지 몰라도 PIPE관리 프로세스가 죽더라도 PIPE 통신이 정상적으로 마무리되어 서브프로세스가 정상적으로 PIPE를 통해 데이터를 전달한 것으로 보여진다. 하지만 Windows에서는 PIPE관리 프로세스가 죽을 경우 곧 바로 PIPE object 들의 메모리가 해제되어 정상적인 종료가 아닌 [Errno 232] The Pipe is being closed. error를 뱉게 된다. 이를 유의하여 join을 호출하는 것이 가장 효과적인 방법이지만, PIPE 관리 프로세스를 살려두는 것도 정상적으로 동작하게끔 하기는 한다.

2. Windows에서는 multiprocessing을 사용하는 프로그램이 고정되어(frozen) 실행파일을 실행하려고 할 때에 multiprocessing.freeze_support()를 main 혹은 실행부 이전에 넣어주어야 한다. 넣지 않을 시 Runtime Erorr가 발생함. 이는 다른 platform에서는 어떠한 문제를 발생시키지 않으며, 윈도우 실행 파일을 생성할 때를 위한 지원을 추가한다.
