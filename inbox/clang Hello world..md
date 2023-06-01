#clang


```c
#define <stdio.h>

int main()
{
  printf("hello, world\n");
  return(0);
}
```

```shell
$ cc -g -Wall hello.c -o hello
```

-g オプションはデバッグ情報の埋め込み指定　gdb で使う。

## Makefile

```makefile
PROGRAM = hello
OBJS = hello.o
SRCS = $(OBJS: %.o = %.c)
CFLAGS = -Wall -g
LDFLAGS =
LDLIBS =

$(PROGRAM): $(OBJS)
     $(CC) $(CFLAGS) $(LDFLAGS) -o $(PROGRAM) $(OBJS) $(LDLIBS)

clean:
     rm $(PROGRAM) $(OBJS)
```
### 規則

target ...: dependency
    command
    command
    
command の前の空欄はTABコードなので注意。

