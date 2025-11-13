# Insertion Sort (Sắp xếp chèn)

## Giới thiệu

Insertion Sort là một trong những thuật toán sắp xếp đơn giản nhất, hoạt động tương tự như cách chúng ta sắp xếp bài khi chơi bài. Thuật toán xây dựng mảng đã sắp xếp từng phần tử một, bằng cách chèn mỗi phần tử vào vị trí đúng của nó trong phần đã sắp xếp.

Mặc dù có độ phức tạp O(n²), Insertion Sort rất hiệu quả cho:
- Mảng nhỏ (< 10-50 phần tử)
- Mảng đã gần sắp xếp
- Online algorithm (có thể sắp xếp trong khi nhận dữ liệu)

## Độ phức tạp

- **Thời gian:**
  - Trung bình: O(n²)
  - Tốt nhất: O(n) - khi mảng đã sắp xếp
  - Xấu nhất: O(n²) - khi mảng sắp xếp ngược
- **Không gian:** O(1)
- **Tính ổn định:** Ổn định (stable)
- **In-place:** Có
- **Adaptive:** Có (nhanh hơn với mảng đã sắp xếp một phần)

## Ý tưởng thuật toán

1. Bắt đầu từ phần tử thứ hai (index 1)
2. So sánh phần tử hiện tại với các phần tử bên trái
3. Dịch chuyển các phần tử lớn hơn sang phải
4. Chèn phần tử hiện tại vào vị trí đúng
5. Lặp lại cho tất cả phần tử

**Ảnh hưởng:** Giống như sắp xếp bài: giữ các lá bài đã sắp xếp ở tay trái, lấy lá mới và chèn vào vị trí đúng.

## Cài đặt

### C++ - Phiên bản cơ bản

```cpp
#include <bits/stdc++.h>
using namespace std;

void insertionSort(vector<int>& arr) {
    int n = arr.size();

    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;

        // Dịch chuyển các phần tử lớn hơn key sang phải
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }

        // Chèn key vào vị trí đúng
        arr[j + 1] = key;
    }
}

int main() {
    vector<int> arr = {12, 11, 13, 5, 6};

    insertionSort(arr);

    cout << "Mang da sap xep: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### C++ - Phiên bản với swap

```cpp
#include <bits/stdc++.h>
using namespace std;

void insertionSort(vector<int>& arr) {
    int n = arr.size();

    for (int i = 1; i < n; i++) {
        int j = i;

        // Swap liên tục cho đến khi đúng vị trí
        while (j > 0 && arr[j] < arr[j - 1]) {
            swap(arr[j], arr[j - 1]);
            j--;
        }
    }
}

int main() {
    vector<int> arr = {12, 11, 13, 5, 6};
    insertionSort(arr);

    cout << "Mang da sap xep: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### C++ - Binary Insertion Sort

```cpp
#include <bits/stdc++.h>
using namespace std;

// Tìm vị trí chèn bằng binary search
int binarySearch(vector<int>& arr, int item, int low, int high) {
    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (item < arr[mid]) {
            high = mid - 1;
        } else {
            low = mid + 1;
        }
    }

    return low;
}

void binaryInsertionSort(vector<int>& arr) {
    int n = arr.size();

    for (int i = 1; i < n; i++) {
        int key = arr[i];

        // Tìm vị trí chèn bằng binary search
        int pos = binarySearch(arr, key, 0, i - 1);

        // Dịch chuyển các phần tử
        for (int j = i - 1; j >= pos; j--) {
            arr[j + 1] = arr[j];
        }

        // Chèn key
        arr[pos] = key;
    }
}

int main() {
    vector<int> arr = {12, 11, 13, 5, 6};
    binaryInsertionSort(arr);

    cout << "Mang da sap xep: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### C++ - Với sentinel (tối ưu)

```cpp
#include <bits/stdc++.h>
using namespace std;

void insertionSortWithSentinel(vector<int>& arr) {
    int n = arr.size();
    if (n <= 1) return;

    // Đặt phần tử nhỏ nhất ở đầu làm sentinel
    int minIdx = 0;
    for (int i = 1; i < n; i++) {
        if (arr[i] < arr[minIdx]) {
            minIdx = i;
        }
    }
    swap(arr[0], arr[minIdx]);

    // Bây giờ có thể bỏ kiểm tra j >= 0
    for (int i = 2; i < n; i++) {
        int key = arr[i];
        int j = i - 1;

        while (arr[j] > key) {  // Không cần kiểm tra j >= 0
            arr[j + 1] = arr[j];
            j--;
        }

        arr[j + 1] = key;
    }
}

int main() {
    vector<int> arr = {12, 11, 13, 5, 6};
    insertionSortWithSentinel(arr);

    cout << "Mang da sap xep: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### Python

```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1

        # Dịch chuyển các phần tử lớn hơn key
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1

        # Chèn key vào vị trí đúng
        arr[j + 1] = key

    return arr

# Sử dụng
arr = [12, 11, 13, 5, 6]
insertion_sort(arr)
print("Mảng đã sắp xếp:", arr)
```

### Python - Pythonic version

```python
def insertion_sort_pythonic(arr):
    for i in range(1, len(arr)):
        # Chèn arr[i] vào vị trí đúng trong arr[:i]
        key = arr[i]
        # Tìm vị trí bằng bisect
        import bisect
        pos = bisect.bisect_left(arr[:i], key)
        arr.insert(pos, arr.pop(i))

    return arr
```

## Ví dụ minh họa

Sắp xếp mảng: `[5, 2, 4, 6, 1, 3]`

```
Ban đầu: [5, 2, 4, 6, 1, 3]
          ^
        sorted

i=1: key=2
[5, 2, 4, 6, 1, 3]
 ^  ^
Chèn 2 vào trước 5
[2, 5, 4, 6, 1, 3]
    ^
  sorted

i=2: key=4
[2, 5, 4, 6, 1, 3]
    ^  ^
Chèn 4 vào giữa 2 và 5
[2, 4, 5, 6, 1, 3]
       ^
    sorted

i=3: key=6
[2, 4, 5, 6, 1, 3]
          ^
Đã đúng vị trí
[2, 4, 5, 6, 1, 3]
          ^
       sorted

i=4: key=1
[2, 4, 5, 6, 1, 3]
             ^
Chèn 1 vào đầu
[1, 2, 4, 5, 6, 3]
             ^
          sorted

i=5: key=3
[1, 2, 4, 5, 6, 3]
                ^
Chèn 3 vào giữa 2 và 4
[1, 2, 3, 4, 5, 6]
                ^
             sorted
```

## Phân tích chi tiết

### Số lần so sánh và dịch chuyển

**Best case (mảng đã sắp xếp):**
- So sánh: n - 1
- Dịch chuyển: 0
- Tổng: O(n)

**Average case:**
- So sánh: n²/4
- Dịch chuyển: n²/4
- Tổng: O(n²)

**Worst case (mảng sắp xếp ngược):**
- So sánh: n(n-1)/2
- Dịch chuyển: n(n-1)/2
- Tổng: O(n²)

### Đếm số nghịch thế (Inversion)

Insertion Sort thực hiện chính xác **số lần swap = số cặp nghịch thế** trong mảng.

**Định nghĩa:** Cặp nghịch thế là cặp (i, j) sao cho i < j nhưng arr[i] > arr[j].

## Tối ưu hóa

### 1. Binary Insertion Sort

Giảm số lần so sánh từ O(n²) xuống O(n log n), nhưng vẫn O(n²) dịch chuyển:

```cpp
void binaryInsertionSort(vector<int>& arr) {
    for (int i = 1; i < arr.size(); i++) {
        int key = arr[i];

        // Binary search: O(log i)
        int pos = lower_bound(arr.begin(), arr.begin() + i, key) - arr.begin();

        // Shift: O(i)
        for (int j = i; j > pos; j--) {
            arr[j] = arr[j - 1];
        }

        arr[pos] = key;
    }
}
```

### 2. Pair Insertion Sort

Xử lý hai phần tử cùng lúc:

```cpp
void pairInsertionSort(vector<int>& arr) {
    int n = arr.size();

    for (int i = 1; i < n; i += 2) {
        if (i + 1 < n) {
            int a = arr[i], b = arr[i + 1];
            if (a > b) swap(a, b);

            // Chèn a, sau đó chèn b
            // ... (implementation)
        } else {
            // Chèn arr[i] như bình thường
        }
    }
}
```

### 3. Unguarded Insertion Sort

Dùng sentinel để bỏ kiểm tra biên:

```cpp
void unguardedInsertionSort(vector<int>& arr, int left, int right) {
    // Giả định arr[left-1] là sentinel (min)

    for (int i = left + 1; i <= right; i++) {
        int key = arr[i];
        int j = i - 1;

        while (arr[j] > key) {  // Không cần j >= left
            arr[j + 1] = arr[j];
            j--;
        }

        arr[j + 1] = key;
    }
}
```

## Ưu điểm

1. **Đơn giản:** Code ngắn, dễ hiểu và cài đặt
2. **Hiệu quả với mảng nhỏ:** Tốt hơn Quick Sort, Merge Sort
3. **Adaptive:** O(n) cho mảng đã sắp xếp
4. **Ổn định:** Giữ nguyên thứ tự tương đối
5. **In-place:** Chỉ cần O(1) bộ nhớ
6. **Online:** Có thể sắp xếp trong khi nhận dữ liệu
7. **Ít swap:** Tốt cho dữ liệu lớn (swap tốn kém)

## Nhược điểm

1. **Chậm với mảng lớn:** O(n²) không chấp nhận được
2. **Nhiều dịch chuyển:** Kém hiệu quả với mảng ngẫu nhiên
3. **Worst case O(n²):** Không đảm bảo tốc độ

## So sánh với các thuật toán khác

| Thuật toán | Trung bình | Best Case | Worst Case | Stable | Space |
|------------|-----------|-----------|-----------|--------|-------|
| Insertion Sort | O(n²) | O(n) | O(n²) | Có | O(1) |
| Selection Sort | O(n²) | O(n²) | O(n²) | Không | O(1) |
| Bubble Sort | O(n²) | O(n) | O(n²) | Có | O(1) |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | Không | O(log n) |

## Khi nào sử dụng Insertion Sort?

✅ **Nên dùng khi:**
- Mảng nhỏ (< 10-50 phần tử)
- Mảng đã gần sắp xếp
- Cần thuật toán ổn định và in-place
- Online algorithm (dữ liệu đến dần dần)
- Làm phần của thuật toán lai (Hybrid Sort)
- Linked list (không cần dịch chuyển)

❌ **Không nên dùng khi:**
- Mảng lớn và ngẫu nhiên
- Cần đảm bảo O(n log n)

## Ứng dụng thực tế

### 1. Hybrid Sorting (IntroSort, TimSort)

```cpp
const int THRESHOLD = 10;

void hybridSort(vector<int>& arr, int left, int right) {
    if (right - left < THRESHOLD) {
        // Dùng Insertion Sort cho mảng nhỏ
        for (int i = left + 1; i <= right; i++) {
            int key = arr[i];
            int j = i - 1;
            while (j >= left && arr[j] > key) {
                arr[j + 1] = arr[j];
                j--;
            }
            arr[j + 1] = key;
        }
    } else {
        // Dùng Quick Sort cho mảng lớn
        int pivot = partition(arr, left, right);
        hybridSort(arr, left, pivot - 1);
        hybridSort(arr, pivot + 1, right);
    }
}
```

### 2. Online Sorting

```cpp
class OnlineSorter {
private:
    vector<int> sorted;

public:
    void insert(int value) {
        // Tìm vị trí và chèn
        auto pos = lower_bound(sorted.begin(), sorted.end(), value);
        sorted.insert(pos, value);
    }

    vector<int> getSorted() {
        return sorted;
    }
};
```

### 3. Sắp xếp Linked List

```cpp
struct Node {
    int data;
    Node* next;
};

void insertionSortLinkedList(Node*& head) {
    Node* sorted = nullptr;

    Node* current = head;
    while (current != nullptr) {
        Node* next = current->next;

        // Chèn current vào sorted list
        if (sorted == nullptr || sorted->data >= current->data) {
            current->next = sorted;
            sorted = current;
        } else {
            Node* temp = sorted;
            while (temp->next != nullptr && temp->next->data < current->data) {
                temp = temp->next;
            }
            current->next = temp->next;
            temp->next = current;
        }

        current = next;
    }

    head = sorted;
}
```

### 4. Sắp xếp với ít swap

```cpp
// Khi việc swap phần tử rất tốn kém (ví dụ: object lớn)
void insertionSortLessSwap(vector<HugeObject>& arr) {
    for (int i = 1; i < arr.size(); i++) {
        HugeObject key = arr[i];  // Copy một lần
        int j = i - 1;

        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];  // Move (nhanh hơn swap)
            j--;
        }

        arr[j + 1] = key;  // Move một lần
    }
}
```

## Bài tập áp dụng

### Cơ bản
1. Cài đặt Insertion Sort và test
2. Đếm số lần swap cần thiết
3. Sắp xếp giảm dần

### Nâng cao
1. **Binary Insertion Sort:** Cài đặt với binary search
2. **Linked List Insertion Sort:** Sắp xếp linked list
3. **Inversions Count:** Đếm số cặp nghịch thế
4. **K-sorted Array:** Sắp xếp mảng mà mỗi phần tử cách vị trí đúng tối đa k

### Bài tập trên OJ
- CSES - Inversions (có thể dùng Insertion Sort cho n nhỏ)
- LeetCode 147 - Insertion Sort List
- LeetCode 148 - Sort List
- SPOJ - INSORT: Insertion Sort Practice

## Biến thể

### 1. Shell Sort

Cải tiến của Insertion Sort bằng cách sắp xếp các phần tử cách nhau một khoảng h:

```cpp
void shellSort(vector<int>& arr) {
    int n = arr.size();

    // Bắt đầu với gap lớn, giảm dần
    for (int gap = n / 2; gap > 0; gap /= 2) {
        // Insertion sort với gap
        for (int i = gap; i < n; i++) {
            int temp = arr[i];
            int j = i;

            while (j >= gap && arr[j - gap] > temp) {
                arr[j] = arr[j - gap];
                j -= gap;
            }

            arr[j] = temp;
        }
    }
}
```

### 2. Tree Sort

Sử dụng BST để sắp xếp (tương tự ý tưởng Insertion Sort):

```cpp
struct Node {
    int data;
    Node *left, *right;
};

Node* insert(Node* root, int data) {
    if (root == nullptr) return new Node{data, nullptr, nullptr};

    if (data < root->data) {
        root->left = insert(root->left, data);
    } else {
        root->right = insert(root->right, data);
    }

    return root;
}

void inorder(Node* root, vector<int>& result) {
    if (root == nullptr) return;
    inorder(root->left, result);
    result.push_back(root->data);
    inorder(root->right, result);
}

void treeSort(vector<int>& arr) {
    Node* root = nullptr;
    for (int x : arr) {
        root = insert(root, x);
    }

    arr.clear();
    inorder(root, arr);
}
```

## Tài liệu tham khảo

- [VNOI Wiki - Insertion Sort](https://vnoi.info/wiki/algo/basic/sorting.md)
- Introduction to Algorithms (CLRS) - Chapter 2
- [Visualgo - Insertion Sort Visualization](https://visualgo.net/en/sorting)
- The Art of Computer Programming Vol. 3 (Knuth)

## Mẹo trong lập trình thi đấu

1. **Kết hợp với Quick Sort:** STL `sort()` chuyển sang Insertion Sort khi mảng con < 16 phần tử

2. **Kiểm tra đã sắp xếp:**
   ```cpp
   bool is_sorted = true;
   for (int i = 1; i < n; i++) {
       if (arr[i] < arr[i-1]) {
           is_sorted = false;
           break;
       }
   }
   if (!is_sorted) insertionSort(arr);
   ```

3. **Tối ưu cho mảng nhỏ:**
   ```cpp
   if (n <= 10) {
       insertionSort(arr);
   } else {
       quickSort(arr);
   }
   ```

4. **Linked List:** Insertion Sort tốt hơn nhiều cho linked list (O(n²) nhưng không cần dịch chuyển)

5. **Sentinel trick:** Đặt min ở đầu để bỏ kiểm tra biên, tăng tốc ~10-20%
