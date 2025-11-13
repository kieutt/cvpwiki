# Merge Sort (Sắp xếp trộn)

## Giới thiệu

Merge Sort là một thuật toán sắp xếp dựa trên kỹ thuật **Chia để trị** (Divide and Conquer). Thuật toán được phát minh bởi John von Neumann năm 1945 và là một trong những thuật toán sắp xếp ổn định, hiệu quả nhất.

Merge Sort hoạt động bằng cách chia mảng thành hai nửa, sắp xếp đệ quy từng nửa, sau đó **trộn** (merge) hai nửa đã sắp xếp lại với nhau.

## Độ phức tạp

- **Thời gian:**
  - Trung bình: O(n log n)
  - Tốt nhất: O(n log n)
  - Xấu nhất: O(n log n)
- **Không gian:** O(n) - cần mảng phụ để trộn
- **Tính ổn định:** Ổn định (stable)
- **In-place:** Không (cần bộ nhớ phụ)

## Ý tưởng thuật toán

1. **Chia (Divide):** Chia mảng thành hai nửa bằng nhau
2. **Trị (Conquer):** Sắp xếp đệ quy từng nửa
3. **Kết hợp (Combine):** Trộn hai nửa đã sắp xếp thành một mảng đã sắp xếp

## Cài đặt

### C++ - Phiên bản cơ bản

```cpp
#include <bits/stdc++.h>
using namespace std;

// Hàm trộn hai mảng con đã sắp xếp
void merge(vector<int>& arr, int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;

    // Tạo mảng tạm
    vector<int> L(n1), R(n2);

    // Copy dữ liệu vào mảng tạm
    for (int i = 0; i < n1; i++)
        L[i] = arr[left + i];
    for (int j = 0; j < n2; j++)
        R[j] = arr[mid + 1 + j];

    // Trộn hai mảng tạm về mảng gốc
    int i = 0, j = 0, k = left;

    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k] = L[i];
            i++;
        } else {
            arr[k] = R[j];
            j++;
        }
        k++;
    }

    // Copy các phần tử còn lại của L[]
    while (i < n1) {
        arr[k] = L[i];
        i++;
        k++;
    }

    // Copy các phần tử còn lại của R[]
    while (j < n2) {
        arr[k] = R[j];
        j++;
        k++;
    }
}

void mergeSort(vector<int>& arr, int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;

        // Sắp xếp nửa đầu và nửa sau
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);

        // Trộn hai nửa đã sắp xếp
        merge(arr, left, mid, right);
    }
}

int main() {
    vector<int> arr = {12, 11, 13, 5, 6, 7};
    int n = arr.size();

    mergeSort(arr, 0, n - 1);

    cout << "Mang da sap xep: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### C++ - Phiên bản tối ưu (In-place merge với mảng tạm toàn cục)

```cpp
#include <bits/stdc++.h>
using namespace std;

const int MAXN = 1e6 + 5;
int temp[MAXN];  // Mảng tạm toàn cục để tránh tạo mảng nhiều lần

void merge(vector<int>& arr, int left, int mid, int right) {
    int i = left, j = mid + 1, k = left;

    // Trộn vào mảng tạm
    while (i <= mid && j <= right) {
        if (arr[i] <= arr[j]) {
            temp[k++] = arr[i++];
        } else {
            temp[k++] = arr[j++];
        }
    }

    // Copy phần còn lại
    while (i <= mid) temp[k++] = arr[i++];
    while (j <= right) temp[k++] = arr[j++];

    // Copy từ mảng tạm về mảng gốc
    for (i = left; i <= right; i++) {
        arr[i] = temp[i];
    }
}

void mergeSort(vector<int>& arr, int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);
        merge(arr, left, mid, right);
    }
}

int main() {
    vector<int> arr = {12, 11, 13, 5, 6, 7};
    int n = arr.size();

    mergeSort(arr, 0, n - 1);

    cout << "Mang da sap xep: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### C++ - Bottom-up (Iterative)

```cpp
#include <bits/stdc++.h>
using namespace std;

void merge(vector<int>& arr, int left, int mid, int right) {
    vector<int> temp(right - left + 1);
    int i = left, j = mid + 1, k = 0;

    while (i <= mid && j <= right) {
        if (arr[i] <= arr[j]) {
            temp[k++] = arr[i++];
        } else {
            temp[k++] = arr[j++];
        }
    }

    while (i <= mid) temp[k++] = arr[i++];
    while (j <= right) temp[k++] = arr[j++];

    for (i = 0; i < k; i++) {
        arr[left + i] = temp[i];
    }
}

void mergeSortIterative(vector<int>& arr) {
    int n = arr.size();

    // Bắt đầu với size = 1, tăng dần lên 2, 4, 8, ...
    for (int size = 1; size < n; size *= 2) {
        // Chọn điểm bắt đầu của mảng con trái
        for (int left = 0; left < n - 1; left += 2 * size) {
            int mid = min(left + size - 1, n - 1);
            int right = min(left + 2 * size - 1, n - 1);

            merge(arr, left, mid, right);
        }
    }
}

int main() {
    vector<int> arr = {12, 11, 13, 5, 6, 7};
    mergeSortIterative(arr);

    cout << "Mang da sap xep: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### Python

```python
def merge(arr, left, mid, right):
    # Tạo mảng tạm
    L = arr[left:mid + 1]
    R = arr[mid + 1:right + 1]

    i = j = 0
    k = left

    # Trộn hai mảng
    while i < len(L) and j < len(R):
        if L[i] <= R[j]:
            arr[k] = L[i]
            i += 1
        else:
            arr[k] = R[j]
            j += 1
        k += 1

    # Copy phần còn lại
    while i < len(L):
        arr[k] = L[i]
        i += 1
        k += 1

    while j < len(R):
        arr[k] = R[j]
        j += 1
        k += 1

def merge_sort(arr, left, right):
    if left < right:
        mid = (left + right) // 2
        merge_sort(arr, left, mid)
        merge_sort(arr, mid + 1, right)
        merge(arr, left, mid, right)

# Sử dụng
arr = [12, 11, 13, 5, 6, 7]
merge_sort(arr, 0, len(arr) - 1)
print("Mảng đã sắp xếp:", arr)
```

## Ví dụ minh họa

Sắp xếp mảng: `[38, 27, 43, 3, 9, 82, 10]`

**Bước 1: Chia**
```
[38, 27, 43, 3, 9, 82, 10]
         |
    /         \
[38, 27, 43]  [3, 9, 82, 10]
     |              |
   /   \          /    \
[38] [27,43]   [3,9] [82,10]
      |          |      |
    /  \       /  \   /  \
  [27] [43]  [3] [9][82][10]
```

**Bước 2: Trộn**
```
[27] [43] → [27, 43]
[3] [9] → [3, 9]
[82] [10] → [10, 82]

[38] [27, 43] → [27, 38, 43]
[3, 9] [10, 82] → [3, 9, 10, 82]

[27, 38, 43] [3, 9, 10, 82] → [3, 9, 10, 27, 38, 43, 82]
```

## Tối ưu hóa

### 1. Tối ưu với mảng nhỏ

```cpp
const int THRESHOLD = 10;

void mergeSort(vector<int>& arr, int left, int right) {
    if (right - left < THRESHOLD) {
        // Sử dụng Insertion Sort cho mảng nhỏ
        insertionSort(arr, left, right);
        return;
    }

    if (left < right) {
        int mid = left + (right - left) / 2;
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);
        merge(arr, left, mid, right);
    }
}
```

### 2. Kiểm tra đã sắp xếp

```cpp
void mergeSort(vector<int>& arr, int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);

        // Nếu arr[mid] <= arr[mid+1] thì đã sắp xếp, không cần merge
        if (arr[mid] > arr[mid + 1]) {
            merge(arr, left, mid, right);
        }
    }
}
```

### 3. Natural Merge Sort

```cpp
// Sắp xếp bằng cách tận dụng các đoạn đã sắp xếp sẵn trong mảng
void naturalMergeSort(vector<int>& arr) {
    int n = arr.size();
    vector<int> runs;  // Lưu vị trí bắt đầu của các đoạn tăng dần

    // Tìm các đoạn tăng dần
    runs.push_back(0);
    for (int i = 1; i < n; i++) {
        if (arr[i] < arr[i - 1]) {
            runs.push_back(i);
        }
    }
    runs.push_back(n);

    // Trộn các đoạn
    while (runs.size() > 2) {
        vector<int> newRuns = {0};
        for (int i = 0; i + 2 < runs.size(); i += 2) {
            merge(arr, runs[i], runs[i + 1] - 1, runs[i + 2] - 1);
            newRuns.push_back(runs[i + 2]);
        }
        if (runs.size() % 2 == 0) {
            newRuns.push_back(n);
        }
        runs = newRuns;
    }
}
```

## Ưu điểm

1. **Độ phức tạp ổn định:** Luôn O(n log n) trong mọi trường hợp
2. **Thuật toán ổn định:** Giữ nguyên thứ tự tương đối của các phần tử bằng nhau
3. **Dễ song song hóa:** Có thể chia nhỏ và xử lý song song
4. **Hiệu quả với linked list:** Không cần mảng phụ khi sắp xếp linked list
5. **External sorting:** Rất tốt cho sắp xếp dữ liệu lớn không nằm hết trong RAM

## Nhược điểm

1. **Tốn bộ nhớ:** Cần O(n) bộ nhớ phụ
2. **Không in-place:** Không thể sắp xếp tại chỗ
3. **Chậm hơn Quick Sort:** Trong thực tế thường chậm hơn Quick Sort trên mảng nhỏ
4. **Overhead:** Chi phí tạo và copy mảng tạm

## So sánh với các thuật toán khác

| Thuật toán | Trung bình | Worst Case | Stable | Space |
|------------|-----------|-----------|--------|-------|
| Merge Sort | O(n log n) | O(n log n) | Có | O(n) |
| Quick Sort | O(n log n) | O(n²) | Không | O(log n) |
| Heap Sort | O(n log n) | O(n log n) | Không | O(1) |

## Khi nào sử dụng Merge Sort?

✅ **Nên dùng khi:**
- Cần đảm bảo O(n log n) trong mọi trường hợp
- Cần thuật toán ổn định
- Sắp xếp linked list
- External sorting (dữ liệu lớn)
- Bộ nhớ không bị hạn chế

❌ **Không nên dùng khi:**
- Bộ nhớ hạn chế (dùng Quick Sort hoặc Heap Sort)
- Cần sắp xếp in-place
- Mảng nhỏ (dùng Insertion Sort)

## Ứng dụng đặc biệt

### 1. Đếm số cặp nghịch thế (Inversion Count)

```cpp
long long mergeAndCount(vector<int>& arr, int left, int mid, int right) {
    vector<int> temp;
    int i = left, j = mid + 1;
    long long inv_count = 0;

    while (i <= mid && j <= right) {
        if (arr[i] <= arr[j]) {
            temp.push_back(arr[i++]);
        } else {
            temp.push_back(arr[j++]);
            inv_count += (mid - i + 1);  // Đếm nghịch thế
        }
    }

    while (i <= mid) temp.push_back(arr[i++]);
    while (j <= right) temp.push_back(arr[j++]);

    for (int i = left, k = 0; i <= right; i++, k++) {
        arr[i] = temp[k];
    }

    return inv_count;
}

long long countInversions(vector<int>& arr, int left, int right) {
    long long inv_count = 0;
    if (left < right) {
        int mid = left + (right - left) / 2;
        inv_count += countInversions(arr, left, mid);
        inv_count += countInversions(arr, mid + 1, right);
        inv_count += mergeAndCount(arr, left, mid, right);
    }
    return inv_count;
}
```

### 2. Merge K Sorted Arrays

```cpp
vector<int> mergeKArrays(vector<vector<int>>& arrays) {
    // Sử dụng priority queue (min heap)
    priority_queue<tuple<int, int, int>, vector<tuple<int, int, int>>, greater<>> pq;

    // Thêm phần tử đầu tiên của mỗi mảng vào heap
    for (int i = 0; i < arrays.size(); i++) {
        if (!arrays[i].empty()) {
            pq.push({arrays[i][0], i, 0});
        }
    }

    vector<int> result;
    while (!pq.empty()) {
        auto [val, arrayIdx, elemIdx] = pq.top();
        pq.pop();

        result.push_back(val);

        // Thêm phần tử tiếp theo từ cùng mảng
        if (elemIdx + 1 < arrays[arrayIdx].size()) {
            pq.push({arrays[arrayIdx][elemIdx + 1], arrayIdx, elemIdx + 1});
        }
    }

    return result;
}
```

## Bài tập áp dụng

### Cơ bản
1. Cài đặt Merge Sort và test với các trường hợp khác nhau
2. So sánh số lần so sánh của Merge Sort với Quick Sort
3. Cài đặt cả phiên bản đệ quy và iterative

### Nâng cao
1. **CSES - Inversion Count:** Đếm số cặp nghịch thế trong mảng
2. **Merge K Sorted Lists:** Trộn k danh sách đã sắp xếp
3. **Count of Range Sum:** Đếm số đoạn con có tổng trong khoảng [lower, upper]
4. **Reverse Pairs:** Đếm số cặp (i, j) sao cho i < j và arr[i] > 2*arr[j]

### Bài tập trên OJ
- CSES - Inversions
- SPOJ - MSORT: Merge Sort Practice
- VNOI - NKINV: Đếm cặp nghịch thế
- LeetCode 315 - Count of Smaller Numbers After Self

## Tim Sort

**Tim Sort** là thuật toán lai giữa Merge Sort và Insertion Sort, được sử dụng trong Python và Java:

```cpp
const int RUN = 32;

void insertionSort(vector<int>& arr, int left, int right) {
    for (int i = left + 1; i <= right; i++) {
        int key = arr[i];
        int j = i - 1;
        while (j >= left && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}

void timSort(vector<int>& arr) {
    int n = arr.size();

    // Sắp xếp các đoạn nhỏ bằng Insertion Sort
    for (int start = 0; start < n; start += RUN) {
        int end = min(start + RUN - 1, n - 1);
        insertionSort(arr, start, end);
    }

    // Bắt đầu merge từ size = RUN
    for (int size = RUN; size < n; size *= 2) {
        for (int left = 0; left < n; left += 2 * size) {
            int mid = left + size - 1;
            int right = min(left + 2 * size - 1, n - 1);

            if (mid < right) {
                merge(arr, left, mid, right);
            }
        }
    }
}
```

## Tài liệu tham khảo

- [VNOI Wiki - Merge Sort](https://vnoi.info/wiki/algo/basic/sorting.md)
- Introduction to Algorithms (CLRS) - Chapter 2
- [Visualgo - Merge Sort Visualization](https://visualgo.net/en/sorting)

## Mẹo trong lập trình thi đấu

1. **Sử dụng STL:** `stable_sort()` trong C++ sử dụng Merge Sort
   ```cpp
   stable_sort(arr.begin(), arr.end());
   ```

2. **Mảng tạm toàn cục:** Để tránh overhead của việc tạo mảng nhiều lần
   ```cpp
   const int MAXN = 1e6 + 5;
   int temp[MAXN];
   ```

3. **Đếm nghịch thế:** Merge Sort là cách tối ưu nhất để đếm inversion count

4. **External sorting:** Khi dữ liệu quá lớn, sử dụng Merge Sort với file
