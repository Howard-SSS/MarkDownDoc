[mysql索引数据结构](https://github.com/yehongzhi/learningSummary/blob/master/MySQL%E6%95%B0%E6%8D%AE%E5%BA%93/%E8%B0%88%E8%B0%88MYSQL%E7%B4%A2%E5%BC%95%E6%98%AF%E5%A6%82%E4%BD%95%E6%8F%90%E9%AB%98%E6%9F%A5%E8%AF%A2%E6%95%88%E7%8E%87%E7%9A%84.md)

## B树

每个节点存储多个元素,包含键值和数据;叶子节点都在同一层,具有相同深度

![img](https://camo.githubusercontent.com/3b7f642bc171e741f2a43f4f8b27e70eff4f9a9648022c27b4af479aa6c32e7c/68747470733a2f2f7374617469632e6c6f766562696c6962696c692e636f6d2f6d7973716c5f73756f79696e5f322e706e67)

## B+树

B树上改进,只包含键值;叶子结点由双向链表连接(获取大范围,通过链表访问下一个叶子结点,不用在此根节点查询)

![img](https://camo.githubusercontent.com/e1a71d1c1b8a7a7eebdc3ca8ceaceab0fa7396555f92d11548bdea54995c953c/68747470733a2f2f7374617469632e6c6f766562696c6962696c692e636f6d2f6d7973716c5f73756f79696e5f342e706e67)