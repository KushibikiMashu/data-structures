# ソートアルゴリズム
データ構造の本であるが、整列アルゴリズムを紹介することには意義がある。例えば、BinaryHeapを使って要素を全てadd(x)し、remove()すれば順番に要素を取り出せる。しかし、BinaryHeapの限られた機能しか使っていないし、メモリ効率も良くない。シンプルに整列をするアルゴリズムを考えることには意義がある。

この節では、以下の３つの整列アルゴリズムを紹介する。

- マージソート
- クイックソート
- ヒープソート

配列aを入力すると、いずれも比較による整列であり、 $$O(n\log n)$$ の期待実行時間でaの要素を昇順にソートする。

なお値を比較するメソッドcompare(a, b)は、以下のように振る舞うものとする。

- a < b なら -1 を返す
- a > b なら 1 を返す
- a = b なら 0 を返す

## マージソート
マージソートは、再帰的な分割統治法の例として古典的なアルゴリズムである。配列aを半分ずつに分け、その配列a0, a1を再帰的に整列し、a0, a1を併合することで、整列済みの配列aを得る。

```cpp
void mergeSort(array<T> &a) {
	if (a.length <= 1) return; // 要素数が1なら整列済み
	array<T> a0(0);
	array<T>::copyOfRange(a0, a, 0, a.length/2); // a0にaのindexが0からn/2までを割り当てる
	array<T> a1(0);
	array<T>::copyOfRange(a1, a, a.length/2, a.length);
	mergeSort(a0);
	mergeSort(a1);
	merge(a0, a1, a); // a0とa1を併合して、aに格納する
}
```

a0, a1の併合は、aに要素を１つずつ加えていけばいい。aに追加する要素は、a0かa1の小さい方である。

```cpp
void merge(array<T> &a0, array<T> &a1, array<T> &a) {
	int i0 = 0, i1 = 0; // a0, a1のindex
	for (int i = 0; i < a.length; i++) {
		if (i0 == a0.length) // i0がa0の長さと同じとき
			a[i] = a1[i1++];
		else if (i1 == a1.length) // i1がa1の長さと同じとき
			a[i] = a0[i0++];
		else if (compare(a0[i0], a1[i1]) < 0) // a0[i0] < a1[i1] のとき
			a[i] = a0[i0++]; 
		else  // a0[i0] >= a1[i1] のとき
			a[i] = a1[i1++]; 
	}
}
```

- mergeSort(a)の実行時間は $$O(n\log n)$$ であり、最大で $$n\log n$$ 回の比較を行う


```cpp

```


```cpp

```


```cpp

```


```cpp

```


