#include <numeric>
  int std::gcd(int a, int b);

  - 参数：a, b，两个整数（模板函数，支持 int、long long 等整数类型）。
  - 返回值：类型和参数相同的整数，即最大公约数。返回值恒为非负。

gcd函数的实现方法有：
1.枚举
从min(a,b)往下一个个试，能同时整除两个数的最大那个就是答案：
int gcd(int a,int b){
for(int i=min(a,b);i>=1'i--){
if(a%i==0&&b%i==0)
return i;

return 1;
}

2.高效算法：欧几里得（辗转相除）
核心定理：
gcd(a,b)=gcd(b,a%b)
int gcd(int a, int b) {
      if (b == 0) return a;
      return gcd(b, a % b);
  }