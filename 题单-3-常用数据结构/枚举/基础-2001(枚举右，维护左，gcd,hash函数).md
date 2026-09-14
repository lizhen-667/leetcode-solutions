题意：
用一个下标从 **0** 开始的二维整数数组 `rectangles` 来表示 `n` 个矩形，其中 `rectangles[i] = [widthi, heighti]` 表示第 `i` 个矩形的宽度和高度。

如果两个矩形 `i` 和 `j`（`i < j`）的宽高比相同，则认为这两个矩形 **可互换** 。更规范的说法是，两个矩形满足 `widthi/heighti == widthj/heightj`（使用实数除法而非整数除法），则认为这两个矩形 **可互换** 。

计算并返回 `rectangles` 中有多少对 **可互换** 矩形。

思路：还是枚举右，维护左为基本算法，关键解题点在于将w/h化为最简分数比，然后在利用hash函数将w/h转化为一个特点的标定值，long long key = (long long)w * 100001 + h;  // h<=1e5，编码成单值

代码：
long long interchangeableRectangles(vector<vector<int>>& rectangles) {

        unordered_map<int,int> count;

        long long ans=0;

        for(int i=0;i<rectangles.size();i++){

            int x=rectangles[i][0];

            int y=rectangles[i][1];

            int g=gcd(x,y);

            int gx=x/g;

            int gy=y/g;

            long long key=(long long)100001*gx+gy;

            ans+=count[key];

            count[key]++;

        }

        return ans;

  
  

    }
