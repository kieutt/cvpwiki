# Selection Sort (Sắp xếp chọn)

## Giới thiệu

Selection Sort là một trong những thuật toán sắp xếp đơn giản nhất. Ý tưởng chính là **lựa chọn** phần tử nhỏ nhất (hoặc lớn nhất) từ phần chưa sắp xếp và đặt nó vào đầu (hoặc cuối) phần đã sắp xếp.

Thuật toán này được đặt tên là "Selection" vì nó liên tục **chọn** (select) phần tử tối ưu (min hoặc max) trong mỗi bước.

## Độ phức tạp

- **Thời gian:**
  - Trung bình: O(n²)
  - Tốt nhất: O(n²)
  - Xấu nhất: O(n²)
- **Không gian:** O(1)
- **Tính ổn định:** Không ổn định (unstable) - có thể làm ổn định
- **In-place:** Có
- **Adaptive:** Không (luôn O(n²) dù mảng đã sắp xếp)

## Ý tưởng thuật toán

1. Chia mảng thành hai phần: đã sắp xếp (bên trái) và chưa sắp xếp (bên phải)
2. Ban đầu phần đã sắp xếp rỗng
3. Với mỗi vòng lặp:
   - Tìm phần tử nhỏ nhất trong phần chưa sắp xếp
   - Swap nó với phần tử đầu tiên của phần chưa sắp xếp
   - Mở rộng phần đã sắp xếp thêm 1 phần tử

## Cài đặt

### C++ - Phiên bản cơ bản

```cpp
#include <bits/stdc++.h>
using namespace std;

void selectionSort(vector<int>& arr) {
    int n = arr.size();

    // Di chuyển biên của phần chưa sắp xếp
    for (int i = 0; i < n - 1; i++) {
        // Tìm phần tử nhỏ nhất trong phần chưa sắp xếp
        int minIdx = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIdx]) {
                minIdx = j;
            }
        }

        // Swap phần tử nhỏ nhất với phần tử đầu tiên
        if (minIdx != i) {
            swap(arr[i], arr[minIdx]);
        }
    }
}

int main() {
    vector<int> arr = {64, 25, 12, 22, 11};

    selectionSort(arr);

    cout << "Mang da sap xep: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### C++ - Stable Selection Sort

```cpp
#include <bits/stdc++.h>
using namespace std;

void stableSelectionSort(vector<int>& arr) {
    int n = arr.size();

    for (int i = 0; i < n - 1; i++) {
        // Tìm phần tử nhỏ nhất
        int minIdx = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIdx]) {
                minIdx = j;
            }
        }

        // Dịch chuyển thay vì swap để giữ tính stable
        int key = arr[minIdx];
        for (int j = minIdx; j > i; j--) {
            arr[j] = arr[j - 1];
        }
        arr[i] = key;
    }
}

int main() {
    vector<int> arr = {64, 25, 12, 22, 11};
    stableSelectionSort(arr);

    cout << "Mang da sap xep: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### C++ - Bidirectional Selection Sort (Cocktail Selection Sort)

```cpp
#include <bits/stdc++.h>
using namespace std;

void bidirectionalSelectionSort(vector<int>& arr) {
    int left = 0, right = arr.size() - 1;

    while (left < right) {
        // Tìm min và max trong một pass
        int minIdx = left, maxIdx = left;

        for (int i = left; i <= right; i++) {
            if (arr[i] < arr[minIdx]) {
                minIdx = i;
            }
            if (arr[i] > arr[maxIdx]) {
                maxIdx = i;
            }
        }

        // Swap min với left
        if (minIdx != left) {
            swap(arr[left], arr[minIdx]);

            // Nếu maxIdx = left, cập nhật maxIdx
            if (maxIdx == left) {
                maxIdx = minIdx;
            }
        }

        // Swap max với right
        if (maxIdx != right) {
            swap(arr[right], arr[maxIdx]);
        }

        left++;
        right--;
    }
}

int main() {
    vector<int> arr = {64, 25, 12, 22, 11};
    bidirectionalSelectionSort(arr);

    cout << "Mang da sap xep: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### C++ - Sắp xếp giảm dần

```cpp
#include <bits/stdc++.h>
using namespace std;

void selectionSortDescending(vector<int>& arr) {
    int n = arr.size();

    for (int i = 0; i < n - 1; i++) {
        // Tìm phần tử LỚN nhất
        int maxIdx = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] > arr[maxIdx]) {
                maxIdx = j;
            }
        }

        if (maxIdx != i) {
            swap(arr[i], arr[maxIdx]);
        }
    }
}

int main() {
    vector<int> arr = {64, 25, 12, 22, 11};
    selectionSortDescending(arr);

    cout << "Mang da sap xep giam dan: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### Python

```python
def selection_sort(arr):
    n = len(arr)

    for i in range(n - 1):
        # Tìm phần tử nhỏ nhất
        min_idx = i
        for j in range(i + 1, n):
            if arr[j] < arr[min_idx]:
                min_idx = j

        # Swap
        if min_idx != i:
            arr[i], arr[min_idx] = arr[min_idx], arr[i]

    return arr

# Sử dụng
arr = [64, 25, 12, 22, 11]
selection_sort(arr)
print("Mảng đã sắp xếp:", arr)
```

### Python - Pythonic version

```python
def selection_sort_pythonic(arr):
    for i in range(len(arr) - 1):
        min_idx = min(range(i, len(arr)), key=lambda j: arr[j])
        arr[i], arr[min_idx] = arr[min_idx], arr[i]
    return arr
```

## Ví dụ minh họa

Sắp xếp mảng: `[64, 25, 12, 22, 11]`

```
Ban đầu: [64, 25, 12, 22, 11]
           ^
        unsorted

Vòng 1: Tìm min = 11 tại index 4
[64, 25, 12, 22, 11]
 ^               ^
Swap 64 và 11
[11, 25, 12, 22, 64]
 ^
sorted

Vòng 2: Tìm min = 12 tại index 2
[11, 25, 12, 22, 64]
     ^   ^
Swap 25 và 12
[11, 12, 25, 22, 64]
     ^
   sorted

Vòng 3: Tìm min = 22 tại index 3
[11, 12, 25, 22, 64]
         ^   ^
Swap 25 và 22
[11, 12, 22, 25, 64]
         ^
      sorted

Vòng 4: Tìm min = 25 tại index 3
[11, 12, 22, 25, 64]
             ^
Đã đúng vị trí, không swap
[11, 12, 22, 25, 64]
             ^
          sorted

Kết quả: [11, 12, 22, 25, 64]
```

## Phân tích chi tiết

### Số lần so sánh

**Trong mọi trường hợp (Best, Average, Worst):**
```
So sánh = (n-1) + (n-2) + ... + 1
        = n(n-1)/2
        = O(n²)
```

Selection Sort luôn thực hiện **đúng n(n-1)/2 phép so sánh**, không phụ thuộc vào dữ liệu đầu vào!

### Số lần swap

- **Best case:** 0 swap (mảng đã sắp xếp)
- **Average case:** O(n) swap
- **Worst case:** n-1 swap (tối đa)

Selection Sort có **ít swap hơn nhiều** so với Bubble Sort và Insertion Sort!

## Tối ưu hóa

### 1. Bidirectional (Tìm cả min và max)

```cpp
void optimizedSelectionSort(vector<int>& arr) {
    int left = 0, right = arr.size() - 1;

    while (left < right) {
        int minIdx = left, maxIdx = left;

        // Một pass tìm cả min và max
        for (int i = left + 1; i <= right; i++) {
            if (arr[i] < arr[minIdx]) minIdx = i;
            if (arr[i] > arr[maxIdx]) maxIdx = i;
        }

        // Xử lý swap cẩn thận
        if (minIdx == right && maxIdx == left) {
            swap(arr[left], arr[right]);
        } else {
            if (minIdx != left) swap(arr[left], arr[minIdx]);
            if (maxIdx != right) {
                if (maxIdx == left) maxIdx = minIdx;
                swap(arr[right], arr[maxIdx]);
            }
        }

        left++;
        right--;
    }
}
```

### 2. Early Termination

```cpp
void selectionSortWithEarlyStop(vector<int>& arr) {
    int n = arr.size();
    bool swapped;

    for (int i = 0; i < n - 1; i++) {
        int minIdx = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIdx]) {
                minIdx = j;
            }
        }

        swapped = (minIdx != i);
        if (swapped) {
            swap(arr[i], arr[minIdx]);
        }

        // Nếu không swap trong vòng lặp, có thể dừng sớm
        // (Nhưng Selection Sort ít khi có lợi từ optimization này)
    }
}
```

### 3. Sử dụng Heap cho Selection

```cpp
// Thực chất là Heap Sort - Selection Sort tối ưu
void heapSelectionSort(vector<int>& arr) {
    priority_queue<int, vector<int>, greater<int>> minHeap(arr.begin(), arr.end());

    for (int i = 0; i < arr.size(); i++) {
        arr[i] = minHeap.top();
        minHeap.pop();
    }
}
```

## Ưu điểm

1. **Đơn giản:** Code ngắn và dễ hiểu
2. **Ít swap:** Chỉ O(n) swap, tốt khi swap tốn kém
3. **In-place:** Chỉ cần O(1) bộ nhớ
4. **Dự đoán được:** Luôn O(n²), không có worst case bất ngờ
5. **Không cần extra space:** Không như Merge Sort

## Nhược điểm

1. **Luôn O(n²):** Không adaptive, chậm với mảng lớn
2. **Không ổn định:** Có thể thay đổi thứ tự các phần tử bằng nhau
3. **Không hiệu quả:** Chậm hơn Insertion Sort trong thực tế
4. **Nhiều so sánh:** n²/2 so sánh trong mọi trường hợp

## So sánh với các thuật toán khác

| Thuật toán | So sánh | Swap/Move | Adaptive | Stable |
|------------|---------|-----------|----------|--------|
| Selection Sort | n²/2 | n | Không | Không |
| Insertion Sort | ~n²/4 | ~n²/4 | Có | Có |
| Bubble Sort | ~n²/2 | ~n²/2 | Có | Có |

**Selection Sort vs Insertion Sort:**
- Selection Sort: nhiều so sánh hơn, ít swap hơn
- Insertion Sort: ít so sánh hơn, nhiều swap hơn
- **Kết luận:** Insertion Sort thường nhanh hơn trong thực tế

## Khi nào sử dụng Selection Sort?

✅ **Nên dùng khi:**
- Swap rất tốn kém (ví dụ: object lớn, flash memory)
- Cần thuật toán đơn giản để giảng dạy
- Bộ nhớ rất hạn chế
- Mảng rất nhỏ (< 10 phần tử)

❌ **Không nên dùng khi:**
- Mảng lớn (> 50 phần tử)
- Cần tốc độ (dùng Quick Sort, Merge Sort)
- Cần thuật toán ổn định
- Mảng đã gần sắp xếp (Insertion Sort tốt hơn)

## Ứng dụng đặc biệt

### 1. Sắp xếp với chi phí swap cao

```cpp
struct HugeObject {
    char data[10000];
    int key;

    bool operator<(const HugeObject& other) const {
        return key < other.key;
    }
};

void sortHugeObjects(vector<HugeObject>& arr) {
    // Selection Sort vì ít swap nhất
    int n = arr.size();

    for (int i = 0; i < n - 1; i++) {
        int minIdx = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIdx]) {
                minIdx = j;
            }
        }

        if (minIdx != i) {
            swap(arr[i], arr[minIdx]);  // Chỉ 1 swap mỗi vòng
        }
    }
}
```

### 2. Tìm K phần tử nhỏ nhất

```cpp
vector<int> findKSmallest(vector<int>& arr, int k) {
    // Chỉ cần k vòng lặp của Selection Sort
    int n = arr.size();

    for (int i = 0; i < k && i < n - 1; i++) {
        int minIdx = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIdx]) {
                minIdx = j;
            }
        }
        if (minIdx != i) {
            swap(arr[i], arr[minIdx]);
        }
    }

    return vector<int>(arr.begin(), arr.begin() + k);
}
```

### 3. Tìm median

```cpp
int findMedian(vector<int>& arr) {
    int n = arr.size();
    int k = n / 2;

    // Chỉ cần k+1 vòng lặp
    for (int i = 0; i <= k; i++) {
        int minIdx = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIdx]) {
                minIdx = j;
            }
        }
        if (minIdx != i) {
            swap(arr[i], arr[minIdx]);
        }
    }

    return arr[k];
}
```

### 4. Partial Sorting

```cpp
void partialSort(vector<int>& arr, int k) {
    // Sắp xếp k phần tử đầu tiên
    int n = arr.size();

    for (int i = 0; i < k && i < n - 1; i++) {
        int minIdx = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIdx]) {
                minIdx = j;
            }
        }
        if (minIdx != i) {
            swap(arr[i], arr[minIdx]);
        }
    }
}
```

## Bài tập áp dụng

### Cơ bản
1. Cài đặt Selection Sort và test
2. Đếm số lần so sánh và swap
3. Sắp xếp giảm dần

### Nâng cao
1. **Stable Selection Sort:** Cài đặt phiên bản ổn định
2. **Bidirectional Selection Sort:** Tìm cả min và max
3. **K Smallest Elements:** Tìm k phần tử nhỏ nhất
4. **Partial Sort:** Sắp xếp một phần mảng

### Bài tập trên OJ
- CSES - Sorting Practice
- SPOJ - SELSORT: Selection Sort Practice
- LeetCode 215 - Kth Largest Element (có thể dùng Selection Sort)

## Biến thể

### 1. Double Selection Sort

Tìm cả min và max trong mỗi pass:

```cpp
void doubleSelectionSort(vector<int>& arr) {
    int n = arr.size();

    for (int i = 0; i < n / 2; i++) {
        int minIdx = i, maxIdx = i;

        for (int j = i; j < n - i; j++) {
            if (arr[j] < arr[minIdx]) minIdx = j;
            if (arr[j] > arr[maxIdx]) maxIdx = j;
        }

        swap(arr[i], arr[minIdx]);
        if (maxIdx == i) maxIdx = minIdx;
        swap(arr[n - 1 - i], arr[maxIdx]);
    }
}
```

### 2. Cycle Sort

Tối ưu hóa để có chính xác n-1 lần ghi (write):

```cpp
void cycleSort(vector<int>& arr) {
    int n = arr.size();

    for (int cycleStart = 0; cycleStart < n - 1; cycleStart++) {
        int item = arr[cycleStart];

        // Tìm vị trí đúng
        int pos = cycleStart;
        for (int i = cycleStart + 1; i < n; i++) {
            if (arr[i] < item) pos++;
        }

        if (pos == cycleStart) continue;

        // Bỏ qua duplicate
        while (item == arr[pos]) pos++;

        // Đặt item vào vị trí đúng
        swap(item, arr[pos]);

        // Rotate cycle
        while (pos != cycleStart) {
            pos = cycleStart;
            for (int i = cycleStart + 1; i < n; i++) {
                if (arr[i] < item) pos++;
            }

            while (item == arr[pos]) pos++;
            swap(item, arr[pos]);
        }
    }
}
```

### 3. Heap Sort

Selection Sort với heap để tối ưu tìm min:

```cpp
void heapSort(vector<int>& arr) {
    // Heap Sort thực chất là Selection Sort tối ưu
    // Giảm tìm min từ O(n) xuống O(log n)
    make_heap(arr.begin(), arr.end());

    for (int i = arr.size() - 1; i > 0; i--) {
        pop_heap(arr.begin(), arr.begin() + i + 1);
    }
}
```

## Tài liệu tham khảo

- [VNOI Wiki - Selection Sort](https://vnoi.info/wiki/algo/basic/sorting.md)
- Introduction to Algorithms (CLRS) - Chapter 2
- [Visualgo - Selection Sort Visualization](https://visualgo.net/en/sorting)
- The Art of Computer Programming Vol. 3 (Knuth)

## Mẹo trong lập trình thi đấu

1. **Không dùng trong contests:** Selection Sort quá chậm, luôn dùng `sort()` của STL

2. **Tìm k smallest:** Nếu k << n, Selection Sort có thể chấp nhận được
   ```cpp
   // O(k × n) thay vì O(n log n)
   for (int i = 0; i < k; i++) {
       // Selection Sort pass
   }
   ```

3. **Chi phí swap cao:** Khi swap rất tốn kém, Selection Sort tốt hơn Insertion Sort

4. **Giảng dạy:** Tốt để giảng dạy vì đơn giản và dễ hiểu

5. **Benchmark:** So sánh với Insertion Sort để hiểu về adaptive algorithms
   ```cpp
   // Selection Sort: luôn n²/2 so sánh
   // Insertion Sort: n đến n²/2 so sánh (adaptive)
   ```

## Kết luận

Selection Sort là thuật toán đơn giản nhưng **không hiệu quả** trong thực tế. Chỉ nên sử dụng khi:
- Giảng dạy về thuật toán
- Swap rất tốn kém
- Mảng cực kỳ nhỏ

Trong hầu hết các trường hợp khác, nên dùng:
- **Insertion Sort:** cho mảng nhỏ hoặc gần sắp xếp
- **Quick Sort / Merge Sort:** cho mảng lớn
- **STL sort():** trong lập trình thi đấu
