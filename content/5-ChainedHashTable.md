# ChainedHashTable
ChainedHashTableはUSetインターフェースを実装する。resize()のコストを無視すると、ChainedHashTableにおけるadd(x)、remove(x)、find(x)の期待実行時間はO(1)である。

```cpp
array<List> t;
int n;
```

- xのハッシュ値はhash(x)
- リストt[i]は n <= t.length を満たす

## add - O(1)
xをリストt[i]に追加する。

```cpp
bool add(T x) {
  // 同じ値があったらfalseを返す
  if (find(x) != null) return false;
  // リストが満杯だったらリストをresizeする
  if (n+1 > t.length) resize();

  t[hash(x)].add(x);
  n++;
  return true;
}
```

## remove - $O(n_{hash(x)})$
要素の削除。リストの長さをt[i]とすると計算量は$O(n_{hash(x)})$である。

```cpp
T remove(T x) {
  int j = hash(x);

  for (int i = 0; i < t[j].size(); i++) {
    T y = t[j].get(i);
    if (x == y) {
      t[j].remove(i);
      n--;
      return y;
    }
  }

  return null;
}
```

## find - O(n hash(x))
t[hash(x)]を線形に探索する。

```cpp
T find(T x) {
  int j = hash(x);

  for (int i = 0; i < t[j].size(); i++) {
    if (x == t[j].get(i)) {
      return t[j].get(i);
    }
  }

  return null;
}
```

## 計算量
優れたhash関数は、ハッシュテーブルの計算量を`O(n/t.length) = O(1)`にする。
