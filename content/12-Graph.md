# グラフ
数学における有向グラフ（directed graph）とは、頂点の集合Vと辺の集合Eからなる、組 G = （V, E）である。なお、辺は頂点の組（i, j）であり、iからjに向かっているものとする。iは辺の始点（source）と呼ばれ、jは終点（target）と呼ばれる。

グラフに対する典型的な操作は次のようなものがある。

- addEdge(i,j): 辺（i,j）をEに加える
- removeEdge(i,j): 辺(i,j)をEから除く
- hasEdge(i,j): $$ (i,j) \in E $$ かどうかを調べる
- outEdges(i): $$ (i,j) \in E $$ を満たす整数jのリストを返す
- inEdges(i):  $$ (j,i) \in E $$ を満たす整数jのリストを返す

addEdge, removeEdge, hasEdgeはUSetを使って実装できる（ハッシュテーブルなら実行時間は定数）。outEdge, inEdgeは各頂点ごとに隣接する頂点のリストを保持すれば、定数時間で実行できる。

## AdjacencyMatrix: 行列によるグラフ表現
n個の頂点を持つグラフ $$G = (V,E)$$ を真偽値を並べたn×n行列aを使って表現したものを、隣接行列（adjacency matrix）と呼ぶ。

```cpp
int n;
bool **a;
```

a[i][j]がtrueのとき、i,jは隣接している（経路がある）
falseのとき、i,jは隣接していない。

隣接行列において、addEdge(i,j)、removeEdge(i,j)、hasEdge(i,j)の各操作は、いずれも行列の要素a[i][j]を読み書きするだけで実装できる。いずれも定数時間で実行できる。

```cpp
void addEdge(int i, int j) {
	a[i][j] = true;
}

void removeEdge(int i, int j) {
	a[i][j] = false;
}

bool hasEdge(int i, int j) {
	return a[i][j];
}
```
