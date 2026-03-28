**灵神代码**：

class Solution {

public:

&#x20;   int maxScore(vector<int>\& cardPoints, int k) {

&#x20;       int s = reduce(cardPoints.begin(), cardPoints.begin() + k);

&#x20;       int ans = s;

&#x20;       for (int i = 1; i <= k; i++) {

&#x20;           s += cardPoints\[cardPoints.size() - i] - cardPoints\[k - i];

&#x20;           ans = max(ans, s);

&#x20;       }

&#x20;       return ans;

&#x20;   }

};



**作者：灵茶山艾府**

链接：https://leetcode.cn/problems/maximum-points-you-can-obtain-from-cards/solutions/2551432/liang-chong-fang-fa-ni-xiang-si-wei-zhen-e3gb/

来源：力扣（LeetCode）

著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



**lizhen-667**在参照灵神代码后写出的代码：

class Solution {

public:

&#x20;   int maxScore(vector<int>\& cardPoints, int k) {

&#x20;       

&#x20;       long long sum=0;

&#x20;       long long maxium=0;

&#x20;       int length=cardPoints.size();





&#x20;       for(int i=0;i<k;i++){

&#x20;           sum+=cardPoints\[i];

&#x20;       }

&#x20;       maxium=sum;



&#x20;       for(int i=1;i<=k;i++){

&#x20;           sum+=cardPoints\[length-i]-cardPoints\[k-i];

&#x20;           maxium=max(maxium,sum);



&#x20;       }

&#x20;   

&#x20;   return maxium;

&#x20;   

&#x20;   }

};

