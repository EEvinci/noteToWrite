# PAT（Advanced Level）

## A+B Format

the first question

liuchuo is do powerful

![image-20230312222928844](C:\Users\PC\AppData\Roaming\Typora\typora-user-images\image-20230312222928844.png)

```c++
#include<iostream>

using namespace std;

int main(){
	int a, b;
	cin >> a >> b;
	
	int res;
	res = a + b;
	
	string str = to_string(res);
	int len = str.length();
	
	for(int i = 0; i < len; i++){
		cout << str[i];
		
		if(str[i] == '-')
			continue;
        
		// the most important
		if( (i+1)%3 == len%3 && (i+1) != len )
			cout << ',';
	}
	
	return 0;
}
```

### core

这里的3可以替换为任何数n

核心思想就是使用len%n作为从len%n开始的逗号输出位置，类似一个偏置项

因为是%，所以具有周期性，经历了n个数之后才会再输出

### attention

对于-999,111这种情况(假定n为3, 符号为-)
就是对负号(或其他符号)的条件判断
所以如果是符号, 那么就不要进行索引的判断, 因为这时候len==7, len%3==1, 在输出符号之后如果还检查索引
就会输出-,999,111

