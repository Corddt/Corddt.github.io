#  编程学习

2024年5月26日，新设专栏。
目的是让自己之前学习的编程知识进行系统性梳理，并且在这基础上进行知识结构优化。

循序渐进，每日记录。

## 2024.5.26
今天从第一章开始整理。
```markdown
编程是一种控制计算机的方式，和我们平时双击打开文件、关机、重启没有任何区别。
                                        -闫学灿
```

```
#include <iostream> //cin,cout
#include <cstdio> //pringf,scanf
using namespace std; //使用std这个命名空间 
int main()
{
	//逻辑书写区 
	cout << "Hello World" << endl;
	
	//逻辑书写结束 
	return 0;//程序正常结束一定要返回0 
} 
```
找错误就找error

bool false/true 1byte
char 'c','a',' ','\n'  1byte
c++里一个字符一定要用单引号表示
int -2^31~+2^31 => -2147483648~2147483647  4 byte
float 1.23,2.5,也可以表示科学计数法,1.235e2 4 byte
精度低，float也成为单精度浮点数，有效数字6-7位
double float 有效数字15-16位 8 byte
long long (int) -2^63~+2^63,int扩展版 8byte
long double 有效数字18-19位 8/12/16byte

B=>Byte
b=>bit
1Byte = 8bit
8Mb = 1MB

今天听的1.1 变量、输入输出、表达式和顺序语句，听到51：58
