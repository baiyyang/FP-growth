# FP-growth
该项目是FP-growth算法的实现，该算法用于快速的寻找关联规则和频繁项集，只需要扫描两次数据库，设计很精妙。
其中算法的伪代码给出如下：

一、FP-Tree构造算法 


输入：事务数据集 D，最小支持度阈值 min_sup 

输出：FP-Tree 

(1) 　　扫描事务数据集 D 一次，获得频繁项的集合 F 和其中每个频繁项的支持度。对 F 中的所有频繁项按其支持度进行降序排序，结果为频繁项表 L ; 

(2) 　　创建一个 FP-Tree 的根节点 T，标记为“null”; 

(3) 　　for 事务数据集 D 中每个事务 Trans do 

(4) 　　　　对 Trans 中的所有频繁项按照 L 中的次序排序; 

(5) 　　　　对排序后的频繁项表以 [p|P] 格式表示，其中 p 是第一个元素，而 P 是频繁项表中除去 p 后剩余元素组成的项表; 

(6) 　　　　调用函数 insert_tree( [p|P], T ); 

(7) 　　end for 


insert_tree( [p|P], root) 

(1) 　　if root 有孩子节点 N and N.item-name=p.item-name then 

(2) 　　　　N.count++; 

(3) 　　Else 

(4) 　　　　创建新节点 N; 

(5) 　　　　N.item-name=p.item-name; 

(6) 　　　　N.count++; 

(7) 　　　　p.parent=root; 

(8) 　　　　将 N.node-link 指向树中与它同项目名的节点; 

(9) 　　end if 

(10)　　if P 非空 then 

(11)　　　　把 P 的第一项目赋值给 p，并把它从 P 中删除; 

(12)　　　　递归调用 insert_tree( [p|P], N); 

(13)　　end if



二、FP-Growth(FP-Tree, α); 

输入：已经构造好的 FP-Tree，项集 α（初值为空），最小支持度 min_sup; 

输出：事务数据集 D 中的频繁项集 L; 

(1) 　　L 初值为空 

(2) 　　if Tree 只包含单个路径 P then 

(3) 　　　　for 路径 P 中节点的每个组合（记为 β） do 

(4) 　　　　　　产生项目集 α∪β，其支持度 support 等于 β 中节点的最小支持度数; 

(5) 　　　　　　return L = L ∪ 支持度数大于 min_sup 的项目集 β∪α 

(6)　　 else　　　　//包含多个路径 

(7) 　　　　for Tree 的头表中的每个频繁项 αf do 

(8) 　　　　　　产生一个项目集 β = αf∪α，其支持度等于 αf 的支持度; 

(9) 　　　　　　构造 β 的条件模式基 B，并根据该条件模式基 B 构造 β 的条件 FP- 树 Treeβ; 

(10) 　　　　　　if Treeβ≠Φ then 

(11) 　　　　　　　　递归调用 FP-Growth(Treeβ, β); 

(12) 　　　　　　end if 

(13) 　　　　end for 

(14)　　end if
