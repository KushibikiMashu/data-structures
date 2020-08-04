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
インデックスiの位置にxを追加する。

```cpp
void add(int i, T x) {
	if (n + 1 > a.length) resize();
	for (j = n; j > i; j--)
		a[j] = a[j - 1];

	a[i] = x;
	n++;
}
```


空のArrayStackに対して、任意のm個のadd(i, x)およびremove(i)からなる操作の列を実行する。この時、resize()にかかる時間の合計はO(m)である。



```cpp

```

```cpp

```






