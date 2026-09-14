1.range-based for + 结构化绑定
for(const auto& [key,value] :  m){
  std::cout << key << " -> " << value << "\n";
}

2.用迭代器遍历
for(auto it=m.begin();it!=m.end();++it){
 std::cout << it->first << " -> " << it->second << "\n";
}