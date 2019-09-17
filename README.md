# pymulti
python multiprocessing module note


- 미립자팁
1. python multiprocessing에서는 PIPE 통신시에 PIPE object를 가진 Main process, 혹은 PIPE object를 관리하는 프로세스(이하 PIPE 관리 프로세스)가 죽을 경우, PIPE가 삭제된다. 하지만 Ubuntu 서버에서는 어떤 이유 때문인지 몰라도 PIPE관리 프로세스가 죽더라도 PIPE 통신이 정상적으로 마무리되어 서브프로세스가 정상적으로 PIPE를 통해 데이터를 전달한 것으로 보여진다. 하지만 Windows에서는 PIPE관리 프로세스가 죽을 경우 곧 바로 PIPE object 들의 메모리가 해제되어 정상적인 종료가 아닌 [Errno 232] The Pipe is being closed. error를 뱉게 된다. 이를 유의하여 join을 호출하는 것이 가장 효과적인 방법이지만, PIPE 관리 프로세스를 살려두는 것도 정상적으로 동작하게끔 하기는 한다.

2. Windows에서는 multiprocessing을 사용하는 프로그램이 고정되어(frozen) 실행파일을 실행하려고 할 때에 multiprocessing.freeze_support()를 main 혹은 실행부 이전에 넣어주어야 한다. 넣지 않을 시 Runtime Erorr가 발생함. 이는 다른 platform에서는 어떠한 문제를 발생시키지 않으며, 윈도우 실행 파일을 생성할 때를 위한 지원을 추가한다.




>번역

1. In Python multiprocessing, PIPE objects are deallocated when the Main process with the PIPE objects or the process which is managing the PIPE objects (hereinafter referred to as 'PIPE management process') dies during PIPE communication. On Ubuntu servers, however, for some reasons, even if PIPE management process dies, it seems that the PIPE communication is normally completed and the subprocess has successfully delivered data through PIPE. However, on Windows, PIPE objects are freed immediately after PIPE management process dies causing `[Errno 232] The Pipe is being closed` error. With that in mind, calling join is the most efficient way to do this, but keeping PIPE management process alive also works.

2. On Windows, when a program which uses multiprocessing library has been frozen to produce a Widnows executable, it is required to add a line `multipocessing.freeze_support()` right after main or before execution code using multiprocessing. If not, Runtime Error occurs. This line cause nothing on other platforms, and add support for when the program which uses multiprocessing.
