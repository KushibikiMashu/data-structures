# BinaryTree
**二分木は、コンピュータサイエンスで現れる基本的なデータ構造である。**二分木は、連結（connected）な有限無向グラフであり、閉路（cycle）を持たず、すべての頂点の次数（degree）が3以下のもの。

![二分木]()

- rootと呼ばれる特殊なノードrを持つ（rooted）。次数は2以下
- ノードuからrに向かう1つ目のノードはuの**親（parent）**と呼ぶ
- uに隣接する親以外のノードを**子（child）**と呼ぶ
- ノードuの**高さ（height）**は、uからuの子孫への経路の長さの最大値
- ノードuが子を持たない場合、uは**葉（leaf）**という

また、木を考えるとき、外部ノード（external node）で拡張すると便利なことがある（図6.2(b)）。
n >= 1 このノードを持つ二分木は、n+1個の外部ノードを持つことができる。


# 感想・考察
- 
- 
- 

# BinaryTree: 基本的な二分木
ノードuは、uに隣接するノードを明示的に保持するように表現する。

```cpp
class BTNode {
	N *left;
	N *right;
	N *parent;
	BTNode() {
		left = right = parent = NULL;
	}
}
```

rootノードのparentは常にNullである。

```cpp
Node *r;
```

ノードuの深さは、uから親を辿って根に辿り着くまでのステップ数である。

```cpp
int depth() {
	int d = 0;

	while (u != r) {
		u = u->parent;
		d++;
	}

	return d;
}
```

## 再帰的なアルゴリズム
uを根とする二分木のノードの数（サイズ）は、左の子と右の子を再帰的に辿るステップ数に1（root自身の数）を足して求める。

```cpp
int size(Node *u) {
	if (u == nil) return 0;
	return 1 + size(u->left) + size(u->right);
}
```

## 二分木の走査
二分木を再帰的に操作するコードは下記のように書ける。traverseは「走査する」という意味。ASTを辿る場面や、デザインパターンのVisitorパターンで使われる単語。どちらも扱うデータは木構造であるため。

二分木の走査の方法は3通りある。

- 再帰で辿る
- loopで左の子から右の子へ辿る
- 幅優先探索で同じ深さのノードを左から右に辿る

![二分木の走査]()

```cpp
void traverse(Node *u) {
	if (u == nil) return;
	traverse(u->left);
	traverse(u->right);
} 
```

しかし、ノードが多すぎると、スタックオーバーフローを起こしてしまう。再帰を用いずにtraverseを実装する。

```cpp
void traverse2() {
	Node *u = r, *prev = nil, *next;

	// 次に辿るnodeがないとき、u == nilになる
	while (u != nil) {
		if (prev == u->parent) { // 親から降りていく（下方向）
			// 次に辿るノードをnextに格納する
			if (u->left != nil) next = u->left;
			else if (u->right != nil) next = u->right;
			else next = u->parent;
		} else if (prev == u->left) { // 左の子から上に登る（上方向）
			// 左は走査済みなので、右の子か親にしか進まない
			if (u->right != nil) next = u->right;
			else next = u->parent;
		} else { // 右の子から上に登る（上方向）
			// 上に上がるだけ
			next = u->parent;
		}

		prev = u;
		u = next;
	}
}
```

木のサイズを計算するためには、rootからノードを下に辿っていく回数をカウントすればいい。


```cpp
int size2() {
	Node *u = r, *prev = nil, *next;
	int n = 0;

	// 次に辿るnodeがないとき、u == nilになる
	while (u != nil) {
		if (prev == u->parent) { // 親から降りていく（下方向）
			n++;
			// 次に辿るノードをnextに格納する
			if (u->left != nil) next = u->left;
			else if (u->right != nil) next = u->right;
			else next = u->parent;
		} else if (prev == u->left) { // 左の子から上に登る（上方向）
			// 左は走査済みなので、右の子か親にしか進まない
			if (u->right != nil) next = u->right;
			else next = u->parent;
		} else { // 右の子から上に登る（上方向）
			// 上に上がるだけ
			next = u->parent;
		}

		prev = u;
		u = next;
	}

	return n;
}
```

ListかStackを使うと、二分木でparentを使わない実装が可能。

また、別の操作方法としてキューを使った幅優先探索がある。

![幅優先探索]()

キューqは、初期状態は根だけを含む。各ステップでは、qから次のノードuを取り出し、u.left, u.rightを（nilでなければ）qに追加する。幅優先探索は、各深さの左から右に訪問する。

下記はDequeを使った実装。

```cpp
void bfTraverse() {
	ArrayDeque<Node> q;
	if (r != nil) q.add(q.size(), r);

	while (q.size() > 0) {
		Node *u = q.remove(q.size() - 1);
		if (u->left != nil) q.add(q.size(), u->left);
		if (u->right != nil) q.add(q.size(), u->right);
	}
}
```

## 


```cpp

```


```cpp

```


```cpp

```


```cpp

```


