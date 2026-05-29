lower_bound函数的作用是找到第一个>=x的元素的迭代器
upper_boundh函数的作用是找到第一个>x的元素的迭代器
lower_bound(v.begin(),v.end(),x)
upper_bound(v.begin(),v.end(),x)
如果找不到x lower_bound和upper_bound都会返回v.end();
##注意v.end()是一个迭代器，所指向的是v数组的末尾元素的下一个的位置
如果想要直接返回索引 可以使用lower_bound(v.begin(),v.end(),x)-v.begin();
##注意迭代器类型和整型是不同的，不能直接进行比较或者运算，必须先转换成整型才能进行操作
