# 連結リスト

SLList(singly-linked list、単方向連結リスト)とDLList(doubly-linked list、双方向連結リスト)がある。

SLListは、Queue（add、remove）、Stack（push、pop）の操作をO(1)で実装できる。
DLListは、Queue、Stack、Dequeの操作をO(1)で実装できる。

配列（Array）と比較すると、連結リストの短所はget(i)、set(i, x)が全ての要素に対して定数時間ではなくなる。長所は、ノードへの参照uがあれば、uの削除やuの隣へのノードの挿入が定数時間で実行できる。

## SLList: 単方向連結リスト
SLListは、StackとQueueインターフェースを実装する。push(x)、pop()、add(x)、remove()の実行時間はいずれもO(1)である。

### ノードを定義する
各ノードuは、データu.xと参照u.nextを保持している。列の末尾のノードwにおいては、`w.next = null`である。

```cpp
class Node {
public:
	T x;
	Node *next;
	Node(T x0) {
		x = 0;
		next = NULL;
	}
};
```

先頭と末尾のノードを変数に格納する。

```cpp
Node *head;
Node *tail;
int n;
```

### push() - O(1)
先頭に要素を追加する。

```cpp
T push(T x) {
	Node *u = new Node(x);
	u->next = head;
	head = u;
	if (n == 0) {
		tail = u;
	}
	n++;
	return u;
}
```

### pop() - O(1)
先頭の要素を取り出す。

```cpp
T pop() {
	if (n == 0) return null;
	T x = head->x;
	Node *u = head;
	head = head->next;
	delete u;
	n--;
	if (n == 0) tail = NULL;
	return x;
}
```

### remove() - O(1)
先頭の要素を取り出す。popと実装は同じ。

```cpp
T remove() {
	if (n == 0) return null;
	T x = head->x;
	Node *u = head;
	head = head->next;
	delete u;
	n--;
	if (n == 0) tail = NULL;
	return x;
}
```

### add() - O(1)
末尾に要素を追加する。

```cpp
bool add(T x) {
	Node *u = new Node(x);

	if (n == 0) {
		head = u;
	} else {
		tail->next = u;
	}

	tail = u;
  n++;
	return true;
}
```

## DLList: 双方向連結リスト
DLListは、Listインターフェースを実装する。get(i)、set(i, x)、add(i, x)、remove(i)の実行時間はいずれも`O(1 + min{i, n-i})`である。

### ノードを定義する
前後のノードの参照を持つ。

```cpp
struct Node {
	T x;
	Node *prev, *next;
}
```

先頭のノードの前、末尾のノードの次にはdummyノードを設置する。


```cpp
Node dummy;
int n;
DLList() {
	dummy.next = &dummy;
	dummy.prev = &dummy;
	n = 0;
}
```

i番目のノードを取得する。計算量はO(min{i, n-i})

```cpp
Node* getNode(int i) {
	Node* p;

	if (i < n/2) {
		p = dummy.next;
		for (int j = 0; j < i; j++)
			p = p->next;
	} else {
		p = &dummy;
		for (int j = n; j > i; j--)
			p = p->prev;
	}

	return (p);
}
```

### get()/set() - O(1 + min{i, n-i})
getNode(i)を利用する。


```cpp
T get(int i) {
	return getNode(i)->x;
}
```

```cpp
T set(i, x) {
	Node* u = getNode(i);
	T y = u->x;
	u->x = x;
	return y;
}
```

### add(i, x)/remove(i) - O(1 + min{i, n-i})
ノードwの直前にノードuを追加する。

```cpp
Node* addBefore(Node *w, T x) {
	Node *u = new Node(x);
	u->prev = w->prev;
	u->next = w;
	u->prev->next = u;
	u->next->prev = u;
	n++;
	return u;
}
```

addは、addBeforeとgetNodeを組み合わせる。

```cpp
void add(int i, T x) {
	addBefore(getNode(i), x);
}
```

ノードwを削除する。

```cpp
void remove(Node *w) {
	w->prev->next = w->next;
	w->next->prev = w->prev;
	delete w;
	n--;
}
```

次にremove(i)を実装する。

```cpp
T remove(int i) {
	Node *w = getNode(i);
	T x = w->x;
	remove(w);
	return x;
}
```
