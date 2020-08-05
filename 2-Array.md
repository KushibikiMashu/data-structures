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





