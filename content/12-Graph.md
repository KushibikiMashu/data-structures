# グラフ
数学における有向グラフ（directed graph）とは、頂点の集合Vと辺の集合Eからなる、組 G = （V, E）である。なお、辺は頂点の組（i, j）であり、iからjに向かっているものとする。iは辺の始点（source）と呼ばれ、jは終点（target）と呼ばれる。

グラフに対する典型的な操作は次のようなものがある。

- addEdge(i,j): 辺（i,j）をEに加える
- removeEdge(i,j): 辺(i,j)をEから除く
- hasEdge(i,j): $$ (i,j) \in E $$ かどうかを調べる
- outEdges(i): $$ (i,j) \in E $$ を満たす整数jのリストを返す
- inEdges(i):  $$ (j,i) \in E $$ を満たす整数jのリストを返す

addEdge, removeEdge, hasEdgeはUSetを使って実装できる（ハッシュテーブルなら実行時間は定数）。outEdge, inEdgeは各頂点ごとに隣接する頂点のリストを保持すれば、定数時間で実行できる。
