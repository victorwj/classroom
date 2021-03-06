# Interview

This section contains my preparation for CS technical interviews, designed
to be a quick reference.

## Data Structures

- Summary

Data Structure | O(1) | O(logn) | O(n) 
--- | --- | --- | ---
Vector | `[n], push_back*, pop_back, size, empty` |  | `insert, erase`
List | `front, push_front, pop_front, back, push_back, pop_back, insert, size, empty` | | `insert`
U-Map | `[n]*, [n] = a*, erase*, find*, size, empty` | | 
Stack | `top, push, pop, size, empty` | | 
Queue | `front, push, pop, back, size, empty` | |
Deque | `[n], front, push_front*, pop_front, back, push_back*, pop_back, size, empty` | | `insert, erase`
Priority Queue | `top, size, empty` | `push, pop` | 


\*Amortized
- Vector

```c++
#include <vector>
vector<int> v = {2, 4, 6};

// O(1)
v[0];
v.pop_back();
v.size();
v.empty();

// Amortized O(1)
v.push_back(8); // Might need to re-allocate

// O(n)
v.insert(v.begin() + 1, 3); // Insert BEFORE the iterator
v.erase(v.begin() + v.size() - 1); // Erase THIS element
v.clear();
```

- Doubly Linked List

```c++
#include <list>
list<int> l = {2, 4, 6};

// O(1)
l.front();
l.push_front(1);
l.pop_front();
l.back();
l.push_back(5);
l.pop_back();
l.insert(l.begin(), 0);
l.size();
l.empty();

// O(n)
l.insert(next(l.begin(), 2), 3); // Advance the pointer twice
l.insert(next(l.begin()), 1); // Advance the pointer once
```

- Hash table

```c++
#include <unordered_map> 
unordered_map<string, int> h = {
    {"b", 2},
    {"d", 4},
    {"f", 6}
};

// O(1)
h.size();
h.empty();

// Amortized O(1)
h["b"]; // Worst case O(n) due to hash collisions
h["a"] = 0; // Worst case O(n) due to potential re-hashing
h["c"]; // Will insert a default value of 0
h["z"]++; // Automatically create and increment
h.erase("a");
if (h.find("a") != h.end()) {
    cout << "found";
}
```

- Stack

```c++
#include <stack>
stack<int> s;

// There are no initializer lists for stack
for (const auto& x : {6, 4, 2}) {
    s.push(x);
}

// O(1)
s.top();
s.pop(); // pop_back on underlying container
s.push(0); // push_back on underlying container
s.size();
s.empty();
```

- Queue

```c++
#include <queue>
queue<int> q;

// There are no initializer lists for queue
for (const auto& x : {6, 4, 2}) {
    q.push(x);
}

// O(1)
q.front();
q.back();
q.push(0); // push_back on underlying container
q.pop(); // pop_front on underlying container
q.size();
q.empty();
```

- Deque

```c++
// Deques are like vectors, but also with efficient insertion at beginning
// Allows random access as well
// Tradeoff: non contiguous storage
#include <deque>
deque<int> d = {2, 4, 6};

// O(1)
d[1];
d.front();
d.back();
d.pop_front();
d.pop_back();
d.size();
d.empty();

// Amortized O(1)
d.push_front(1);
d.push_back(7);

// O(n)
d.insert
d.insert(d.begin() + 1, 3);
```

- Priority Queues

```c++
#include <queue>

// PQ is the abstract data type, heap is the implementation
// Standard order is greatest to least (max heap)
priority_queue<int> p;

// Least to greatest (min heap)
priority_queue<int, vector<int>, greater<int>> p_min;

// O(1)
p.top();
p.empty();

// O(logn)
p.push(2);
p.pop();
```

## C++

- Range based / Iterator for loop

```c++
// Read-only:
for (auto x : v) // By value, makes a copy
for (const auto& x : v) // More efficient, const reference

// Write:
for (auto& x : v)

// Iterator
for (auto it = v.begin(); it < v.end(); it++) {
    cout << *it << endl;
    *it += 1
}
```

- Files

```c++
ifstream if;
if.open("file.txt");
if >> var;
ofstream of;
of.open("file.txt");
of << var;
```

- Ternary Operator

```c++
// conditional ? (if true) : (if false)

return x == 0 ? "true" : "false"
```

## Algorithms

- BFS

```c++
void bfs(Node * root) {
    queue<Node *> q;
    q.push(root);
    while (!q.empty()) {
        Node * cur = q.front();
        cout << cur->val << " ";
        if (cur->left) q.push(cur->left);
        if (cur->right) q.push(cur->right);
        q.pop();
    }
    cout << endl;
    return;
}
```

- Depth First Search

```c++
void inorder(Node* root) {
    if (!root) {
        return;
    }
    inorder(root->left);
    cout << root->val << " ";
    inorder(root->right);
    return;
}

void preorder(Node* root) {
    if (!root) {
        return;
    }
    cout << root->val << " ";
    preorder(root->left);
    preorder(root->right);
    return;
}

void postorder(Node* root) {
    if (!root) {
        return;
    }
    postorder(root->left);
    postorder(root->right);
    cout << root->val << " ";
    return;
}
```

- Binary Search

```c++
// v must be sorted
int bsearch(const vector<int>& v, int val) {
    int left = 0;
    int right = v.size() - 1;
    while (left <= right) {
        int idx = floor((left + right) / 2.0);
        int cur = v[idx];
        if (cur == val) return idx;
        if (cur < val) left = idx + 1;
        if (cur > val) right = idx - 1;
    }
    return -1;
}
```

- Binary Search Tree

    - Left child must be less than parent's value.

    - Use inorder traversal to get a sorted array.

    - Insertion is very similar to search.

    - Deletion involves replacing node with in-order successor (left-most child of right child). Then delete the in-order successor, which is easy since it has 0 children or 1 right child. 

- Sorts

```c++
void insertion(vector<int>& v) {
    for (size_t i = 1; i < v.size(); ++i) {
        for (size_t j = i; j > 0; --j) {
            if (v[j] < v[j - 1]) swap(v[j], v[j - 1]);
        }
    }

}

void selection(vector<int>& v) {
    size_t cur_idx = 0;
    while (cur_idx < v.size()) {
        size_t min_idx = cur_idx;
        for (size_t j = cur_idx + 1; j < v.size(); ++j) {
            if (v[min_idx] > v[j]) {
                min_idx = j;
            }
        }
        if (min_idx != cur_idx) swap(v[cur_idx], v[min_idx]);
        cur_idx++;
    }
}

void bubble(vector<int>& v) {
    bool swapped = true;
    while (swapped) {
        swapped = false;
        for (size_t i = 0; i < v.size() - 1; ++i) {
            if (v[i] > v[i + 1]) {
                swap(v[i], v[i + 1]);
                swapped = true;
            }
        }
    }
}
```
