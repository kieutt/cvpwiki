# Heap Sort (Sắp xếp vun đống)

## Giới thiệu

Heap Sort là một thuật toán sắp xếp dựa trên cấu trúc dữ liệu **Heap** (đống). Thuật toán được phát minh bởi J. W. J. Williams năm 1964 và là một thuật toán sắp xếp so sánh hiệu quả.

Heap Sort hoạt động bằng cách xây dựng một **Max Heap** (hoặc Min Heap) từ mảng, sau đó lần lượt lấy phần tử lớn nhất (gốc heap) ra và đặt vào cuối mảng.

## Độ phức tạp

- **Thời gian:**
  - Trung bình: O(n log n)
  - Tốt nhất: O(n log n)
  - Xấu nhất: O(n log n)
- **Không gian:** O(1)
- **Tính ổn định:** Không ổn định (unstable)
- **In-place:** Có

## Kiến thức cần có: Binary Heap

**Binary Heap** là cây nhị phân hoàn chỉnh thỏa mãn tính chất heap:
- **Max Heap:** Giá trị mỗi node ≥ giá trị các node con
- **Min Heap:** Giá trị mỗi node ≤ giá trị các node con

**Biểu diễn trong mảng:**
```
Với node i (đánh số từ 0):
- Node cha: (i - 1) / 2
- Node con trái: 2 * i + 1
- Node con phải: 2 * i + 2
```

## Ý tưởng thuật toán

1. **Xây dựng Max Heap:** Biến đổi mảng thành Max Heap
2. **Lặp n lần:**
   - Swap phần tử đầu tiên (max) với phần tử cuối
   - Giảm kích thước heap đi 1
   - Heapify lại từ gốc để duy trì tính chất heap

## Cài đặt

### C++ - Phiên bản cơ bản

```cpp
#include <bits/stdc++.h>
using namespace std;

// Hàm heapify một cây con với gốc i
// n là kích thước của heap
void heapify(vector<int>& arr, int n, int i) {
    int largest = i;        // Khởi tạo largest là gốc
    int left = 2 * i + 1;   // Node con trái
    int right = 2 * i + 2;  // Node con phải

    // Nếu con trái lớn hơn gốc
    if (left < n && arr[left] > arr[largest])
        largest = left;

    // Nếu con phải lớn hơn largest hiện tại
    if (right < n && arr[right] > arr[largest])
        largest = right;

    // Nếu largest không phải là gốc
    if (largest != i) {
        swap(arr[i], arr[largest]);

        // Đệ quy heapify cây con bị ảnh hưởng
        heapify(arr, n, largest);
    }
}

void heapSort(vector<int>& arr) {
    int n = arr.size();

    // Xây dựng Max Heap
    // Bắt đầu từ node cha cuối cùng đến gốc
    for (int i = n / 2 - 1; i >= 0; i--)
        heapify(arr, n, i);

    // Lần lượt lấy phần tử từ heap
    for (int i = n - 1; i > 0; i--) {
        // Chuyển gốc (max) về cuối
        swap(arr[0], arr[i]);

        // Heapify lại heap đã giảm kích thước
        heapify(arr, i, 0);
    }
}

int main() {
    vector<int> arr = {12, 11, 13, 5, 6, 7};

    heapSort(arr);

    cout << "Mang da sap xep: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### C++ - Phiên bản iterative (không đệ quy)

```cpp
#include <bits/stdc++.h>
using namespace std;

void heapifyIterative(vector<int>& arr, int n, int i) {
    while (true) {
        int largest = i;
        int left = 2 * i + 1;
        int right = 2 * i + 2;

        if (left < n && arr[left] > arr[largest])
            largest = left;

        if (right < n && arr[right] > arr[largest])
            largest = right;

        if (largest == i)
            break;

        swap(arr[i], arr[largest]);
        i = largest;
    }
}

void heapSort(vector<int>& arr) {
    int n = arr.size();

    // Xây dựng Max Heap
    for (int i = n / 2 - 1; i >= 0; i--)
        heapifyIterative(arr, n, i);

    // Sắp xếp
    for (int i = n - 1; i > 0; i--) {
        swap(arr[0], arr[i]);
        heapifyIterative(arr, i, 0);
    }
}

int main() {
    vector<int> arr = {12, 11, 13, 5, 6, 7};
    heapSort(arr);

    cout << "Mang da sap xep: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### C++ - Sử dụng STL Priority Queue

```cpp
#include <bits/stdc++.h>
using namespace std;

void heapSort(vector<int>& arr) {
    // Min heap - để lấy phần tử nhỏ nhất trước
    priority_queue<int, vector<int>, greater<int>> pq;

    // Thêm tất cả phần tử vào heap
    for (int x : arr) {
        pq.push(x);
    }

    // Lấy từng phần tử ra theo thứ tự tăng dần
    for (int i = 0; i < arr.size(); i++) {
        arr[i] = pq.top();
        pq.pop();
    }
}

int main() {
    vector<int> arr = {12, 11, 13, 5, 6, 7};
    heapSort(arr);

    cout << "Mang da sap xep: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### Python

```python
def heapify(arr, n, i):
    largest = i
    left = 2 * i + 1
    right = 2 * i + 2

    if left < n and arr[left] > arr[largest]:
        largest = left

    if right < n and arr[right] > arr[largest]:
        largest = right

    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]
        heapify(arr, n, largest)

def heap_sort(arr):
    n = len(arr)

    # Xây dựng Max Heap
    for i in range(n // 2 - 1, -1, -1):
        heapify(arr, n, i)

    # Lần lượt lấy phần tử từ heap
    for i in range(n - 1, 0, -1):
        arr[0], arr[i] = arr[i], arr[0]
        heapify(arr, i, 0)

# Sử dụng
arr = [12, 11, 13, 5, 6, 7]
heap_sort(arr)
print("Mảng đã sắp xếp:", arr)
```

## Ví dụ minh họa

Sắp xếp mảng: `[4, 10, 3, 5, 1]`

**Bước 1: Xây dựng Max Heap**
```
Mảng ban đầu: [4, 10, 3, 5, 1]

       4
      / \
     10  3
    / \
   5   1

Heapify từ node cuối có con (i=1):
       4
      / \
     10  3
    / \
   5   1

Heapify node gốc (i=0):
       10
      / \
     5   3
    / \
   4   1
```

**Bước 2: Sắp xếp**
```
Lần 1: Swap 10 và 1, heapify
[1, 5, 3, 4, | 10]
       5
      / \
     4   3
    /
   1

Lần 2: Swap 5 và 1, heapify
[1, 4, 3, | 5, 10]
       4
      / \
     1   3

Lần 3: Swap 4 và 3, heapify
[3, 1, | 4, 5, 10]
       3
      /
     1

Lần 4: Swap 3 và 1
[1, | 3, 4, 5, 10]

Kết quả: [1, 3, 4, 5, 10]
```

## Phân tích chi tiết

### Xây dựng Heap: O(n)

Nhiều người nghĩ xây dựng heap mất O(n log n), nhưng thực tế là **O(n)**:

- Số node ở độ cao h: ≈ n / 2^(h+1)
- Chi phí heapify node độ cao h: O(h)
- Tổng: Σ(n/2^(h+1) * h) = O(n)

### Sắp xếp: O(n log n)

- n lần lấy phần tử ra
- Mỗi lần heapify: O(log n)
- Tổng: O(n log n)

## Tối ưu hóa

### 1. Bottom-up Heapify (Floyd's Algorithm)

```cpp
void buildHeap(vector<int>& arr) {
    int n = arr.size();

    // Bắt đầu từ node cha cuối cùng
    for (int i = n / 2 - 1; i >= 0; i--) {
        // Sift down không dùng đệ quy
        int current = i;
        while (true) {
            int largest = current;
            int left = 2 * current + 1;
            int right = 2 * current + 2;

            if (left < n && arr[left] > arr[largest])
                largest = left;
            if (right < n && arr[right] > arr[largest])
                largest = right;

            if (largest == current)
                break;

            swap(arr[current], arr[largest]);
            current = largest;
        }
    }
}
```

### 2. Ternary Heap (Heap bậc 3)

```cpp
void ternaryHeapify(vector<int>& arr, int n, int i) {
    int largest = i;

    // 3 node con
    int child1 = 3 * i + 1;
    int child2 = 3 * i + 2;
    int child3 = 3 * i + 3;

    if (child1 < n && arr[child1] > arr[largest])
        largest = child1;
    if (child2 < n && arr[child2] > arr[largest])
        largest = child2;
    if (child3 < n && arr[child3] > arr[largest])
        largest = child3;

    if (largest != i) {
        swap(arr[i], arr[largest]);
        ternaryHeapify(arr, n, largest);
    }
}
```

## Ưu điểm

1. **Độ phức tạp ổn định:** Luôn O(n log n) trong mọi trường hợp
2. **In-place:** Chỉ cần O(1) bộ nhớ phụ
3. **Không đệ quy:** Có thể cài đặt iterative, tránh stack overflow
4. **Deterministic:** Không phụ thuộc vào dữ liệu đầu vào

## Nhược điểm

1. **Không ổn định:** Thứ tự tương đối của các phần tử bằng nhau có thể thay đổi
2. **Không cache-friendly:** Truy cập bộ nhớ không tuần tự
3. **Chậm hơn trong thực tế:** Thường chậm hơn Quick Sort và Merge Sort
4. **Hằng số ẩn lớn:** Nhiều phép so sánh và swap hơn

## So sánh với các thuật toán khác

| Thuật toán | Trung bình | Worst Case | Stable | Space |
|------------|-----------|-----------|--------|-------|
| Heap Sort | O(n log n) | O(n log n) | Không | O(1) |
| Quick Sort | O(n log n) | O(n²) | Không | O(log n) |
| Merge Sort | O(n log n) | O(n log n) | Có | O(n) |

## Khi nào sử dụng Heap Sort?

✅ **Nên dùng khi:**
- Cần đảm bảo O(n log n) trong mọi trường hợp
- Bộ nhớ rất hạn chế (chỉ cần O(1))
- Cần tránh worst case O(n²) của Quick Sort
- Hệ thống embedded với stack nhỏ

❌ **Không nên dùng khi:**
- Cần thuật toán ổn định (dùng Merge Sort)
- Cần tốc độ tốt nhất trong thực tế (dùng Quick Sort)
- Mảng nhỏ (dùng Insertion Sort)

## Ứng dụng của Heap

### 1. Priority Queue

```cpp
class PriorityQueue {
private:
    vector<int> heap;

    void heapifyUp(int i) {
        while (i > 0) {
            int parent = (i - 1) / 2;
            if (heap[i] > heap[parent]) {
                swap(heap[i], heap[parent]);
                i = parent;
            } else {
                break;
            }
        }
    }

    void heapifyDown(int i) {
        int n = heap.size();
        while (true) {
            int largest = i;
            int left = 2 * i + 1;
            int right = 2 * i + 2;

            if (left < n && heap[left] > heap[largest])
                largest = left;
            if (right < n && heap[right] > heap[largest])
                largest = right;

            if (largest == i)
                break;

            swap(heap[i], heap[largest]);
            i = largest;
        }
    }

public:
    void push(int val) {
        heap.push_back(val);
        heapifyUp(heap.size() - 1);
    }

    int top() {
        return heap[0];
    }

    void pop() {
        heap[0] = heap.back();
        heap.pop_back();
        heapifyDown(0);
    }

    bool empty() {
        return heap.empty();
    }
};
```

### 2. Top K Elements

```cpp
vector<int> topKElements(vector<int>& arr, int k) {
    // Min heap chứa k phần tử lớn nhất
    priority_queue<int, vector<int>, greater<int>> minHeap;

    for (int x : arr) {
        minHeap.push(x);
        if (minHeap.size() > k) {
            minHeap.pop();
        }
    }

    vector<int> result;
    while (!minHeap.empty()) {
        result.push_back(minHeap.top());
        minHeap.pop();
    }

    return result;
}
```

### 3. Merge K Sorted Arrays

```cpp
vector<int> mergeKSorted(vector<vector<int>>& arrays) {
    priority_queue<tuple<int, int, int>, vector<tuple<int, int, int>>, greater<>> minHeap;

    // Thêm phần tử đầu tiên của mỗi mảng
    for (int i = 0; i < arrays.size(); i++) {
        if (!arrays[i].empty()) {
            minHeap.push({arrays[i][0], i, 0});
        }
    }

    vector<int> result;
    while (!minHeap.empty()) {
        auto [val, arrIdx, elemIdx] = minHeap.top();
        minHeap.pop();

        result.push_back(val);

        if (elemIdx + 1 < arrays[arrIdx].size()) {
            minHeap.push({arrays[arrIdx][elemIdx + 1], arrIdx, elemIdx + 1});
        }
    }

    return result;
}
```

### 4. Median of Stream

```cpp
class MedianFinder {
private:
    priority_queue<int> maxHeap;  // Nửa nhỏ
    priority_queue<int, vector<int>, greater<int>> minHeap;  // Nửa lớn

public:
    void addNum(int num) {
        maxHeap.push(num);

        // Cân bằng
        minHeap.push(maxHeap.top());
        maxHeap.pop();

        if (maxHeap.size() < minHeap.size()) {
            maxHeap.push(minHeap.top());
            minHeap.pop();
        }
    }

    double findMedian() {
        if (maxHeap.size() > minHeap.size()) {
            return maxHeap.top();
        }
        return (maxHeap.top() + minHeap.top()) / 2.0;
    }
};
```

## Bài tập áp dụng

### Cơ bản
1. Cài đặt Heap Sort và test với các trường hợp khác nhau
2. Cài đặt Priority Queue từ đầu
3. Tìm k phần tử lớn nhất trong mảng

### Nâng cao
1. **Kth Largest Element:** Tìm phần tử lớn thứ k
2. **Merge K Sorted Lists:** Trộn k danh sách đã sắp xếp
3. **Running Median:** Tìm median của dòng dữ liệu
4. **Top K Frequent Elements:** Tìm k phần tử xuất hiện nhiều nhất

### Bài tập trên OJ
- CSES - Distinct Values Queries
- LeetCode 215 - Kth Largest Element in an Array
- LeetCode 295 - Find Median from Data Stream
- LeetCode 347 - Top K Frequent Elements
- SPOJ - HEAP: Heap Operations

## Các biến thể của Heap

### 1. Min Heap
Sắp xếp giảm dần bằng Min Heap:
```cpp
void minHeapify(vector<int>& arr, int n, int i) {
    int smallest = i;
    int left = 2 * i + 1;
    int right = 2 * i + 2;

    if (left < n && arr[left] < arr[smallest])
        smallest = left;
    if (right < n && arr[right] < arr[smallest])
        smallest = right;

    if (smallest != i) {
        swap(arr[i], arr[smallest]);
        minHeapify(arr, n, smallest);
    }
}
```

### 2. D-ary Heap
Heap với d con thay vì 2 con.

### 3. Fibonacci Heap
Hiệu quả hơn cho các thao tác decrease-key.

## Tài liệu tham khảo

- [VNOI Wiki - Heap](https://vnoi.info/wiki/algo/data-structures/heap.md)
- Introduction to Algorithms (CLRS) - Chapter 6
- [Visualgo - Heap Visualization](https://visualgo.net/en/heap)

## Mẹo trong lập trình thi đấu

1. **Sử dụng STL:** `priority_queue` đã được tối ưu rất tốt
   ```cpp
   priority_queue<int> maxHeap;
   priority_queue<int, vector<int>, greater<int>> minHeap;
   ```

2. **Top K problems:** Luôn nghĩ đến heap khi gặp bài toán tìm k phần tử lớn/nhỏ nhất

3. **Heap vs Sort:** Nếu chỉ cần k phần tử, dùng heap O(n log k) tốt hơn sort O(n log n)

4. **Custom comparator:** Có thể tùy chỉnh hàm so sánh cho priority_queue
   ```cpp
   auto cmp = [](const pair<int,int>& a, const pair<int,int>& b) {
       return a.first > b.first;
   };
   priority_queue<pair<int,int>, vector<pair<int,int>>, decltype(cmp)> pq(cmp);
   ```
