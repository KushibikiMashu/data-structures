# RedBlackTree
赤黒木（red-black tree）は、木の高さが要素数の対数で抑えられる二分木である。赤黒木の性質は以下の通りである。

1. n個の要素を含む赤黒木の高さは $$2\log n$$ 以下である
1. add(x)およびremove(x)を最悪の場合でも $$O(\log n)$$ の時間で実行できる
1. add(x)およびremove(x)における回転の回数は、償却すると定数である

赤黒木は優れた性質を持つが、実装が複雑である。赤黒木を理解するために、まずは背景知識として2-4木を知る必要がある。


# 2-4木
2-4木の性質は以下である。

1. [高さ] 全ての葉の深さは等しい
1. [次数] 全ての内部ノードは2個以上4個以下の子ノードを持つ

また、n個の葉を持つ2-4木の高さは $$\log n$$ 以下である。

## 葉の追加 （$$\log n$$ 以下）
2-4木に葉を追加するのは簡単。親ノードwの子として葉uを追加したい時は、まず追加をする。その後、次数の制約に沿って処理を行う。wが4つの子を持っていれば、wを分割して子を2つ持つノードwと子を3つ持つノードw\'とする。このとき、w\'の親はwの親とする。そして、この処理を再帰的に繰り返す。

2-4木の高さは常に $$\log n$$ 以下なので、葉の追加は $$\log n$$ ステップ以下で完了する。

## 葉の削除 （$$\log n$$ 以下）
2-4木から葉を削除するためには、まず親wから葉uを削除する。そして、wの子が1つだけになれば、次数の制約を満たさなくなるので、修正の処理を加える。

つまり、wの兄弟w\'を調べて、w\'の子が3つまたは4つなら、そのうち1つをw\'からwに移す。w\'の子が2つなら、wとw\'を併合して、子を3つ持つノードwとする。そして、w\'をw\'の親から取り除く。

この処理は自分自身w、もしくは兄弟w\'が子を３つ以上持つようなノードuが見つかるか、根に到達したら終了する。

2-4木の高さは常に $$\log n$$ 以下なので、葉の削除も追加と同様に $$\log n$$ ステップ以下で完了する。


# RedBlackTree: 2-4木をシミュレートする二分木
赤黒木は、各ノードuが赤か黒の色を持つ二分探索木である。赤ノードは0で、黒ノードは1で表現される。赤黒木の性質は以下である。

1. 葉から根への経路には、いずれも黒ノードが同じ数だけ含まれる。（黒の高さの性質。葉から根への経路について、色の総和は全て等しい）
1. 赤ノード同士は隣接しない（赤の辺の性質。赤の辺はないということ。根でない任意のノードuについて、 `u.color + u.parent.color >= 1`が成り立つ）

これらの性質は、根rがどちらの色であっても満たされる。以下では、根が黒であると仮定する。また、赤黒木を更新するアルゴリズムでは、根は黒のまま保たれるとする。なお、外部ノード(nil)は黒ノードとして扱う。

```cpp
class RedBlackNode : public BSTNode<Node, T> {
	friend class RedBlackTree<Node, T>;
	char color;
}

int red = 0;
int black = 1;
```

## 赤黒木と2-4木
赤黒木は2-4木を二分木として効率的にシミュレートするように設計されている。

例えば、赤黒木Tに対して「全ての赤ノードを取り除き、uの2つの子を両方ともuの親（黒ノード）に接続する」という操作を実行すると、T\'は黒ノードだけを持つ2-4木になるのである。

2-4木T\'はn+1個の葉を持ち、各葉は赤黒木のn+1個の外部ノードと対応する。よって、この木の高さは $$\log (n+1)$$ 以下である。

黒ノードのみを持つ2-4木T\'の高さは $$\log (n+1)$$ 以下なので、赤黒木Tの黒ノードの個数は $$\log (n+1)$$ 以下である。また、2つの内部ノードのうち、赤は1追加なので、赤ノードの個数は $$\log (n+1) - 1$$ 以下である。よって、n>=1について、根から任意の内部ノードへの経路のうちで最長のものの長さは、次の左辺の値以下である

$$$
2\log n+1 - 2 <= 2\log n
$$$

このことから、赤黒木の最も重要な性質を示せる。

3. n個のノードからなる赤黒木の高さは $$2\log n$$ 以下である


