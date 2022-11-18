**使用函数输出指定范围内的Fibonacci数**

```c
#include <stdio.h>

int fib( int n );
void PrintFN( int m, int n );
    
int main()
{
    int m, n, t;

    scanf("%d %d %d", &m, &n, &t);
    printf("fib(%d) = %d\n", t, fib(t));
    PrintFN(m, n);

    return 0;
}

/* 你的代码将被嵌在这里 */
int fib( int n );
{
    int f1 = 1;
    int f2 = 1;
    int temp ;
    if(n == 1)
        return f1;
    else if(n == 2)
        return f2;
    else{
        for(int i = 3; i <= n; i++){
            temp = f1 + f2
            f1 = f2
            f2 = temp;
        }
        return temp;
    }
}
void PrintFN( int m, int n ){
    
}

```