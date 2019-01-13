# Code

[TOC]

## 一 STL some demos

1. sort

```c++
#include <cstdio>
#include <vector>
#include <algorithm>
#include <iostream>

using namespace std;

struct Point {
    int x, y;
    Point() {}
    Point(int xx, int yy) { x = xx; y = yy; }
};

struct Mycomp {
    bool operator() (const Point& p1, const Point& p2)
    {
        bool flag = false;
        if (p1.x >= p2.x) { flag = true; }
        else if (p1.x == p2.x && p1.y >= p2.y) { flag = true; }

        return !flag;
    }
} mycomp;

bool myfunction(const Point& p1, const Point& p2)
{
    bool flag = false;
    if (p1.x >= p2.x) { flag = true; }
    else if (p1.x == p2.x && p1.y >= p2.y) { flag = true; }

    return flag;
}

ostream& operator<< (ostream& os, Point& p)
{
    os << p.x << " " << p.y << endl;
    return os;
}

int main()
{
    vector<Point> vs;
    vs.push_back(Point(7,4));
    vs.push_back(Point(4,4));
    vs.push_back(Point(5,3));
    vs.push_back(Point(5,2));
    vs.push_back(Point(8,2));
    vs.push_back(Point(1,5));
    vs.push_back(Point(2,4));
    vs.push_back(Point(3,2));
    vs.push_back(Point(3,4));

    sort(vs.begin(), vs.end(), mycomp);
    for (int i = 0; i < vs.size(); i++) {
        cout << vs[i] << endl;
    }

    cout << "-------------------------------" << endl;

    sort(vs.begin(), vs.end(), myfunction);
    for (int i = 0; i < vs.size(); i++) {
        cout << vs[i] << endl;
    }
}
```

2. sort_2

```c++
#include <iostream>
#include <algorithm>

using namespace std;

struct Point {
    int x, y;
    Point() {}
    Point(int xx, int yy) { x = xx; y = yy; }
};

class MyComp
{
public:
    bool operator() (const Point& p1, const Point& p2)
    {
        if (p1.x < p2.x) { return true; }
        else if (p1.x == p2.x && p1.y < p2.y) { return true; }
        return false;
    }
} mycomp;

bool myfunc(const Point& p1, const Point& p2)
{
    if (p1.x < p2.x) { return false; }
    else if (p1.x == p2.x && p1.y < p2.y) { return false; }
    return true;
}

ostream& operator<< (ostream& os, const Point& p)
{
    os << "(" << p.x << ", " << p.y << ")";
    return os;
}

int main()
{
    vector<Point> vps;
    vps.push_back(Point(5,4));
    vps.push_back(Point(7,3));
    vps.push_back(Point(7,1));
    vps.push_back(Point(3,4));
    vps.push_back(Point(2,0));
    vps.push_back(Point(6,4));
    vps.push_back(Point(3,4));
    vps.push_back(Point(2,7));
    vps.push_back(Point(1,2));
    sort(vps.begin(), vps.end(), myfunc);

    for (vector<Point>::iterator it = vps.begin(); it < vps.end(); it++) {
        cout << *it << endl;
    }

    return 0;
}
```

3. map

```c++
#include <map>
#include <iostream>

using namespace std;

struct Point {
    int x, y;
    Point() {}
    Point(int xx, int yy) { x = xx; y = yy; }
    bool operator< (const Point& p) const
    {
        if (this->x < p.x) { return true; }
        else if (this->x == p.x && this->y < p.y) { return true; }
        return false;
    }
    /*
    bool operator== (const Point& p)
    {
        if (this->x == p.x && this->y == p.y) { return true; }
        return false;
    }
    */
};

bool mycomp(const Point& p1, const Point& p2)
{
    if (p1.x < p2.x) { return true; }
    else if (p1.x == p2.x && p1.y < p2.y) { return true; }
    return false;
}

ostream& operator<< (ostream& os, Point p)
{
    os << "(" << p.x << ", " << p.y << ")";
    return os;
}

int main()
{
    bool(*fn_pt)(const Point&, const Point&) = mycomp;

    map<Point, int, bool(*)(const Point&, const Point&)> pi(fn_pt);
    pi.insert(make_pair(Point(1,1), 1));
    pi[Point(1,2)] = 2;
    pi.insert(make_pair(Point(3,4), 4));
    pi.insert(make_pair(Point(4,5), 3));
    pi.insert(make_pair(Point(2,1), 8));
    pi.insert(make_pair(Point(7,5), 9));
    pi.insert(make_pair(Point(7,4), 1));
    pi.insert(make_pair(Point(6,3), 1));

    cout << pi[Point(1,1)] << endl;
    cout << pi.size() << endl;

    for (map<Point, int, bool(*)(const Point&, const Point&)>::iterator it = pi.begin(); it != pi.end(); it++) {
        cout << it->first << " " << it->second << endl;
    }

    return 0;
}
```

4. set

```c++
#include <iostream>
#include <set>
#include <map>

using namespace std;

struct Point {
    int x, y;
    Point() {}
    Point(int xx, int yy) { x = xx; y = yy; }

    bool operator< (const Point& p) const
    {
        if (this->x < p.x) { return true; }
        else if (this->x == p.x && this->y < p.y) { return true; }
        return false;
    }
};

ostream& operator<<(ostream& os, const Point& p)
{
    os << "(" << p.x << ", " << p.y << ")";
    return os;
}

int main()
{
    set<Point> ps;
    ps.insert(Point(1,1));
    ps.insert(Point(3,4));
    ps.insert(Point(2,9));
    ps.insert(Point(3,6));

    cout << ps.size() << endl;
    for (set<Point>::iterator it = ps.begin(); it != ps.end(); it++) {
        cout << *it << endl;
    }
    cout << "-----------------------" << endl;

    map<Point, int> mpi;
    mpi[Point(9,1)] = 1;
    mpi[Point(4,5)] = 5;
    mpi[Point(6,3)] = 3;
    mpi[Point(6,1)] = 7;
    mpi[Point(2,7)] = 9;
    cout << mpi.size() << endl;
    for (map<Point, int>::iterator it = mpi.begin(); it != mpi.end(); it++) {
        cout << it->first << " : " << it->second << endl;
    }

    return 0;
}
```

5. priority_queue

```c++
#include <iostream>
#include <queue>
#include <vector>

using namespace std;

struct Point {
    int x, y;
    Point() {}
    Point(int xx, int yy) { x = xx; y = yy; }

    /*
    bool operator< (const Point& p)
    {
        if (this->x < p.x) { return true; }
        else if (this->x == p.x && this->y < p.y) { return true; }
        return false;
    }
    */
};

class Mycomp
{
public:
    bool operator() (const Point& p1, const Point& p2) const
    {
        if (p1.x < p2.x) { return true; }
        else if (p1.x == p2.x && p1.y < p2.y) { return true; }
        return false;
    }
};

ostream& operator<< (ostream& os, const Point& p)
{
    os << "(" << p.x << ", " << p.y << ")";
    return os;
}

int main()
{
    priority_queue<Point, vector<Point>, Mycomp> ppq;
    ppq.push(Point(7, 2));
    ppq.push(Point(4, 1));
    ppq.push(Point(3, 5));
    ppq.push(Point(8, 1));
    ppq.push(Point(2, 1));
    ppq.push(Point(3, 3));

    while (!ppq.empty()) {
        cout << ppq.top() << endl;
        ppq.pop();
    }

    /*
    priority_queue<int, vector<int>, greater<int>> pq;
    pq.push(5);
    pq.push(3);
    pq.push(8);
    pq.push(1);

    cout << pq.top() << endl;
    */

    return 0;
}
```

## 二 图论算法

1. dfs

```c++
#include <cstdio>
#include <iostream>
#include <vector>

using namespace std;

#define N 10005
#define WHITE 0
#define GRAY 1
#define BLACK 2

int n;
vector<int> G[N];
int color[N], d[N], f[N], tt;

void dfs_visit(int u)
{
    color[u] = GRAY;
    d[u] = ++tt;
    int v;
    for (int i = 0; i < G[u].size(); i++) {
        if (color[G[u][i]] == WHITE) { dfs_visit(G[u][i]); }
    }
    color[u] = BLACK;
    f[u] = ++tt;
}

void dfs()
{
    // init
    tt = 0;
    for (int i = 1; i <= n; i++) {
        color[i] = WHITE;
    }

    for (int i = 1; i <= n; i++) {
        if (color[i] == WHITE) { dfs_visit(i); }
    }

    for (int i = 1; i <= n; i++) {
        printf("%d %d %d\n", i, d[i], f[i]);
    }
}

int main()
{
    scanf("%d", &n);
    for (int i = 1; i <= n; i++) {
        int u, k;
        scanf(" %d %d", &u, &k);
        for (int j = 1; j <= k; j++) {
            int v; scanf(" %d", &v);
            G[u].push_back(v);
        }
    }

    dfs();

    return 0;
}
```

2. bfs

```c++
#include <cstdio>
#include <iostream>
#include <vector>
#include <queue>

using namespace std;

#define N 1005
#define WHITE 0
#define GRAY 1
#define BLACK 2
vector<int> G[N];

int color[N], d[N];
int n;

void bfs(int s)
{
    // init
    for (int i = 1; i <= n; i++) {
        color[i] = WHITE;
        d[i] = -1;
    }

    queue<int> q;
    q.push(s);
    d[s] = 0;

    while (!q.empty()) {
        int u = q.front(); q.pop();
        for (int i = 0; i < G[u].size(); i++) {
            int v = G[u][i];
            if (color[v] == WHITE) {
                // visit!
                color[v] = GRAY;
                d[v] = d[u] + 1;
                q.push(v);
            }
        }
        color[u] = BLACK;
    }

    for (int i = 1; i <= n; i++) {
        cout << i << " " << d[i] << endl;
    }
}

int main()
{
    scanf("%d", &n);
    for (int i = 1; i <= n; i++) {
        int u, k;
        scanf(" %d %d", &u, &k);
        for (int j = 1; j <= k; j++) {
            int v;
            scanf(" %d", &v);
            G[u].push_back(v);
            G[v].push_back(u);
        }
    }

    bfs(1);

    return 0;
}
```

3. prim

```c++
#include <cstdio>
#include <iostream>

using namespace std;

#define N 1005
#define INF (1<<30)
#define WHITE 0
#define GRAY 1
#define BLACK 2

int graph[N][N];
int n;

int prim(int s)
{
    // init
    int color[N], d[N], f[N];
    for (int i = 1; i <= n; i++) {
        color[i] = WHITE;
        d[i] = INF;
        f[i] = -1;
    }
    d[s] = 0;
    color[s] = GRAY;

    while (1) {
        int u = -1;
        int minu = INF;
        for (int v = 1; v <= n; v++) {
            if (color[v] != BLACK && d[v] < minu) {
                u = v;
                minu = d[v];
            }
        }
        if (u == -1) { break; }
        color[u] = BLACK;

        for (int v = 1; v <= n; v++) {
            if (graph[u][v] != -1 && color[v] != BLACK && graph[u][v] < d[v]) {
                d[v] = graph[u][v];
                f[v] = u;
                color[v] = GRAY;
            }
        }
    }

    int sum = 0;
    for (int i = 1; i <= n; i++) {
        if (f[i] != -1) {
            sum += graph[f[i]][i];
        }
    }
    printf("%d\n", sum);
}

int main()
{
    scanf("%d", &n);
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            scanf(" %d", &graph[i][j]);
        }
    }

    prim(1);

    return 0;
}
```

4. dijkstra

```c++
#include <cstdio>
#include <iostream>

using namespace std;

#define N 1005
#define INF (1<<30)
#define WHITE 0
#define GRAY 1
#define BLACK 2

int n;
int graph[N][N];
int color[N], d[N], f[N];

void dijkstr(int s)
{
    // init
    for (int i = 0; i < n; i++) {
        color[i] = WHITE;
        d[i] = INF;
        f[i] = -1;
    }

    d[s] = 0;
    color[s] = GRAY;
    while (1) {
        int u = -1;
        int minu = INF;
        for (int i = 0; i < n; i++) {
            if (color[i] != BLACK && d[i] < minu) {
                u = i;
                minu = d[i];
            }
        }

        if (-1 == u) { break; }
        color[u] = BLACK;
        for (int v = 0; v < n; v++) {
            if (color[v] != BLACK && graph[u][v] != INF) {
                if (d[u] + graph[u][v] < d[v]) {
                    d[v] = d[u] + graph[u][v];
                    f[v] = u;
                    color[v] = GRAY;
                }
            }
        }
    }

    for (int i = 0; i < n; i++) {
        printf("%d %d\n", i, d[i]);
    }
}

int main()
{
    scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        for (int j = 0; j <= n; j++) {
            graph[i][j] = INF;
        }
    }

    for (int i = 0; i < n; i++) {
        int u, k;
        scanf(" %d %d", &u, &k);

        for (int j = 1; j <= k; j++) {
            int v, w;
            scanf(" %d %d", &v, &w);
            graph[u][v] = w;
            graph[v][u] = w;
        }
    }

    dijkstr(0);

    return 0;
}
```

5. disset

```c++
#include <iostream>
#include <cstdio>

using namespace std;

#define N 1005

int f[N];
int rankk[N];
int n, m;

void make_set(int n)
{
    for (int i = 0; i < n; i++) {
        f[i] = i;
        rankk[i] = 1;
    }
}

int find_f(int i)
{
    if (i != f[i]) {
        f[i] = find_f(f[i]);
    }

    return f[i];
}

void union_set(int x, int y)
{
    x = find_f(x);
    y = find_f(y);
    if (rankk[x] < rankk[y]) {
        f[x] = y;
    } else {
        f[y] = x;
        if (rankk[x] == rankk[y]) {
            rankk[x]++;
        }
    }
}

bool same(int x, int y)
{
    return find_f(x) == find_f(y);
}

int main()
{
    scanf("%d %d", &n, &m);

    // init
    make_set(n);

    for (int i = 1; i <= m; i++) {
        int type, x, y;
        scanf(" %d %d %d", &type, &x, &y);
        if (0 == type) {
            union_set(x, y);
        } else {
            if (same(x, y)) {
                printf("%d\n", 1);
            } else {
                printf("%d\n", 0);
            }
        }
    }

    return 0;
}
```

6. floyd

```c++
#include <cstdio>
#include <iostream>

using namespace std;

#define N 1005
#define INF (1<<30)
int graph[N][N];
int n, e;

void floyd()
{
    for (int k = 1; k <= n; k++) {
        for (int i = 1; i <= n; i++) {
            if (graph[i][k] == INF) { continue; }
            for (int j = 1; j <= n; j++) {
                if (graph[k][j] == INF) { continue; }
                if (graph[i][k] + graph[k][j] < graph[i][j]) {
                    graph[i][j] = graph[i][k] + graph[k][j];
                }
            }
        }
    }
}

int main()
{
    scanf("%d %d", &n, &e);
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            graph[i][j] = (i == j ? 0 : INF);
        }
    }

    for (int i = 1; i <= e; i++) {
        int u, v, w;
        scanf(" %d %d %d", &u, &v, &w);
        graph[u][v] = w;
    }

    floyd();

    bool flag = false;
    for (int i = 1; i <= n; i++) {
        if (graph[i][i] < 0) { flag = true; break; }
    }

    if (flag) { cout << "NEG!" << endl; }
    else {
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                if (j-1) { cout << " "; }
                if (graph[i][j] == INF) {
                    cout << "INF";
                } else {
                    cout << graph[i][j];
                }
            }
            cout << endl;
        }
    }

    return 0;
}
```

7. topo sort

```c++

#include <iostream>
#include <cstdio>
#include <cstring>
#include <vector>

using namespace std;

#define N 1005

int n, e;
vector<int> G[N];
int ind[N];
bool visited[N];
vector<int> out;

void topo()
{
    out.clear();
    memset(visited, 0, sizeof(visited));

    while (out.size() != n) {
        int u = -1;
        for (int i = 0; i < n; i++) {
            if (!visited[i] && ind[i] == 0) {
                out.push_back(i);
                u = i;
                visited[i] = true;

                for (int j = 0; j < G[i].size(); j++) {
                    int v = G[i][j];
                    ind[v]--;
                }
                break;
            }
        }

        if (-1 == u) { break; }
    }

    for (int i = 0; i < out.size(); i++) {
        if (i) { cout << " "; }
        cout << out[i];
    }
    cout << endl;
}


int main()
{
    scanf("%d %d", &n, &e);
    memset(ind, 0, sizeof(ind));

    for (int i = 0; i < e; i++) {
        int u, v;
        scanf(" %d %d", &u, &v);
        G[u].push_back(v);
        ind[v]++;
    }

    topo();

    return 0;
}
```

## 三 STL API

### 1 **string**

#### 1.1 基本

1. constructor

```c++
explicit string ( );
string ( const string& str );
string ( const string& str, size_t pos, size_t n = npos );
string ( const char * s, size_t n );
string ( const char * s );
string ( size_t n, char c );
template<class InputIterator> string (InputIterator begin, InputIterator end);
```

```c++
// string constructor
#include <iostream>
#include <string>
using namespace std;

int main ()
{
  // constructors used in the same order as described above:
  string s1;
  string s5 ("Initial string");
  string s2 (s5);
  string s3 (s5, 8, 3);
  string s4 (s5, 8);
  string s6 (10, 'x');
  string s7a (10, 42);
  string s7b (s5.c_str(), s5.c_str()+4);

  cout << "s1: " << s1 << "\ns2: " << s2 << "\ns3: " << s3;
  cout << "\ns4: " << s4 << "\ns5: " << s5 << "\ns6: " << s6;
  cout << "\ns7a: " << s7a << "\ns7b: " << s7b << endl;
  return 0;
}
```

output:
s1: 
s2: Initial string
s3: str
s4: Initial
s5: Initial string
s6: xxxxxxxxxx
s7a: **********s
7b: Init

#### 1.2 Iterator

1. rbegin

```c++
reverse_iterator rbegin();
const_reverse_iterator rbegin() const;
```

```c++
// string::rbegin and string::rend
#include <iostream>
#include <string>
using namespace std;

int main ()
{
  string str ("now step live...");
  string::reverse_iterator rit;
  for ( rit=str.rbegin() ; rit < str.rend(); rit++ )
    cout << *rit;
  return 0;
}
```

#### 1.3 Capacity

1. resize

```c++
void resize ( size_t n, char c );
void resize ( size_t n );
```

> Resizes the string content to n characters.
> If n is smaller than the current length of the string, the content is reduced to its first n characters, the rest being dropped.
> If n is greater than the current length of the string, the content is expanded by appending as many instances of the c character as needed to reach a size of n characters.　
> The second version, actually calls: resize(n,char()), so when a string is resized to a greater size without passing a second argument, the new character positions are filled with the default value of a char, which is the null character.

```c++
// resizing string
#include <iostream>
#include <string>
using namespace std;

int main ()
{
  size_t sz;
  string str ("I like to code in C");
  cout << str << endl;

  sz=str.size();

  str.resize (sz+2,'+');
  cout << str << endl;

  str.resize (14);
  cout << str << endl;
  return 0;
}
```

#### 1.4 Modifiers

1. assign

```c++
string& assign ( const string& str );
string& assign ( const string&, size_t pos, size_t n );
string& assign ( const char* s, size_t n );
string& assign ( const char* s );
string& assign ( size_t n, char c );
template <class InputIterator>
   string& assign ( InputIterator first, InputIterator last );
```

----------------------------

```c++
// string::assign
#include <iostream>
#include <string>
using namespace std;

int main ()
{
  string str;
  string base="The quick brown fox jumps over a lazy dog.";

  // used in the same order as described above:

  str.assign(base);
  cout << str << endl;

  str.assign(base,10,9);
  cout << str << endl;         // "brown fox"

  str.assign("pangrams are cool",7);
  cout << str << endl;         // "pangram"

  str.assign("c-string");
  cout << str << endl;         // "c-string"

  str.assign(10,'*');
  cout << str << endl;         // "**********"

  str.assign<int>(10,0x2D);
  cout << str << endl;         // "----------"

  str.assign(base.begin()+16,base.end()-12);
  cout << str << endl;         // "fox jumps over"

  return 0;
}
```

2. insert

```c++
string& insert ( size_t pos1, const string& str );
 string& insert ( size_t pos1, const string& str, size_t pos2, size_t n );
 string& insert ( size_t pos1, const char* s, size_t n);
 string& insert ( size_t pos1, const char* s );
 string& insert ( size_t pos1, size_t n, char c );
iterator insert ( iterator p, char c );
    void insert ( iterator p, size_t n, char c );
template<class InputIterator>
    void insert ( iterator p, InputIterator first, InputIterator last );
```

```c++
// inserting into a string
#include <iostream>
#include <string>
using namespace std;

int main ()
{
  string str="to be question";
  string str2="the ";
  string str3="or not to be";
  string::iterator it;

  // used in the same order as described above:
  str.insert(6,str2);                 // to be (the )question
  str.insert(6,str3,3,4);             // to be (not )the question
  str.insert(10,"that is cool",8);    // to be not (that is )the question
  str.insert(10,"to be ");            // to be not (to be )that is the question
  str.insert(15,1,':');               // to be not to be(:) that is the question
  it = str.insert(str.begin()+5,','); // to be(,) not to be: that is the question
  str.insert (str.end(),3,'.');       // to be, not to be: that is the question(...)
  str.insert (it+2,str3.begin(),str3.begin()+3); // (or )

  cout << str << endl;
  return 0;
}
```

3. erase

```c++
string& erase ( size_t pos = 0, size_t n = npos );
iterator erase ( iterator position );
iterator erase ( iterator first, iterator last );
```

```c++
// string::erase
#include <iostream>
#include <string>
using namespace std;

int main ()
{
  string str ("This is an example phrase.");
  string::iterator it;

  // erase used in the same order as described above:
  str.erase (10,8);
  cout << str << endl;        // "This is an phrase."

  it=str.begin()+9;
  str.erase (it);
  cout << str << endl;        // "This is a phrase."

  str.erase (str.begin()+5, str.end()-7);
  cout << str << endl;        // "This phrase."
  return 0;
}
```

4. replace

```c++
string& replace ( size_t pos1, size_t n1,   const string& str );
string& replace ( iterator i1, iterator i2, const string& str );

string& replace ( size_t pos1, size_t n1, const string& str, size_t pos2, size_t n2 );

string& replace ( size_t pos1, size_t n1,   const char* s, size_t n2 );
string& replace ( iterator i1, iterator i2, const char* s, size_t n2 );

string& replace ( size_t pos1, size_t n1,   const char* s );
string& replace ( iterator i1, iterator i2, const char* s );

string& replace ( size_t pos1, size_t n1,   size_t n2, char c );
string& replace ( iterator i1, iterator i2, size_t n2, char c );

template<class InputIterator>
   string& replace ( iterator i1, iterator i2, InputIterator j1, InputIterator j2 );
```

```c++
// replacing in a string
#include <iostream>
#include <string>
using namespace std;

int main ()
{
  string base="this is a test string.";
  string str2="n example";
  string str3="sample phrase";
  string str4="useful.";

  // function versions used in the same order as described above:

  // Using positions:                 0123456789*123456789*12345
  string str=base;                // "this is a test string."
  str.replace(9,5,str2);          // "this is an example string."
  str.replace(19,6,str3,7,6);     // "this is an example phrase."
  str.replace(8,10,"just all",6); // "this is just a phrase."
  str.replace(8,6,"a short");     // "this is a short phrase."
  str.replace(22,1,3,'!');        // "this is a short phrase!!!"

  // Using iterators:                      0123456789*123456789*
  string::iterator it = str.begin();   //  ^
  str.replace(it,str.end()-3,str3);    // "sample phrase!!!"
  str.replace(it,it+6,"replace it",7); // "replace phrase!!!"
  it+=8;                               //          ^
  str.replace(it,it+6,"is cool");      // "replace is cool!!!"
  str.replace(it+4,str.end()-4,4,'o'); // "replace is cooool!!!"
  it+=3;                               //             ^
  str.replace(it,str.end(),str4.begin(),str4.end());
                                       // "replace is useful."
  cout << str << endl;
  return 0;
}
```

5. copy

```c++
size_t copy ( char* s, size_t n, size_t pos = 0) const;
```

```c++
// string::copy
#include <iostream>
#include <string>
using namespace std;

int main ()
{
  int length;
  char buffer[20];
  string str ("Test string...");
  length=str.copy(buffer,6,5);
  buffer[length]='\0';
  cout << "buffer contains: " << buffer << "\n";
  return 0;
}
```

#### 1.5 String operations

1. find

```c++
size_t find ( const string& str, size_t pos = 0 ) const;
size_t find ( const char* s, size_t pos, size_t n ) const;
size_t find ( const char* s, size_t pos = 0 ) const;
size_t find ( char c, size_t pos = 0 ) const;
```

```c++
// string::find
#include <iostream>
#include <string>
using namespace std;

int main ()
{
  string str ("There are two needles in this haystack with needles.");
  string str2 ("needle");
  size_t found;

  // different member versions of find in the same order as above:
  found=str.find(str2);
  if (found!=string::npos)
    cout << "first 'needle' found at: " << int(found) << endl;

  found=str.find("needles are small",found+1,6);
  if (found!=string::npos)
    cout << "second 'needle' found at: " << int(found) << endl;

  found=str.find("haystack");
  if (found!=string::npos)
    cout << "'haystack' also found at: " << int(found) << endl;

  found=str.find('.');
  if (found!=string::npos)
    cout << "Period found at: " << int(found) << endl;

  // let's replace the first needle:
  str.replace(str.find(str2),str2.length(),"preposition");
  cout << str << endl;

  return 0;
}
```

2. rfind

```c++
size_t find ( const string& str, size_t pos = npos ) const;
size_t find ( const char* s, size_t pos, size_t n ) const;
size_t find ( const char* s, size_t pos = npos ) const;
size_t find ( char c, size_t pos = npos ) const;
```

```c++
// string::rfind
#include <iostream>
#include <string>
using namespace std;

int main ()
{
  string str ("The sixth sick sheik's sixth sheep's sick.");
  string key ("sixth");
  size_t found;

  found=str.rfind(key);
  if (found!=string::npos)
    str.replace (found,key.length(),"seventh");

  cout << str << endl;

  return 0;
}
```

3. find_first_of

```c++
size_t find_first_of ( const string& str, size_t pos = 0 ) const;
size_t find_first_of ( const char* s, size_t pos, size_t n ) const;
size_t find_first_of ( const char* s, size_t pos = 0 ) const;
size_t find_first_of ( char c, size_t pos = 0 ) const;
```

```c++
// string::find_first_of
#include <iostream>
#include <string>
using namespace std;

int main ()
{
  string str ("Replace the vowels in this sentence by asterisks.");
  size_t found;

  found=str.find_first_of("aeiou");
  while (found!=string::npos)
  {
    str[found]='*';
    found=str.find_first_of("aeiou",found+1);
  }

  cout << str << endl;

  return 0;
}
```

4. find_last_of

```c++
size_t find_last_of ( const string& str, size_t pos = npos ) const;
size_t find_last_of ( const char* s, size_t pos, size_t n ) const;
size_t find_last_of ( const char* s, size_t pos = npos ) const;
size_t find_last_of ( char c, size_t pos = npos ) const;
```

```c++
// string::find_last_of
#include <iostream>
#include <string>
using namespace std;

void SplitFilename (const string& str)
{
  size_t found;
  cout << "Splitting: " << str << endl;
  found=str.find_last_of("/\\");
  cout << " folder: " << str.substr(0,found) << endl;
  cout << " file: " << str.substr(found+1) << endl;
}

int main ()
{
  string str1 ("/usr/bin/man");
  string str2 ("c:\\windows\\winhelp.exe");

  SplitFilename (str1);
  SplitFilename (str2);

  return 0;
}
```

5. find_first_not_of

```c++
size_t find_first_not_of ( const string& str, size_t pos = 0 ) const;
size_t find_first_not_of ( const char* s, size_t pos, size_t n ) const;
size_t find_first_not_of ( const char* s, size_t pos = 0 ) const;
size_t find_first_not_of ( char c, size_t pos = 0 ) const;
```

```c++
// string::find_first_not_of
#include <iostream>
#include <string>
using namespace std;

int main ()
{
  string str ("look for non-alphabetic characters...");
  size_t found;

  found=str.find_first_not_of("abcdefghijklmnopqrstuvwxyz ");
  if (found!=string::npos)
  {
    cout << "First non-alphabetic character is " << str[found];
    cout << " at position " << int(found) << endl;
  }

  return 0;
}
```

6. find_last_not_of

```c++
size_t find_last_not_of ( const string& str, size_t pos = npos ) const;
size_t find_last_not_of ( const char* s, size_t pos, size_t n ) const;
size_t find_last_not_of ( const char* s, size_t pos = npos ) const;
size_t find_last_not_of ( char c, size_t pos = npos ) const;
```

```c++
// string::find_last_not_of
#include <iostream>
#include <string>
using namespace std;

int main ()
{
  string str ("erase trailing white-spaces   \n");
  string whitespaces (" \t\f\v\n\r");
  size_t found;
  
  found=str.find_last_not_of(whitespaces);
  if (found!=string::npos)
    str.erase(found+1);
  else
    str.clear();            // str is all whitespace

  cout << '"' << str << '"' << endl;

  return 0;
}
```

7. substr

```c++
string substr ( size_t pos = 0, size_t n = npos ) const;
```

```c++
// string::substr
#include <iostream>
#include <string>
using namespace std;

int main ()
{
  string str="We think in generalities, but we live in details.";
                             // quoting Alfred N. Whitehead
  string str2, str3;
  size_t pos;

  str2 = str.substr (12,12); // "generalities"

  pos = str.find("live");      // position of "live" in str
  str3 = str.substr (pos);   // get from "live" to the end

  cout << str2 << ' ' << str3 << endl;

  return 0;
}
```

8. compare

```c++
int compare ( const string& str ) const;
int compare ( const char* s ) const;
int compare ( size_t pos1, size_t n1, const string& str ) const;
int compare ( size_t pos1, size_t n1, const char* s) const;
int compare ( size_t pos1, size_t n1, const string& str, size_t pos2, size_t n2 ) const;
int compare ( size_t pos1, size_t n1, const char* s, size_t n2) const;
```

> 0 if the compared characters sequences are equal, otherwise a number different from 0 is returned, with its sign indicating whether the object is considered greater than the comparing string passed as parameter (positive sign), or smaller (negative sign).
> If either pos1 or pos2 is specified with a position greater than the size of the corresponding string object, an exception of type out_of_range is thrown.

```c++
// comparing apples with apples
#include <iostream>
#include <string>
using namespace std;

int main ()
{
  string str1 ("green apple");
  string str2 ("red apple");

  if (str1.compare(str2) != 0)
    cout << str1 << " is not " << str2 << "\n";

  if (str1.compare(6,5,"apple") == 0)
    cout << "still, " << str1 << " is an apple\n";

  if (str2.compare(str2.size()-5,5,"apple") == 0)
    cout << "and " << str2 << " is also an apple\n";

  if (str1.compare(6,5,str2,4,5) == 0)
    cout << "therefore, both are apples\n";

  return 0;
}
```

### **2 stack**

size empty top push pop

- constructor

```c++
explicit stack ( const Container& ctnr = Container() );
```

```c++
// constructing stacks
#include <iostream>
#include <vector>
#include <deque>
#include <stack>
using namespace std;

int main ()
{
  deque<int> mydeque (3,100);     // deque with 3 elements
  vector<int> myvector (2,200);   // vector with 2 elements

  stack<int> first;               // empty stack
  stack<int> second (mydeque);    // stack initialized to copy of deque

  stack<int,vector<int> > third;  // empty stack using vector
  stack<int,vector<int> > fourth (myvector);

  cout << "size of first: " << (int) first.size() << endl;
  cout << "size of second: " << (int) second.size() << endl;
  cout << "size of third: " << (int) third.size() << endl;
  cout << "size of fourth: " << (int) fourth.size() << endl;

  return 0;
}
```

### **3 queue**

size empty front back push pop

- constructor

```c++
explicit stack ( const Container& ctnr = Container() );
```

```c++
// constructing stacks
#include <iostream>
#include <vector>
#include <deque>
#include <stack>
using namespace std;

int main ()
{
  deque<int> mydeque (3,100);     // deque with 3 elements
  vector<int> myvector (2,200);   // vector with 2 elements

  stack<int> first;               // empty stack
  stack<int> second (mydeque);    // stack initialized to copy of deque

  stack<int,vector<int> > third;  // empty stack using vector
  stack<int,vector<int> > fourth (myvector);

  cout << "size of first: " << (int) first.size() << endl;
  cout << "size of second: " << (int) second.size() << endl;
  cout << "size of third: " << (int) third.size() << endl;
  cout << "size of fourth: " << (int) fourth.size() << endl;

  return 0;
}
```

### **4 priority_queue**

size empty top push pop

- constructor

```c++
explicit priority_queue ( const Compare& x = Compare(),
                          const Container& y = Container() );
template <class InputIterator>
         priority_queue ( InputIterator first, InputIterator last,
                          const Compare& x = Compare(),
                          const Container& y = Container() );
```

```c++
// constructing priority queues
#include <iostream>
#include <queue>
using namespace std;

class mycomparison
{
  bool reverse;
public:
  mycomparison(const bool& revparam=false)
    {reverse=revparam;}
  bool operator() (const int& lhs, const int&rhs) const
  {
    if (reverse) return (lhs>rhs);
    else return (lhs<rhs);
  }
};

int main ()
{
  int myints[]= {10,60,50,20};

  priority_queue<int> first;
  priority_queue<int> second (myints,myints+3);
  priority_queue< int, vector<int>, greater<int> > third (myints,myints+3);

  // using mycomparison:
  priority_queue< int, vector<int>, mycomparison > fourth;

  typedef priority_queue<int,vector<int>,mycomparison> mypq_type;
  mypq_type fifth (mycomparison());
  mypq_type sixth (mycomparison(true));

  return 0;
}
```

### **5 set**

#### 5.1 member functions

1. constructor

```c++
explicit set ( const Compare& comp = Compare(),
               const Allocator& = Allocator() );
template <class InputIterator>
  set ( InputIterator first, InputIterator last,
        const Compare& comp = Compare(), const Allocator& = Allocator() );
set ( const set<Key,Compare,Allocator>& x );
```

```c++
// constructing sets
#include <iostream>
#include <set>
using namespace std;

bool fncomp (int lhs, int rhs) {return lhs<rhs;}

struct classcomp {
  bool operator() (const int& lhs, const int& rhs) const
  {return lhs<rhs;}
};

int main ()
{
  set<int> first;                           // empty set of ints

  int myints[]= {10,20,30,40,50};
  set<int> second (myints,myints+5);        // pointers used as iterators

  set<int> third (second);                  // a copy of second

  set<int> fourth (second.begin(), second.end());  // iterator ctor.

  set<int,classcomp> fifth;                 // class as Compare

  bool(*fn_pt)(int,int) = fncomp;
  set<int,bool(*)(int,int)> sixth (fn_pt);  // function pointer as Compare

  return 0;
}
```

#### 5.2 Iterator

1. begin / end

```c++
// set::begin/end
#include <iostream>
#include <set>
using namespace std;

int main ()
{
  int myints[] = {75,23,65,42,13};
  set<int> myset (myints,myints+5);

  set<int>::iterator it;

  cout << "myset contains:";
  for ( it=myset.begin() ; it != myset.end(); it++ )
    cout << " " << *it;

  cout << endl;

  return 0;
}
```

2. rbegin

```c++
// set::rbegin/rend
#include <iostream>
#include <set>
using namespace std;

int main ()
{
  int myints[] = {78,21,64,49,17};
  set<int> myset (myints,myints+5);

  set<int>::reverse_iterator rit;

  cout << "myset contains:";
  for ( rit=myset.rbegin() ; rit != myset.rend(); rit++ )
    cout << " " << *rit;

  cout << endl;

  return 0;
}
```

#### 5.3 Modifier

1. insert

```c++
pair<iterator,bool> insert ( const value_type& x );
           iterator insert ( iterator position, const value_type& x );
template <class InputIterator>
      void insert ( InputIterator first, InputIterator last );
```

```c++
// set::insert
#include <iostream>
#include <set>
using namespace std;

int main ()
{
  set<int> myset;
  set<int>::iterator it;
  pair<set<int>::iterator,bool> ret;

  // set some initial values:
  for (int i=1; i<=5; i++) myset.insert(i*10);    // set: 10 20 30 40 50

  ret = myset.insert(20);               // no new element inserted

  if (ret.second==false) it=ret.first;  // "it" now points to element 20

  myset.insert (it,25);                 // max efficiency inserting
  myset.insert (it,24);                 // max efficiency inserting
  myset.insert (it,26);                 // no max efficiency inserting

  int myints[]= {5,10,15};              // 10 already in set, not inserted
  myset.insert (myints,myints+3);

  cout << "myset contains:";
  for (it=myset.begin(); it!=myset.end(); it++)
    cout << " " << *it;
  cout << endl;

  return 0;
}
```

2. erase

```c++
void erase ( iterator position );
size_type erase ( const key_type& x );
     void erase ( iterator first, iterator last );
```

```c++
// erasing from set
#include <iostream>
#include <set>
using namespace std;

int main ()
{
  set<int> myset;
  set<int>::iterator it;

  // insert some values:
  for (int i=1; i<10; i++) myset.insert(i*10);  // 10 20 30 40 50 60 70 80 90

  it=myset.begin();
  it++;                                         // "it" points now to 20

  myset.erase (it);

  myset.erase (40);

  it=myset.find (60);
  myset.erase ( it, myset.end() );

  cout << "myset contains:";
  for (it=myset.begin(); it!=myset.end(); ++it)
    cout << " " << *it;
  cout << endl;

  return 0;
}
```

#### 5.4 Observers

1. key_comp

```c++
key_compare key_comp ( ) const;
```

```c++
// set::key_comp
#include <iostream>
#include <set>
using namespace std;

int main ()
{
  set<int> myset;
  set<int>::key_compare mycomp;
  set<int>::iterator it;
  int i,highest;

  mycomp = myset.key_comp();

  for (i=0; i<=5; i++) myset.insert(i);

  cout << "myset contains:";

  highest=*myset.rbegin();
  it=myset.begin();
  do {
    cout << " " << *it;
  } while ( mycomp(*it++,highest) );

  cout << endl;

  return 0;
}
```

2. value_comp

```c++
value_compare value_comp ( ) const;
```

```c++
// set::value_comp
#include <iostream>
#include <set>
using namespace std;

int main ()
{
  set<int> myset;
  set<int>::value_compare mycomp;
  set<int>::iterator it;
  int i,highest;

  mycomp = myset.value_comp();

  for (i=0; i<=5; i++) myset.insert(i);

  cout << "myset contains:";

  highest=*myset.rbegin();
  it=myset.begin();
  do {
    cout << " " << *it;
  } while ( mycomp(*it++,highest) );

  cout << endl;

  return 0;
}
```

#### 5.5 Operations

1. find

```c++
iterator find ( const key_type& x ) const;
```

```c++
// set::find
#include <iostream>
#include <set>
using namespace std;

int main ()
{
  set<int> myset;
  set<int>::iterator it;

  // set some initial values:
  for (int i=1; i<=5; i++) myset.insert(i*10);    // set: 10 20 30 40 50

  it=myset.find(20);
  myset.erase (it);
  myset.erase (myset.find(40));

  cout << "myset contains:";
  for (it=myset.begin(); it!=myset.end(); it++)
    cout << " " << *it;
  cout << endl;

  return 0;
}
```

2. count

```c++
size_type count ( cont key_type& x ) const;
```

```c++
// set::count
#include <iostream>
#include <set>
using namespace std;

int main ()
{
  set<int> myset;
  int i;

  // set some initial values:
  for (i=1; i<5; i++) myset.insert(i*3);    // set: 3 6 9 12

  for (i=0;i<10; i++)
  {
    cout << i;
    if (myset.count(i)>0)
      cout << " is an element of myset.\n";
    else 
      cout << " is not an element of myset.\n";
  }

  return 0;
}
```

output:
0 is not an element of myset.
1 is not an element of myset.
2 is not an element of myset.
3 is an element of myset.
4 is not an element of myset.
5 is not an element of myset.
6 is an element of myset.
7 is not an element of myset.
8 is not an element of myset.
9 is an element of myset.

3. lower_bound

```c++
iterator lower_bound ( const key_type& x ) const;
```

```c++
// set::lower_bound/upper_bound
#include <iostream>
#include <set>
using namespace std;

int main ()
{
  set<int> myset;
  set<int>::iterator it,itlow,itup;

  for (int i=1; i<10; i++) myset.insert(i*10); // 10 20 30 40 50 60 70 80 90

  itlow=myset.lower_bound (30);                //       ^
  itup=myset.upper_bound (60);                 //                   ^

  myset.erase(itlow,itup);                     // 10 20 70 80 90

  cout << "myset contains:";
  for (it=myset.begin(); it!=myset.end(); it++)
    cout << " " << *it;
  cout << endl;

  return 0;
}
```

4. upper_bound

```c++
iterator upper_bound ( const key_type& x ) const;
```

```c++
// set::lower_bound/upper_bound
#include <iostream>
#include <set>
using namespace std;

int main ()
{
  set<int> myset;
  set<int>::iterator it,itlow,itup;

  for (int i=1; i<10; i++) myset.insert(i*10); // 10 20 30 40 50 60 70 80 90

  itlow=myset.lower_bound (30);                //       ^
  itup=myset.upper_bound (60);                 //                   ^

  myset.erase(itlow,itup);                     // 10 20 70 80 90

  cout << "myset contains:";
  for (it=myset.begin(); it!=myset.end(); it++)
    cout << " " << *it;
  cout << endl;

  return 0;
}
```

### **6 map**

#### 6.1 Member functions

1. constructor

```c++
explicit map ( const Compare& comp = Compare(),
               const Allocator& = Allocator() );
template <class InputIterator>
  map ( InputIterator first, InputIterator last,
        const Compare& comp = Compare(), const Allocator& = Allocator() );
map ( const map<Key,T,Compare,Allocator>& x );
```

```c++
// constructing maps
#include <iostream>
#include <map>
using namespace std;

bool fncomp (char lhs, char rhs) {return lhs<rhs;}

struct classcomp {
  bool operator() (const char& lhs, const char& rhs) const
  {return lhs<rhs;}
};

int main ()
{
  map<char,int> first;

  first['a']=10;
  first['b']=30;
  first['c']=50;
  first['d']=70;

  map<char,int> second (first.begin(),first.end());

  map<char,int> third (second);

  map<char,int,classcomp> fourth;                 // class as Compare

  bool(*fn_pt)(char,char) = fncomp;
  map<char,int,bool(*)(char,char)> fifth (fn_pt); // function pointer as Compare

  return 0;
}
```

#### 6.2 Iterators

1. begin

```c++
iterator begin ();
const_iterator begin () const;
```

```c++
// map::begin/end
#include <iostream>
#include <map>
using namespace std;

int main ()
{
  map<char,int> mymap;
  map<char,int>::iterator it;

  mymap['b'] = 100;
  mymap['a'] = 200;
  mymap['c'] = 300;

  // show content:
  for ( it=mymap.begin() ; it != mymap.end(); it++ )
    cout << (*it).first << " => " << (*it).second << endl;

  return 0;
}
```

2. rbegin

```c++
reverse_iterator rbegin();
const_reverse_iterator rbegin() const;
```

```c++
// map::rbegin/rend
#include <iostream>
#include <map>
using namespace std;

int main ()
{
  map<char,int> mymap;
  map<char,int>::reverse_iterator rit;

  mymap['x'] = 100;
  mymap['y'] = 200;
  mymap['z'] = 300;

  // show content:
  for ( rit=mymap.rbegin() ; rit != mymap.rend(); rit++ )
    cout << rit->first << " => " << rit->second << endl;

  return 0;
}
```

#### 6.3 Modifiers

1. insert

```c++
pair<iterator,bool> insert ( const value_type& x );
           iterator insert ( iterator position, const value_type& x );
template <class InputIterator>
      void insert ( InputIterator first, InputIterator last );
```

```c++
// map::insert
#include <iostream>
#include <map>
using namespace std;

int main ()
{
  map<char,int> mymap;
  map<char,int>::iterator it;
  pair<map<char,int>::iterator,bool> ret;

  // first insert function version (single parameter):
  mymap.insert ( pair<char,int>('a',100) );
  mymap.insert ( pair<char,int>('z',200) );
  ret=mymap.insert (pair<char,int>('z',500) ); 
  if (ret.second==false)
  {
    cout << "element 'z' already existed";
    cout << " with a value of " << ret.first->second << endl;
  }

  // second insert function version (with hint position):
  it=mymap.begin();
  mymap.insert (it, pair<char,int>('b',300));  // max efficiency inserting
  mymap.insert (it, pair<char,int>('c',400));  // no max efficiency inserting

  // third insert function version (range insertion):
  map<char,int> anothermap;
  anothermap.insert(mymap.begin(),mymap.find('c'));

  // showing contents:
  cout << "mymap contains:\n";
  for ( it=mymap.begin() ; it != mymap.end(); it++ )
    cout << (*it).first << " => " << (*it).second << endl;

  cout << "anothermap contains:\n";
  for ( it=anothermap.begin() ; it != anothermap.end(); it++ )
    cout << (*it).first << " => " << (*it).second << endl;

  return 0;
}
```

2. erase

```c++
void erase ( iterator position );
size_type erase ( const key_type& x );
     void erase ( iterator first, iterator last );
```

```c++
// erasing from map
#include <iostream>
#include <map>
using namespace std;

int main ()
{
  map<char,int> mymap;
  map<char,int>::iterator it;

  // insert some values:
  mymap['a']=10;
  mymap['b']=20;
  mymap['c']=30;
  mymap['d']=40;
  mymap['e']=50;
  mymap['f']=60;

  it=mymap.find('b');
  mymap.erase (it);                   // erasing by iterator

  mymap.erase ('c');                  // erasing by key

  it=mymap.find ('e');
  mymap.erase ( it, mymap.end() );    // erasing by range

  // show content:
  for ( it=mymap.begin() ; it != mymap.end(); it++ )
    cout << (*it).first << " => " << (*it).second << endl;

  return 0;
}
```

#### 6.4 Observers

1. key_comp

```c++
key_compare key_comp ( ) const;
```

```c++
// map::key_comp
#include <iostream>
#include <map>
using namespace std;

int main ()
{
  map<char,int> mymap;
  map<char,int>::key_compare mycomp;
  map<char,int>::iterator it;
  char highest;

  mycomp = mymap.key_comp();

  mymap['a']=100;
  mymap['b']=200;
  mymap['c']=300;

  cout << "mymap contains:\n";

  highest=mymap.rbegin()->first;     // key value of last element

  it=mymap.begin();
  do {
    cout << (*it).first << " => " << (*it).second << endl;
  } while ( mycomp((*it++).first, highest) );

  cout << endl;

  return 0;
}
```

2. value_comp

```c++
value_compare value_comp ( ) const;
```

```c++
// map::value_comp
#include <iostream>
#include <map>
using namespace std;

int main ()
{
  map<char,int> mymap;
  map<char,int>::iterator it;
  pair<char,int> highest;

  mymap['x']=1001;
  mymap['y']=2002;
  mymap['z']=3003;

  cout << "mymap contains:\n";

  highest=*mymap.rbegin();          // last element

  it=mymap.begin();
  do {
    cout << (*it).first << " => " << (*it).second << endl;
  } while ( mymap.value_comp()(*it++, highest) );

  return 0;
}
```

#### 6.5 Operations

1. find

```c++
iterator find ( const key_type& x );
const_iterator find ( const key_type& x ) const;
```

```c++
// map::find
#include <iostream>
#include <map>
using namespace std;

int main ()
{
  map<char,int> mymap;
  map<char,int>::iterator it;

  mymap['a']=50;
  mymap['b']=100;
  mymap['c']=150;
  mymap['d']=200;

  it=mymap.find('b');
  mymap.erase (it);
  mymap.erase (mymap.find('d'));

  // print content:
  cout << "elements in mymap:" << endl;
  cout << "a => " << mymap.find('a')->second << endl;
  cout << "c => " << mymap.find('c')->second << endl;

  return 0;
}
```

2. count

```c++
size_type count ( cont key_type& x ) const;
```

```c++
// map::count
#include <iostream>
#include <map>
using namespace std;

int main ()
{
  map<char,int> mymap;
  char c;

  mymap ['a']=101;
  mymap ['c']=202;
  mymap ['f']=303;

  for (c='a'; c<'h'; c++)
  {
    cout << c;
    if (mymap.count(c)>0)
      cout << " is an element of mymap.\n";
    else 
      cout << " is not an element of mymap.\n";
  }

  return 0;
}
```

3. lower_bound

```c++
iterator lower_bound ( const key_type& x );
const_iterator lower_bound ( const key_type& x ) const;
```

```c++
// map::lower_bound/upper_bound
#include <iostream>
#include <map>
using namespace std;

int main ()
{
  map<char,int> mymap;
  map<char,int>::iterator it,itlow,itup;

  mymap['a']=20;
  mymap['b']=40;
  mymap['c']=60;
  mymap['d']=80;
  mymap['e']=100;

  itlow=mymap.lower_bound ('b');  // itlow points to b
  itup=mymap.upper_bound ('d');   // itup points to e (not d!)

  mymap.erase(itlow,itup);        // erases [itlow,itup)

  // print content:
  for ( it=mymap.begin() ; it != mymap.end(); it++ )
    cout << (*it).first << " => " << (*it).second << endl;

  return 0;
}
```

4. upper_bound

```c++
iterator upper_bound ( const key_type& x );
const_iterator upper_bound ( const key_type& x ) const;
```

```c++
// map::lower_bound/upper_bound
#include <iostream>
#include <map>
using namespace std;

int main ()
{
  map<char,int> mymap;
  map<char,int>::iterator it,itlow,itup;

  mymap['a']=20;
  mymap['b']=40;
  mymap['c']=60;
  mymap['d']=80;
  mymap['e']=100;

  itlow=mymap.lower_bound ('b');  // itlow points to b
  itup=mymap.upper_bound ('d');   // itup points to e (not d!)

  mymap.erase(itlow,itup);        // erases [itlow,itup)

  // print content:
  for ( it=mymap.begin() ; it != mymap.end(); it++ )
    cout << (*it).first << " => " << (*it).second << endl;

  return 0;
}
```

5. equal_range

```c++
pair<iterator,iterator>
   equal_range ( const key_type& x );
pair<const_iterator,const_iterator>
   equal_range ( const key_type& x ) const;
```

```c++
// map::equal_elements
#include <iostream>
#include <map>
using namespace std;

int main ()
{
  map<char,int> mymap;
  pair<map<char,int>::iterator,map<char,int>::iterator> ret;

  mymap['a']=10;
  mymap['b']=20;
  mymap['c']=30;

  ret = mymap.equal_range('b');

  cout << "lower bound points to: ";
  cout << ret.first->first << " => " << ret.first->second << endl;

  cout << "upper bound points to: ";
  cout << ret.second->first << " => " << ret.second->second << endl;

  return 0;
}
```

### **7 vector**

#### 7.1 Modifiers

1. assign

```c++
template <class InputIterator>
  void assign ( InputIterator first, InputIterator last );
void assign ( size_type n, const T& u );
```

```c++
// vector assign
#include <iostream>
#include <vector>
using namespace std;

int main ()
{
  vector<int> first;
  vector<int> second;
  vector<int> third;

  first.assign (7,100);             // a repetition 7 times of value 100

  vector<int>::iterator it;
  it=first.begin()+1;

  second.assign (it,first.end()-1); // the 5 central values of first

  int myints[] = {1776,7,4};
  third.assign (myints,myints+3);   // assigning from array.

  cout << "Size of first: " << int (first.size()) << endl;
  cout << "Size of second: " << int (second.size()) << endl;
  cout << "Size of third: " << int (third.size()) << endl;
  return 0;
}
```

2. insert

```c++
iterator insert ( iterator position, const T& x );
    void insert ( iterator position, size_type n, const T& x );
template <class InputIterator>
    void insert ( iterator position, InputIterator first, InputIterator last );
```

```c++
#include <iostream>
#include <vector>
using namespace std;

int main ()
{
  vector<int> myvector (3,100);
  vector<int>::iterator it;

  it = myvector.begin();
  it = myvector.insert ( it , 200 );

  myvector.insert (it,2,300);

  // "it" no longer valid, get a new one:
  it = myvector.begin();

  vector<int> anothervector (2,400);
  myvector.insert (it+2,anothervector.begin(),anothervector.end());

  int myarray [] = { 501,502,503 };
  myvector.insert (myvector.begin(), myarray, myarray+3);

  cout << "myvector contains:";
  for (it=myvector.begin(); it<myvector.end(); it++)
    cout << " " << *it;
  cout << endl;

  return 0;
}
```

3. erase

```c++
iterator erase ( iterator position );
iterator erase ( iterator first, iterator last );
```

```c++
// erasing from vector
#include <iostream>
#include <vector>
using namespace std;

int main ()
{
  unsigned int i;
  vector<unsigned int> myvector;

  // set some values (from 1 to 10)
  for (i=1; i<=10; i++) myvector.push_back(i);
  
  // erase the 6th element
  myvector.erase (myvector.begin()+5);

  // erase the first 3 elements:
  myvector.erase (myvector.begin(),myvector.begin()+3);

  cout << "myvector contains:";
  for (i=0; i<myvector.size(); i++)
    cout << " " << myvector[i];
  cout << endl;

  return 0;
}
```

### **8 list**

#### 8.1 Modifiers

1. insert

```c++
iterator insert ( iterator position, const T& x );
    void insert ( iterator position, size_type n, const T& x );
template <class InputIterator>
    void insert ( iterator position, InputIterator first, InputIterator last );
```

```c++
#include <iostream>
#include <list>
#include <vector>
using namespace std;

int main ()
{
  list<int> mylist;
  list<int>::iterator it;

  // set some initial values:
  for (int i=1; i<=5; i++) mylist.push_back(i); // 1 2 3 4 5

  it = mylist.begin();
  ++it;       // it points now to number 2           ^

  mylist.insert (it,10);                        // 1 10 2 3 4 5

  // "it" still points to number 2                      ^
  mylist.insert (it,2,20);                      // 1 10 20 20 2 3 4 5

  --it;       // it points now to the second 20            ^

  vector<int> myvector (2,30);
  mylist.insert (it,myvector.begin(),myvector.end());
                                                // 1 10 20 30 30 20 2 3 4 5
                                                //               ^
  cout << "mylist contains:";
  for (it=mylist.begin(); it!=mylist.end(); it++)
    cout << " " << *it;
  cout << endl;

  return 0;
}
```

1. erase

```c++
iterator erase ( iterator position );
iterator erase ( iterator first, iterator last );
```

```c++
// erasing from list
#include <iostream>
#include <list>
using namespace std;

int main ()
{
  unsigned int i;
  list<unsigned int> mylist;
  list<unsigned int>::iterator it1,it2;

  // set some values:
  for (i=1; i<10; i++) mylist.push_back(i*10);

                              // 10 20 30 40 50 60 70 80 90
  it1 = it2 = mylist.begin(); // ^^
  advance (it2,6);            // ^                 ^
  ++it1;                      //    ^              ^

  it1 = mylist.erase (it1);   // 10 30 40 50 60 70 80 90
                              //    ^           ^

  it2 = mylist.erase (it2);   // 10 30 40 50 60 80 90
                              //    ^           ^

  ++it1;                      //       ^        ^
  --it2;                      //       ^     ^

  mylist.erase (it1,it2);     // 10 30 60 80 90
                              //        ^

  cout << "mylist contains:";
  for (it1=mylist.begin(); it1!=mylist.end(); ++it1)
    cout << " " << *it1;
  cout << endl;

  return 0;
}
```

#### 8.2 Operations

1. splice

```c++
void splice ( iterator position, list<T,Allocator>& x );
void splice ( iterator position, list<T,Allocator>& x, iterator i );
void splice ( iterator position, list<T,Allocator>& x, iterator first, iterator last );
```

```c++
// splicing lists
#include <iostream>
#include <list>
using namespace std;

int main ()
{
  list<int> mylist1, mylist2;
  list<int>::iterator it;

  // set some initial values:
  for (int i=1; i<=4; i++)
     mylist1.push_back(i);      // mylist1: 1 2 3 4

  for (int i=1; i<=3; i++)
     mylist2.push_back(i*10);   // mylist2: 10 20 30

  it = mylist1.begin();
  ++it;                         // points to 2

  mylist1.splice (it, mylist2); // mylist1: 1 10 20 30 2 3 4
                                // mylist2 (empty)
                                // "it" still points to 2 (the 5th element)

  mylist2.splice (mylist2.begin(),mylist1, it);
                                // mylist1: 1 10 20 30 3 4
                                // mylist2: 2
                                // "it" is now invalid.
  it = mylist1.begin();
  advance(it,3);                // "it" points now to 30

  mylist1.splice ( mylist1.begin(), mylist1, it, mylist1.end());
                                // mylist1: 30 3 4 1 10 20

  cout << "mylist1 contains:";
  for (it=mylist1.begin(); it!=mylist1.end(); it++)
    cout << " " << *it;

  cout << "\nmylist2 contains:";
  for (it=mylist2.begin(); it!=mylist2.end(); it++)
    cout << " " << *it;
  cout << endl;

  return 0;
}
```

2. remove

```c++
void remove ( const T& value );
```

```c++
#include <iostream>
#include <list>
using namespace std;

int main ()
{
  int myints[]= {17,89,7,14};
  list<int> mylist (myints,myints+4);

  mylist.remove(89);

  cout << "mylist contains:";
  for (list<int>::iterator it=mylist.begin(); it!=mylist.end(); ++it)
    cout << " " << *it;
  cout << endl;

  return 0;
}
```

3. remove_if

```c++
template <class Predicate>
  void remove_if ( Predicate pred );
```

```c++
// list::remove_if
#include <iostream>
#include <list>
using namespace std;

// a predicate implemented as a function:
bool single_digit (const int& value) { return (value<10); }

// a predicate implemented as a class:
class is_odd
{
public:
  bool operator() (const int& value) {return (value%2)==1; }
};

int main ()
{
  int myints[]= {15,36,7,17,20,39,4,1};
  list<int> mylist (myints,myints+8);   // 15 36 7 17 20 39 4 1

  mylist.remove_if (single_digit);      // 15 36 17 20 39

  mylist.remove_if (is_odd());          // 36 20

  cout << "mylist contains:";
  for (list<int>::iterator it=mylist.begin(); it!=mylist.end(); ++it)
    cout << " " << *it;
  cout << endl;

  return 0;
}
```

4. unique // 需要先排序

```c++
void unique ( );
template <class BinaryPredicate>
  void unique ( BinaryPredicate binary_pred );
```

```c++
// list::unique
#include <iostream>
#include <cmath>
#include <list>
using namespace std;

// a binary predicate implemented as a function:
bool same_integral_part (double first, double second)
{ return ( int(first)==int(second) ); }

// a binary predicate implemented as a class:
class is_near
{
public:
  bool operator() (double first, double second)
  { return (fabs(first-second)<5.0); }
};

int main ()
{
  double mydoubles[]={ 12.15,  2.72, 73.0,  12.77,  3.14,
                       12.77, 73.35, 72.25, 15.3,  72.25 };
  list<double> mylist (mydoubles,mydoubles+10);
  
  mylist.sort();             //  2.72,  3.14, 12.15, 12.77, 12.77,
                             // 15.3,  72.25, 72.25, 73.0,  73.35

  mylist.unique();           //  2.72,  3.14, 12.15, 12.77
                             // 15.3,  72.25, 73.0,  73.35

  mylist.unique (same_integral_part);  //  2.72,  3.14, 12.15
                                       // 15.3,  72.25, 73.0

  mylist.unique (is_near());           //  2.72, 12.15, 72.25

  cout << "mylist contains:";
  for (list<double>::iterator it=mylist.begin(); it!=mylist.end(); ++it)
    cout << " " << *it;
  cout << endl;

  return 0;
}
```

5. merge

```c++
void merge ( list<T,Allocator>& x );
template <class Compare>
  void merge ( list<T,Allocator>& x, Compare comp );
```

```c++
// list::merge
#include <iostream>
#include <list>
using namespace std;

// this compares equal two doubles if
//  their interger equivalents are equal
bool mycomparison (double first, double second)
{ return ( int(first)<int(second) ); }

int main ()
{
  list<double> first, second;

  first.push_back (3.1);
  first.push_back (2.2);
  first.push_back (2.9);

  second.push_back (3.7);
  second.push_back (7.1);
  second.push_back (1.4);

  first.sort();
  second.sort();

  first.merge(second);

  second.push_back (2.1);

  first.merge(second,mycomparison);

  cout << "first contains:";
  for (list<double>::iterator it=first.begin(); it!=first.end(); ++it)
    cout << " " << *it;
  cout << endl;

  return 0;
}
```

7. sort

```c++
void sort ( );
template <class Compare>
  void sort ( Compare comp );
```

```c++
// list::sort
#include <iostream>
#include <list>
#include <string>
#include <cctype>
using namespace std;

// comparison, not case sensitive.
bool compare_nocase (string first, string second)
{
  unsigned int i=0;
  while ( (i<first.length()) && (i<second.length()) )
  {
    if (tolower(first[i])<tolower(second[i])) return true;
    ++i;
  }
  if (first.length()<second.length()) return true;
  else return false;
}

int main ()
{
  list<string> mylist;
  list<string>::iterator it;
  mylist.push_back ("one");
  mylist.push_back ("two");
  mylist.push_back ("Three");

  mylist.sort();

  cout << "mylist contains:";
  for (it=mylist.begin(); it!=mylist.end(); ++it)
    cout << " " << *it;
  cout << endl;

  mylist.sort(compare_nocase);

  cout << "mylist contains:";
  for (it=mylist.begin(); it!=mylist.end(); ++it)
    cout << " " << *it;
  cout << endl;

  return 0;
}
```

8. reverse

```c++
// reversing vector
#include <iostream>
#include <list>
using namespace std;

int main ()
{
  list<int> mylist;
  list<int>::iterator it;

  for (int i=1; i<10; i++) mylist.push_back(i);

  mylist.reverse();

  cout << "mylist contains:";
  for (it=mylist.begin(); it!=mylist.end(); ++it)
    cout << " " << *it;

  cout << endl;

  return 0;
}
```

### **9 cstdio**

1. sprintf & sscanf

```c++
/// sprintf 将数据存储到字符串中
char* str = "sadada ";

char s[100];
int d = 12;
float fff = 20.8;
sprintf(s, "%s %d %.2f", str, d, fff); // 把其他格式转成C-String类型

cout << s << endl; // sadada  12 20.80

int data = 100;
char s_data[100];
sprintf(s_data, "0x%X", data); // 16进制
cout << s_data << endl;

sprintf(s_data, "0%o", data); // 8进制
cout << s_data << endl;

/// sscanf 从字符串中提取数据
char* ss = "12 12.8";
int dd;
float ff;
sscanf(ss, "%d %f", &dd, &ff);
cout << dd << endl;
cout << ff << endl;

const char *szhi = "http://www.baidu.com:1234";
char protocol[32] = { 0 };
char host[128] = { 0 };
char port[8] = { 0 };
sscanf(szhi, "%[^:]://%[^:]:%[1-9]", protocol, host, port);

printf("protocol: %s\n", protocol);
printf("host: %s\n", host);
printf("port: %s\n", port);
```

### **10 cstring**

#### 10.1 Copying

1. memcpy

```c++
void * memcpy ( void * destination, const void * source, size_t num );
```

```c++
#include <stdio.h>
#include <string.h>

int main ()
{
  char str1[]="Sample string";
  char str2[40];
  char str3[40];
  memcpy (str2,str1,strlen(str1)+1);
  memcpy (str3,"copy successful",16);
  printf ("str1: %s\nstr2: %s\nstr3: %s\n",str1,str2,str3);
  return 0;
}
```

2. strcpy

```c++
char * strcpy ( char * destination, const char * source );
```

```c++
#include <stdio.h>
#include <string.h>

int main ()
{
  char str1[]="Sample string";
  char str2[40];
  char str3[40];
  strcpy (str2,str1);
  strcpy (str3,"copy successful");
  printf ("str1: %s\nstr2: %s\nstr3: %s\n",str1,str2,str3);
  return 0;
}
```

3. strncpy

```c++
char * strncpy ( char * destination, const char * source, size_t num );
```

```c++
#include <stdio.h>
#include <string.h>

int main ()
{
  char str1[]= "To be or not to be";
  char str2[6];
  strncpy (str2,str1,5);
  str2[5]='\0';
  puts (str2);
  return 0;
}
```

#### 10.2 Concatenation

1. strcat

```c++
char * strcat ( char * destination, const char * source );
```

```c++
#include <stdio.h>
#include <string.h>

int main ()
{
  char str[80];
  strcpy (str,"these ");
  strcat (str,"strings ");
  strcat (str,"are ");
  strcat (str,"concatenated.");
  puts (str);
  return 0;
}
```

2. strncat

```c++
char * strncat ( char * destination, char * source, size_t num );
```

```c++
/* strncat example */
#include <stdio.h>
#include <string.h>

int main ()
{
  char str1[20];
  char str2[20];
  strcpy (str1,"To be ");
  strcpy (str2,"or not to be");
  strncat (str1, str2, 6);
  puts (str1);
  return 0;
}
```

#### 10.3 Comparison

1. strcmp

```c++
int strcmp ( const char * str1, const char * str2 );
```

> Returns an integral value indicating the relationship between the strings:
> A zero value indicates that both strings are equal.
> A value greater than zero indicates that the first character that does not match has a greater value in str1 than in str2; And a value less than zero indicates the opposite.

```c++
#include <stdio.h>
#include <string.h>

int main ()
{
  char szKey[] = "apple";
  char szInput[80];
  do {
     printf ("Guess my favourite fruit? ");
     gets (szInput);
  } while (strcmp (szKey,szInput) != 0);
  puts ("Correct answer!");
  return 0;
}
```

#### 10.4 Searching

1. strchr

```c++
const char * strchr ( const char * str, int character );
      char * strchr (       char * str, int character );
```

```c++
#include <stdio.h>
#include <string.h>

int main ()
{
  char str[] = "This is a sample string";
  char * pch;
  printf ("Looking for the 's' character in \"%s\"...\n",str);
  pch=strchr(str,'s');
  while (pch!=NULL)
  {
    printf ("found at %d\n",pch-str+1);
    pch=strchr(pch+1,'s');
  }
  return 0;
}
```

2. strcspn /// 出现指定的

```c++
size_t strcspn ( const char * str1, const char * str2 );
```

```c++
#include <stdio.h>
#include <string.h>

int main ()
{
  char str[] = "fcba73";
  char keys[] = "1234567890";
  int i;
  i = strcspn (str,keys);
  printf ("The first number in str is at position %d.\n",i+1);
  return 0;
}
```

3. strspn /// 未出现指定的

```c++
size_t strspn ( const char * str1, const char * str2 );
```

```c++
#include <stdio.h>
#include <string.h>

int main ()
{
  int i;
  char strtext[] = "129th";
  char cset[] = "1234567890";

  i = strspn (strtext,cset);
  printf ("The length of initial number is %d.\n",i);
  return 0;
}
```

4. strstr

```c++
const char * strstr ( const char * str1, const char * str2 );
      char * strstr (       char * str1, const char * str2 );
```

```c++
#include <stdio.h>
#include <string.h>

int main ()
{
  char str[] ="This is a simple string";
  char * pch;
  pch = strstr (str,"simple");
  strncpy (pch,"sample",5);
  puts (str);
  return 0;
}
```

5. strtok

```c++
// strings and c-strings
#include <iostream>
#include <cstring>
#include <string>
using namespace std;

int main ()
{
  char * cstr, *p;

  string str ("Please split this phrase into tokens");

  cstr = new char [str.size()+1];
  strcpy (cstr, str.c_str());

  // cstr now contains a c-string copy of str

  p=strtok (cstr," ");
  while (p!=NULL)
  {
    cout << p << endl;;
    p=strtok(NULL," ");
  }

  delete[] cstr;  
  return 0;
}
```