**~={red}3페이지 시험=~**
- ptr 저장 영역
	- `&ptr`의 저장위치: stack영역
	- `*ptr`이 가리키는 실제 데이터: heap 영역
- malloc의 `(int *) malloc(2*sizeof(int))`-int로 형변환 하는 이유: malloc은 메모리 할당 후 시작 주소를 반환하는데, 이 주소의 타입이 void이다. 따라서 **4byte단위로 끊어서 읽고 쓰라고 알려주기 위해서 형변환을 사용한다**
- 코드는 **Code**, 초기화된 전역 변수와 정적 변수는 **Data**, 초기화 되지 않은 전역 변수와 정적 변수**BSS**, 동적 할당은 **Heap**, **~={red}지역 변수와 매개 변수=~**는 **Stack**에 저장
- buffer overflow 원리
	- 리턴 주소에 악성 코드의 시작 주소를 덮어 씌워서 악성 코드 주소로 이동하는 것이 원리임
	- 현재 함수의 지역 변수부터 이전 함수의 리턴 주소 전까지만 dump데이터로 덮어쓰우면 된다
	- 악성코드의 정확한 시작 위치를 알 수 없기 때문에 공격의 성공률을 올리기 위해서 악성코드 이전의 아무 NOP을 가리키게 한다
- 빅 앤디안과 리틀 앤디안
	- 빅 앤디안: **가장 중요한 자릿수(MSB, Most Significant Byte)**가 **가장 낮은 메모리 주소(배열의 앞쪽)**에 저장
	- 리틀 앤디안: **가장 덜 중요한 자릿수(LSB, Least Significant Byte)**가 **가장 낮은 메모리 주소(배열의 앞쪽)**에 저장
		- 0xffffd3b8
		- **배열에 넣을 때 (리틀 앤디안 적용):**이 값은 4바이트 정수이므로, 리틀 앤디안 규칙에 따라 **거꾸로** 쪼개서 넣어야 합니다.
		- `B8` (LSB) -> `D3` -> `FF` -> `FF` (MSB)



### 메모리 스택
1. BSS: 초기화 되지 않은 전역 변수와 정적 변수(static)
2. Data: 초기화 된 전역 변수와 정적 변수(Static)
3. Heap: 동적 메모리 할당(**포인터**)
4. Stack: 지역 변수 및 동적 메모리 포인터가 가리키고 있는 값

### 함수 인자 스택 순서
- C언어는 함수를 호출 할 때 오른쪽에서 왼쪽 순서로 스택에 넣는다.
- 따라서 func(5,10)은 10을 먼저 넣고 5를 넣는다
- 아래 2개 `add,mov`는 eax = edx - eax한 값을 ebp+8인 x에 넣는 것이다
### String
- string끝에는 null이 항상 붙어있다

### Buffer Overflow Attack
- 지역 변수부터 악성 코드까지 전체를 덮어씌우는 공격
- 할일
	- offset찾기: 지역변수에서 Return address까지의 거리
	- shell 코드 장소 찾기: shell코드를 어디에 둘지 정하기


## Badfile
- buffer 시작 지점부터 shellcode까지 0x90(NOP)로 덮어씌운다
- offset 위치에 새로운 return address를 적는다. 
- 300-len(shellcode)부터 shell code를 넣는다
- return 주소를 ebp + 112로 한 이유
	1. 쉘코드가 있는 NOP 영역으로 정확히 점프
	2. ebp+nnn에 0을 포함하면 안돼기 때문에
	   ex) 0xbfffeaf8 + 0x08 = 0xbfffeb00 -> 이러면 안된다

## 실행
- setuid프로그램으로 만들어서 실행
- execve의 `argv[1]`에 0이 들어가는 이유: 인자가 더 없기 때문에

## 32bit 레지스터
1. eax는 32비트
2. ax는 16비트
3. ax = ah + al
4. ah,al은 8비트(1바이트)

## 바이트 정렬
- 빅 앤디안: MSB를 가장 낮은 메모리에 
- 리틀 앤디안: LSB를 가장 낮은 메모리에

## 대응책
### 개발자
1. 데이터 길이 검사하는 함수 사용
	1. 데이터 복사 전 길이를 확인하는 더 안전한 동적 라이브러리 사용
		1. libsafe: 안전한 버전의 라이브러리/경계 검사
		2. libmib: 무제한 문자열 지원하여 오버플로우 해결하고자 하는 라이브러리
	2. 더 안전한 언어 사용

### OS
1. ASLR(주소 공간 배치 난수화)-무차별 대입 사용
	1. 시작 할 때마다 스택,힙 등 시작 주소를 랜덤하게 메모리에 배치하여 스택과 ebp를 찾기 어렵게 만든다
	2. 32비트 리눅스는 스택 주소가 가질 수 있는 경우의 수는 19비트 정도로 작다. 따라서 무한 루프로 실행을 하다보면 우연히 일치할 가능성이 있다 -> **~={red}32비트 환경에서는 ASLR만으로는 무차별 대입 공격을 완벽하게 방어하지 못한다=~**
2. 강화된 셸-setuid(0)사용
	1. dash 셸은 RUID와 EUID가 다르다면 보안을 위해 EUID를 RUID로 강제로 낮춘다. 그래서 setuid프로그램으로 실행할 지라도 RUID가 0이 아니면 root shell을 얻을 수 없고, 일반 shell만 얻을 수 있다
	2. 따라서 /bin/sh를 실행하기 전에 **setuid(0)**을 호출하면 RUID가 0이 되어서 root shell을 얻을 수 있다. 
	3. 어셈블리 코드를 shell code 앞에 추가
			1. eax를 xor로 초기화
			2. ebx를 xor로 초기화(setuid인자)
			3. al에 0xd5(setuid번호) 넣기
			4. 시스템 콜 실행
```asm
xorl %eax, %eax
xorl %ebx, %ebx
movb $0xd5, %al
int $0x80
```
3. 실행 불가능 스택
	1. 스택 영역에서 코드 실행을 원천적으로 차단
	2. NX비트를 사용해서 메모리 영역을 데이터와 코드로 구분
	3. 주입 공격은 막을 수 있지만, Return to libc공격에는 무력하다

### 컴파일러
- 스택 가드
	- 함수의 로컬 변수에 할당되어서 스택에 저장됨/코드로 수동 추가 가능
		- 이 랜덤 값은 스택이 아닌 메모리에 저장되어 있음
	- 함수 종료 직전 카나리 값이 달라졌다면 스택 건드린 것으로 간주하고 프로그램 종료
	- stack smashing dected: xxx 결과 나옴
```c
int guard;
guard = secret;
if(guard == secret) return;
```
```asm
movl %gs:20, %eas
movl %eax, -12(%ebp)
xorl %eax, %eax ;set canary
movl -12(%ebp), %eax
xorl %gs:20, %eax
je .L2
call __stack.chk_fail ; check canary
```