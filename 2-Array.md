# 配列を使ったリスト
## ArrayStack: 配列を使った高速なスタック操作
ArrayStackは backing array を使ったListインターフェースを実装。

```cpp
array<T> a;
int n;
int size() {
	return n;
}
```

### get(i)/set(i, x) - O(1)
```cpp
T get(int i) {
	return a[i];
}
```

```cpp
T set(int i, T x) {
	T y = a[i];
	a[i] = x;
	return y;
}
```

### add(i, x)/ remove(i) - O(1 + n - i)
addは、インデックスiの位置にxを追加する。

```cpp
void add(int i, T x) {
	if (n + 1 > a.length) resize();
	for (int j = n; j > i; j--)
		a[j] = a[j - 1];

	a[i] = x;
	n++;
}
```

removeは、インデックスiの要素を削除する。

```cpp
T remove(int i) {
	T x = a[i];
	for (int j = i; j < n - 1; j++)
		a[j] = a[j + 1];
	n--;
	if (a.length >= 3 * n) resize();
	return x;
} 
```

空のArrayStackに対して、任意のm個のadd(i, x)およびremove(i)からなる操作の列を実行する。この時、resize()にかかる時間の合計はO(m)である。

```cpp
void resize() {
	array<T> b(max(2 * n, 1));
	for (int i = 0; i < n; i++)
		b[i] = a[i];
	a = b;
}
```

resize()の実行にはO(n)の時間がかかる。

## ArrayQueue: 配列を使ったキュー
ArrayQueueは、FIFO（先入れ先出し）キューを実装するデータ構造。

ArrayQueueは、FIFOのQueueインターフェースの実装である。resize()のコストを無視すると、ArrayQueueはadd(x)、remove()の実行時間はO(1)である。さらに、空のArrayQueueに対して長さmのadd(i, x)およびremove(i)からなる操作の列を実行するとき、resize()にかかる時間の合計はO(m)である。

jはqueueの先頭の位置。

```cpp
array<T> a;
int j;
int n;
```

### add(i, x)/ remove(i) - O(1)
FIFOなので、addは末尾に要素を追加する。

```cpp
bool add(T x) {
	if (n+1 >= a.length) resize();
	a[(j+n) % a.length] = x;
	n++;
	return true;
}
```

removeは先頭の要素を削除する。

```cpp
T remove() {
	T x = a[j]
	j = (j+1) % a.length;
	n--;
	if (a.length >= 3 * n) resize();
	return x;
}
```

resize()はArrayStackの実装に似ている。ただ、queueの先頭のインデックスjは0ではない点が異なる。

```cpp
void resize() {
	array<T> b(max(2 * n, 1));
	for (int k = 0; k < n; k++)
		b[k] = a[(j+k) % a.length];
	a = b;
	j = 0;
}
```

## ArrayDeque: 配列を使った高速な双方向キュー
Dequeは両端に対して追加と削除が効率的に実行できるデータ構造である。

ArrayDequeはListインターフェースを実装する。

- get(i)およびset(i, x)の実行時間はO(1)である
- add(i, x)およびremove(i)の実行時間はO(1 + min{i, n-i})である

```cpp
array<T> a;
int j;
int n;
```

### get(i)/set(i, x) - O(1)
get(i)、set(i, x)はシンプル。

```cpp
T get(int i) {
	return a[(j+i) % a.length];
}
```

```cpp
T set(int i, T x) {
	int k = (j+i) % a.length;
	T y = a[k];
	a[k] = x;
	return y;
}
```

### add(i, x)/ remove(i) - O(1 + min{i, n - i})
add、removeは、i個の要素を左に、またはn-i個の要素を右にずらす。

resize()を無視すれば、計算量はO(1 + min{i, n - i})である。

```cpp
void add(int i, T x) {
	if (n+1 >= a.length) resize();

	// a[0],...,a[i-1] を左に1つずつずらす
	if (i < n/2) { 
		// jが0なら、indexが-1になってしまうため
		j = (j == 0) ? a.length - 1 : j - 1; 
		for (int k = 0; k <= i-1; k++)
			a[(j+k) % a.length] = a[(j+k+1) % a.length];
	} else {
		// a[i],...a[n-1] を右に1つずつずらす
		for (int k = n; k > i; k--)
			a[(j+k) % a.length] = a[(j+k-1) % a.length];
	}

	a[(j+i) % a.length] = x;
	n++;
}
```

removeもaddと同様に実装できる。

```cpp
T remove(int i) {
	T x = a[(j+i) % a.length];

	if (i < n/2) {
		for (int k = i; k > 0; k--)
			a[(j+k) % a.length] = a[(j+k-1) % a.length];
	} else {
		for (int k = i; k < n-1; k++)
			a[(j+k) % a.length] = a[(j+k+1) % a.length];
	}

	n--;
	if (3*n > a.length) resize();
	return x;
}
```
