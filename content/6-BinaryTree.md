# BinaryTree
**二分木は、コンピュータサイエンスで現れる基本的なデータ構造である。**二分木は、連結（connected）な有限無向グラフであり、閉路（cycle）を持たず、すべての頂点の次数（degree）が3以下のもの。

- rootと呼ばれる特殊なノードrを持つ（rooted）。次数は2以下
- ノードuからrに向かう1つ目のノードはuの**親（parent）**と呼ぶ
- uに隣接する親以外のノードを**子（child）**と呼ぶ
- ノードuの**高さ（height）**は、uからuの子孫への経路の長さの最大値
- ノードuが子を持たない場合、uは**葉（leaf）**という

また、木を考えるとき、外部ノード（external node）で拡張すると便利なことがある（図6.2(b)）。
n >= 1 このノードを持つ二分木は、n+1個の外部ノードを持つことができる。


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
int depth(Node *u) {
	int d = 0;

	while (u != r) {
		u = u->parent;
		d++;
	}

	return d;
}
```

ノードuの高さは、

## 再帰的なアルゴリズム
uを根とする二分木のノードの数（サイズ）は、左の子と右の子を再帰的に辿るステップ数に1（root自身の数）を足して求める。

```cpp
int size(Node *u) {
	if (u == nil) return 0;
	return 1 + size(u->left) + size(u->right);
}
```

ノードuの高さは、uの二つの部分技の高さの最大値を計算し、その結果に1を加える。

```cpp
int height(Node *u) {
	if (u == nil) return -1;
	return 1 + max(height(u->left), height(u->right));
}
```

## 二分木の走査
二分木を再帰的に操作するコードは下記のように書ける。traverseは「走査する」という意味。ASTを辿る場面や、デザインパターンのVisitorパターンで使われる単語。どちらも扱うデータは木構造であるため。

二分木の走査の方法は3通りある。

- 再帰で辿る
- loopで左の子から右の子へ辿る
- 幅優先探索で同じ深さのノードを左から右に辿る

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

# BinarySearchTree: バランスされていない二分探索木
BinarySearchTreeはSSetインターフェースの実装であって、add(x)、remove(x)、find(x)の実行時間はO(n)である。

最悪の場合、二分探索木がアンバランスであり、ほとんどのノードが子を1つだけ持ち、n個のノードからなる長い鎖のような見た目になるかもしれない。

BinarySearchTreeは、次の性質を持つ。

- ノードuについて、u.leftを根とする部分木のデータはすべてu.xより小さい
- 同様に、u.leftの部分木のデータはすべてu.xより大きい

## 探索
xの値を探す。根rからノードuを訪問している時、次の3つの場合がある。

1. x < u.x なら u.leftに進む
1. x > u.x なら u.rightに進む
1. x = u.x なら値が x であるノード u を見つけた

また、u = nil なら探索を終了し、探している値xが木に含まれていないとする。


```cpp
T findEQ(T x) {
	Node *w = r;

	while (w != nil) {
		int comp = compare(x, w->x);
		if (comp < 0) {
			w = w->left;
		} else if (comp > 0) {
			w = w->right;
		} else {
			return w->x;
		}
	}

	return null;
}
```

x以上の値のうちで最小のものを返すためには、最後に探索したノードの値を変数zに記録しておけば良い。

```cpp{2,7,16}
T find(T x) {
	Node *w = r, *z = nil;

	while (w != nil) {
		int comp = compare(x, w->x);
		if (comp < 0) {
			z = w;
			w = w->left;
		} else if (comp > 0) {
			w = w->right;
		} else {
			return w->x;
		}
	}

	return z == nil ? null : z->x;
}
```

## 追加
値xを追加する手順は以下の通り。

1. xを検索して存在すれば、ノードを挿入しない
1. xが存在しなければ、探索で最後に出会ったノードpの子とする

xをノードpの子とするとき、右の子か左の子か、p.xとの比較によって決める。

```cpp
bool add(T x) {
	Node *p = findLast(x);
	Node *u = new Node;
	u->x = x;
	return addChild(p, u);
}
```

```cpp{5,16}
Node* findLast(T x) {
	Node *w = r, *prev = nil;

	while (w != nil) {
		prev = w;
		int comp = compare(x, w->x);
		if (comp < 0) {
			w = w->left;
		} else if (comp > 0) {
			w = w->right;
		} else {
			return w;
		}
	}

	return prev;
}
```

```cpp
// pは最後に見つかった要素、つまりuの親
bool addChild(Node *p, Node *u) {
	if (p == nil) { // r == nil、つまりn == 0のとき
		r = u;
	} else {
		int comp = compare(u->x, p->x);
		if (comp < 0) {
			p->left = u;
		} else if (comp > 0) {
			p->right = u;
		} else { // u.xはすでに木に存在している
			return false;
		}
		u->parent = p;
	}

	n++;
	return true;
}
```

## 削除
ノードuの削除は3パターンある。

1. uが子を持たない（uが葉（leaf））なら、uを削除する
1. uが子を一つだけ持つなら、uの親と子をつなげる（u->parent->left = u->left）
1. uが子を二つ持つなら、子の数が1以下のノードwで w.x >= u.x を満たす最小のwで埋める

spliceは、uが子を持たない、または子を一つだけ持つ場合に、ノードuの親と子を繋ぐ関数。spliceは英語で「継ぎ合わせる」という意味。

```cpp
void splice(Node *u) {
	// 削除するノードの子（またはnil）をs、親をpとする
	Node *s, *p;

	if (u->left == nil) {
		s = u->right; // 子を持たない場合
	} else {
		s = u->left; // 子を1つ持つ場合
	}

	if (u == r) { // 削除するノードがrootである場合
		r = s;
		p = nil;
	} else {
		p = u->paretnt;
		if (p->left == u) {
			p->left = s; // uが親から見て左の子の場合
		} else {
			p->right = s; // uが親から見て右の子の場合
		}
	}

	if (s != nil) {
		s->parent = p;
	}

	n--;
}
```

uが子を二つ持つパターンがややこしいが、「uを根とする部分木の右側の最小の値をuの位置に移動させる」と考えれば処理自体はシンプルである。

removeのelse以下で対応する。

```cpp
void remove(Node *u) {
	if (u->left == nil | u->right == nil) {
		splice(u);
		delete u;
	} else {
		// 部分木の右側の最小の値を探す
		Node *w = u->right;
		while (w->left != nil)
			w = w->left;
		u->x = w->x;
		// 最小の要素を削除する
		splice(w);
		delete w;
	}
}
```
