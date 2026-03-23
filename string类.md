```CPP
#include <iostream>
#include<string>
using std::string
int main(){
string str;//默认构造函数，为空字符串

string str1="hello world";//字面值初始化

string str2(str1);//拷贝构造

string str3=str1;//拷贝构造

string str4=(str1,0,4);//部分初始化,为hell

string str5=(2,'c');//
return 0;
}
```

