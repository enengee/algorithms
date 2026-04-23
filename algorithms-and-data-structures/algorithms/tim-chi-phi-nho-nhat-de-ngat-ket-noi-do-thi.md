---
layout: post
title:  "Tìm chi phí nhỏ nhất để ngắt kết nối đồ thị"
date: 2017-07-19 23:15:00 +0700
tags:
  - algorithm
no-post-nav: 0
comments: 1
---

##### **Bài gốc**
[Codeforces 437C](http://codeforces.com/problemset/problem/437/C)

##### **Đề bài**
Cho đồ thị vô hướng n đỉnh và m cạnh, không có quá một cạnh giữa hai đỉnh bất kì. Mỗi đỉnh i có một giá trị nguyên v<sub>i</sub>; để loại bỏ đỉnh i cần chi phí $$ \sum_{j}{}v_{j}$$, với j là đỉnh kết nối trực tiếp với i và chưa bị loại bỏ.

Tìm chi phí nhỏ nhất để ngắt kết nối toàn đồ thị (loại bỏ các đỉnh đến khi không còn cạnh nào).

**Giới hạn:**

* 1 ≤ n ≤ 1000; 0 ≤ m ≤ 2000
* 0 ≤ vi ≤ 10<sup>5</sup>

##### **Định dạng test**
**Input:**

* Dòng đầu là n và m.
* Dòng thứ hai là n số v<sub>i</sub>.
* Tiếp theo là m dòng, mỗi dòng là x và y, cho biết có kết nối giữa đỉnh x và đỉnh y.

Các đỉnh được đánh số từ 1 đến n.

**Output:**
* Một dòng duy nhất là chi phí tối thiểu.

##### **Thuật toán**

Xoá các đỉnh theo thứ tự giảm dần giá trị v của chúng.

Chứng minh: xét cạnh (x, y), khi cạnh này bị xoá thì v<sub>x</sub> hoặc v<sub>y</sub> được cộng vào tổng chi phí.

=> Nếu ta xoá đỉnh theo thứ tự giảm dần v, thì min(v<sub>x</sub>,v<sub>y</sub>) được cộng vào => thu được chi phí tối thiểu.

##### **Source code**

Ngôn ngữ: C++.

{% highlight cpp linenos %}
const int N = 1000 + 5;
int n, m, a[N][N], cost[N], id[N];

bool cmp(int i, int j) {
    return cost[i] > cost[j];
}

int main() {

    ios::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);

    cin >> n >> m;
    FOR(i,1,n) {
        cin >> cost[i];
        id[i] = i;
    }
    sort(id + 1, id + n + 1, cmp);

    FOR(i,1,m) {
        int u, v; cin >> u >> v;
        a[u][v] = a[v][u] = 1;
    }

    int res = 0;
    FOR(i,1,n) {
        int u = id[i];

        int t = 0;
        FOR(v,1,n)
            if (a[u][v] == 1) {
                t += cost[v];
                a[u][v] = a[v][u] = 0;
            }
        res += t;
    }
    cout << res;

    return 0;
}
{% endhighlight %}

##### **Độ phức tạp**
O(Nlog(N) + N<sup>2</sup>)
