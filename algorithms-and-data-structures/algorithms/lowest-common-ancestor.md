---
layout: post
title:  "Lowest Common Ancestor"
date: 2017-07-27 21:50:00 +0700
tags:
  - algorithm
no-post-nav: 0
comments: 1
---

##### **Bài gốc**
[Codechef TALCA](https://www.codechef.com/problems/TALCA)

##### **Đề bài**
Cho một cây có gốc với n đỉnh và q truy vấn có dạng "u v", cho biết cha chung gần nhất của hai đỉnh u và v, tức là cho biết đỉnh xa gốc nhất là cha của cả u và v.

**Giới hạn:**

* 1 ≤ n ≤ 1e5; 1 ≤ m ≤ 1e5

##### **Định dạng test**
**Input:**

* Dòng đầu là n.
* Tiếp theo là n-1 dòng, mỗi dòng là hai số "u v", cho biết có cạnh nối giữa đỉnh u và v.
* Dòng tiếp theo là q.
* Tiếp theo là q dòng truy vấn, mỗi dòng là hai số "u v".

Các đỉnh được đánh số từ 1 đến n.

**Output:**
* q dòng trả lời cho q truy vấn: cha chung gần nhất của u và v.

**Ví dụ:**

{% highlight text %}
input:
4
1 2
2 3
1 4
3
1 2
2 3
3 4
output:
1 2
2 3
3 4
{% endhighlight %}

##### **Thuật toán**

Sử dụng Sparse Table.

##### **Source code**

Ngôn ngữ: C++.

{% highlight cpp linenos %}
const int N = 300111;
const int logN = 20;
const int ROOT = 1;

int n, q, depth[N], f[N][logN];
vector<int> a[N];

void dfs(int u) {
    FOR(i,1,logN-1)
        f[u][i] = f[f[u][i-1]][i-1];

    for(int v : a[u])
        if (!depth[v]) {
            f[v][0] = u;
            depth[v] = depth[u] + 1;
            dfs(v);
        }
}

int lca(int u, int v) {
    if (depth[u] < depth[v]) swap(u, v);

    DOW(i,logN-1,0)
        if (depth[f[u][i]] >= depth[v])
            u = f[u][i];

    if (u == v) return u;

    DOW(i,logN-1,0)
    if (f[u][i] != f[v][i]) {
        u = f[u][i];
        v = f[v][i];
    }

    return f[u][0];
}

int main() {

    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);

    // read graph
    cin >> n;
    FOR(i,1,n-1) {
        int u, v;
        cin >> u >> v;
        a[u].pb(v);
        a[v].pb(u);
    }

    // lca init
    depth[ROOT] = 1;
    dfs(ROOT);

    // usage: lca(u, v)

    // answer q queries
    cin >> q;
    while(q--) {
        int u, v;
        cin >> u >> v;
        cout << lca(u, v) << '\n';
    }

    return 0;
}
{% endhighlight %}

##### **Độ phức tạp**
Độ phức tạp:
* Để xây dựng mảng f: O(Nlog(N))
* Cho một truy vấn: O(log(N)) => Cho q truy vấn: O(qlog(N))

##### **Tính chất và ứng dụng**
* depth[u] là độ sâu/khoảng cách từ u đến gốc (root), có thể tính khoảng cách từ u đến v (có thể vẽ hình để thấy):

{% highlight cpp linenos %}
int dist(int u, int v) {
    int l = lca(u, v);
    return depth[u] + depth[v] - 2 * depth[l];
}
{% endhighlight %}

* Để tìm LCA cho một cây không gốc (tức là trả lời cho truy vấn "r u v": cho biết LCA của u và v nếu cây nhận r làm gốc), có thể làm như sau:

  * Chọn một đỉnh bất kì t.

  * Tính LCA(t, u, v), LCA(t, u, r), LCA(t, v, r); trong đó LCA(t, u, v) là lca của u và v khi t là gốc (root). *Như trong đoạn code trên, nếu chọn t = ROOT = 1 thì lca(u, v) = lca(ROOT, u, v) = lca(1, u, v).*

  * Nếu 3 giá trị trên bằng nhau (giả sử là w) thì LCA(r, u, v) = w. Ngược lại nếu có hai giá trị bằng nhau, thì LCA(r, u, v) = giá trị còn lại.

{% highlight cpp linenos %}
int lca(int r, int u, int v) {
  int l[3];
  l[0] = lca(r, u);
  l[1] = lca(r, v);
  l[2] = lca(u, v);
  sort(l, l+3);

  if (l[0] == l[2]) return l[0];
  return (l[0] == l[1]) ? l[2] : l[0];
}
{% endhighlight %}

* LCA(r, u, v) = {r, u, v, LCA(1, u, v), LCA(1, u, r), LCA(1, v, r)}.

* Nếu LCA(r, u, v) là x thì tổng khoảng cách (ngắn nhất) dist(x, r) + dist(x, u) + dist(x, v) là ngắn nhất.

##### **Luyện tập**
[Codechef TALCA](https://www.codechef.com/problems/TALCA)

[SPOJ LCA](http://www.spoj.com/problems/LCA/)

[Codeforces 832D](http://codeforces.com/contest/832/problem/D)

##### **Tài liệu tham khảo**
[1] [Các phương pháp giải bài toán LCA]( http://vnoi.info/wiki/algo/data-structures/lca)

[2] [TALCA - Editorial](https://discuss.codechef.com/questions/48548/talca-editorial)
