# Quick Sort (Sắp xếp nhanh)

## Giới thiệu

Quick Sort là một thuật toán sắp xếp dựa trên kỹ thuật **Chia để trị** (Divide and Conquer). Thuật toán được phát triển bởi Tony Hoare năm 1959 và là một trong những thuật toán sắp xếp hiệu quả nhất trong thực tế.

Quick Sort hoạt động bằng cách chọn một phần tử làm **pivot** (trục), sau đó phân chia mảng thành hai phần: các phần tử nhỏ hơn pivot ở bên trái và các phần tử lớn hơn pivot ở bên phải. Quá trình này được lặp lại đệ quy cho các mảng con.

## Độ phức tạp

- **Thời gian:**
  - Trung bình: O(n log n)
  - Tốt nhất: O(n log n)
  - Xấu nhất: O(n²) - khi pivot luôn là phần tử nhỏ nhất hoặc lớn nhất
- **Không gian:** O(log n) - do đệ quy
- **Tính ổn định:** Không ổn định (unstable)
- **In-place:** Có

## Ý tưởng thuật toán

1. **Chọn pivot:** Chọn một phần tử làm pivot (có thể là phần tử đầu, cuối, giữa hoặc ngẫu nhiên)
2. **Phân hoạch (Partition):** Sắp xếp lại mảng sao cho:
   - Tất cả phần tử nhỏ hơn pivot nằm bên trái
   - Tất cả phần tử lớn hơn pivot nằm bên phải
   - Pivot nằm ở vị trí đúng của nó
3. **Đệ quy:** Áp dụng Quick Sort cho mảng con bên trái và bên phải pivot

## Cài đặt

### C++ - Phiên bản cơ bản (Lomuto Partition)

```cpp
#include <bits/stdc++.h>
using namespace std;

// Hàm phân hoạch theo sơ đồ Lomuto
int partition(vector<int>& arr, int low, int high) {
    int pivot = arr[high];  // Chọn phần tử cuối làm pivot
    int i = low - 1;  // Chỉ số của phần tử nhỏ hơn

    for (int j = low; j < high; j++) {
        if (arr[j] <= pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    swap(arr[i + 1], arr[high]);
    return i + 1;
}

void quickSort(vector<int>& arr, int low, int high) {
    if (low < high) {
        // pi là chỉ số phân hoạch
        int pi = partition(arr, low, high);

        // Sắp xếp đệ quy các phần tử trước và sau phân hoạch
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

int main() {
    vector<int> arr = {10, 7, 8, 9, 1, 5};
    int n = arr.size();

    quickSort(arr, 0, n - 1);

    cout << "Mang da sap xep: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### C++ - Phiên bản tối ưu (Hoare Partition)

```cpp
#include <bits/stdc++.h>
using namespace std;

// Hàm phân hoạch theo sơ đồ Hoare (hiệu quả hơn)
int partition(vector<int>& arr, int low, int high) {
    int pivot = arr[low];  // Chọn phần tử đầu làm pivot
    int i = low - 1;
    int j = high + 1;

    while (true) {
        do { i++; } while (arr[i] < pivot);
        do { j--; } while (arr[j] > pivot);

        if (i >= j) return j;
        swap(arr[i], arr[j]);
    }
}

void quickSort(vector<int>& arr, int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi);
        quickSort(arr, pi + 1, high);
    }
}

int main() {
    vector<int> arr = {10, 7, 8, 9, 1, 5};
    int n = arr.size();

    quickSort(arr, 0, n - 1);

    cout << "Mang da sap xep: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### C++ - Phiên bản với Random Pivot

```cpp
#include <bits/stdc++.h>
using namespace std;

int partition(vector<int>& arr, int low, int high) {
    // Chọn pivot ngẫu nhiên để tránh worst case
    int randomIndex = low + rand() % (high - low + 1);
    swap(arr[randomIndex], arr[high]);

    int pivot = arr[high];
    int i = low - 1;

    for (int j = low; j < high; j++) {
        if (arr[j] <= pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    swap(arr[i + 1], arr[high]);
    return i + 1;
}

void quickSort(vector<int>& arr, int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

int main() {
    srand(time(0));  // Khởi tạo random seed
    vector<int> arr = {10, 7, 8, 9, 1, 5};
    int n = arr.size();

    quickSort(arr, 0, n - 1);

    cout << "Mang da sap xep: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### Python

```python
def partition(arr, low, high):
    pivot = arr[high]
    i = low - 1

    for j in range(low, high):
        if arr[j] <= pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]

    arr[i + 1], arr[high] = arr[high], arr[i + 1]
    return i + 1

def quick_sort(arr, low, high):
    if low < high:
        pi = partition(arr, low, high)
        quick_sort(arr, low, pi - 1)
        quick_sort(arr, pi + 1, high)

# Sử dụng
arr = [10, 7, 8, 9, 1, 5]
quick_sort(arr, 0, len(arr) - 1)
print("Mảng đã sắp xếp:", arr)
```

## Ví dụ minh họa

Sắp xếp mảng: `[8, 3, 1, 7, 0, 10, 2]`

**Bước 1:** Chọn pivot = 2 (phần tử cuối)
```
[8, 3, 1, 7, 0, 10, 2]
            ^
         pivot
```

**Bước 2:** Phân hoạch
```
[1, 0, 2, 7, 3, 10, 8]
       ^
    pivot ở vị trí đúng
```

**Bước 3:** Đệ quy cho mảng con trái `[1, 0]` và phải `[7, 3, 10, 8]`

**Tiếp tục** cho đến khi toàn bộ mảng được sắp xếp:
```
[0, 1, 2, 3, 7, 8, 10]
```

## Tối ưu hóa

### 1. Chọn Pivot tốt hơn
- **Median-of-Three:** Chọn giá trị trung vị của ba phần tử (đầu, giữa, cuối)
```cpp
int medianOfThree(vector<int>& arr, int low, int high) {
    int mid = low + (high - low) / 2;

    if (arr[low] > arr[mid]) swap(arr[low], arr[mid]);
    if (arr[low] > arr[high]) swap(arr[low], arr[high]);
    if (arr[mid] > arr[high]) swap(arr[mid], arr[high]);

    return mid;
}
```

### 2. Chuyển sang Insertion Sort cho mảng nhỏ
```cpp
const int THRESHOLD = 10;

void quickSort(vector<int>& arr, int low, int high) {
    if (high - low < THRESHOLD) {
        insertionSort(arr, low, high);
        return;
    }

    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}
```

### 3. Tail Recursion Optimization
```cpp
void quickSort(vector<int>& arr, int low, int high) {
    while (low < high) {
        int pi = partition(arr, low, high);

        // Đệ quy cho phần nhỏ hơn, lặp cho phần lớn hơn
        if (pi - low < high - pi) {
            quickSort(arr, low, pi - 1);
            low = pi + 1;
        } else {
            quickSort(arr, pi + 1, high);
            high = pi - 1;
        }
    }
}
```

## Ưu điểm

1. **Hiệu quả trong thực tế:** Mặc dù worst case là O(n²), nhưng trung bình Quick Sort rất nhanh
2. **In-place:** Chỉ cần O(log n) bộ nhớ cho stack đệ quy
3. **Cache-friendly:** Tốt với bộ nhớ cache do truy cập tuần tự
4. **Dễ cài đặt:** Code ngắn gọn và dễ hiểu

## Nhược điểm

1. **Không ổn định:** Thứ tự tương đối của các phần tử bằng nhau có thể thay đổi
2. **Worst case O(n²):** Khi mảng đã sắp xếp hoặc tất cả phần tử bằng nhau
3. **Đệ quy:** Có thể gây stack overflow với mảng rất lớn
4. **Không phù hợp với dữ liệu đặc biệt:** Như mảng đã sắp xếp hoặc có nhiều phần tử trùng

## So sánh với các thuật toán khác

| Thuật toán | Trung bình | Worst Case | Stable | In-place |
|------------|-----------|-----------|--------|----------|
| Quick Sort | O(n log n) | O(n²) | Không | Có |
| Merge Sort | O(n log n) | O(n log n) | Có | Không |
| Heap Sort | O(n log n) | O(n log n) | Không | Có |

## Khi nào sử dụng Quick Sort?

✅ **Nên dùng khi:**
- Cần thuật toán sắp xếp nhanh trong trường hợp trung bình
- Bộ nhớ hạn chế (in-place)
- Dữ liệu ngẫu nhiên hoặc không có pattern đặc biệt
- Không yêu cầu tính ổn định

❌ **Không nên dùng khi:**
- Cần đảm bảo O(n log n) trong mọi trường hợp (dùng Merge Sort hoặc Heap Sort)
- Cần thuật toán ổn định (dùng Merge Sort)
- Dữ liệu đã gần như sắp xếp (dùng Insertion Sort)

## Bài tập áp dụng

### Cơ bản
1. Cài đặt Quick Sort và test với các trường hợp: mảng ngẫu nhiên, mảng đã sắp xếp, mảng ngược
2. Đếm số lần so sánh và swap trong quá trình sắp xếp
3. Cài đặt Quick Sort với cả ba cách chọn pivot: đầu, cuối, giữa

### Nâng cao
1. **K-th Element:** Tìm phần tử thứ k nhỏ nhất sử dụng QuickSelect (O(n) trung bình)
2. **3-Way Partition:** Cài đặt Quick Sort cho mảng có nhiều phần tử trùng lặp
3. **Dual-Pivot Quick Sort:** Cài đặt với hai pivot
4. **Iterative Quick Sort:** Cài đặt không dùng đệ quy

### Bài tập trên OJ
- VNOI - SORT: Bài toán sắp xếp cơ bản
- Codeforces - Kth Order Statistic
- SPOJ - QSORT: Quick Sort Practice

## Tài liệu tham khảo

- [VNOI Wiki - Quick Sort](https://vnoi.info/wiki/algo/basic/sorting.md)
- Introduction to Algorithms (CLRS) - Chapter 7
- [Visualgo - Quick Sort Visualization](https://visualgo.net/en/sorting)

## Mẹo trong lập trình thi đấu

1. **Sử dụng STL:** Trong C++, `sort()` của STL đã tối ưu rất tốt (IntroSort = Quick Sort + Heap Sort)
   ```cpp
   sort(arr.begin(), arr.end());
   ```

2. **Random Pivot:** Luôn randomize pivot để tránh worst case
   ```cpp
   srand(time(0));
   ```

3. **Kết hợp với Insertion Sort:** Chuyển sang Insertion Sort khi mảng con nhỏ (< 10-20 phần tử)

4. **3-Way Partition:** Khi có nhiều phần tử trùng lặp, sử dụng 3-way partition sẽ hiệu quả hơn
