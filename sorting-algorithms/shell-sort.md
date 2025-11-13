# Shell Sort (Sắp xếp Shell)

## Giới thiệu

Shell Sort là một cải tiến thông minh của **Insertion Sort**, được phát minh bởi Donald Shell năm 1959. Đây là thuật toán sắp xếp dựa trên so sánh đầu tiên phá vỡ rào cản O(n²) của các thuật toán đơn giản.

Ý tưởng chính của Shell Sort là:
- Thay vì so sánh các phần tử liền kề (gap = 1) như Insertion Sort
- So sánh và sắp xếp các phần tử cách nhau một khoảng **gap** lớn hơn
- Giảm dần gap cho đến khi gap = 1 (Insertion Sort thông thường)

Khi gap lớn được xử lý trước, mảng trở nên "gần như đã sắp xếp", khiến bước cuối cùng (gap = 1) rất nhanh.

## Độ phức tạp

Độ phức tạp của Shell Sort **phụ thuộc vào chuỗi gap** được chọn:

| Chuỗi Gap | Độ phức tạp | Người đề xuất |
|-----------|-------------|---------------|
| n/2, n/4, ..., 1 | O(n²) | Shell (1959) |
| 2^k - 1 | O(n^(3/2)) | - |
| 2^p × 3^q | O(n log² n) | Pratt (1971) |
| (3^k - 1)/2 | O(n^(3/2)) | Knuth (1973) |
| 4^k + 3×2^(k-1) + 1 | O(n^(4/3)) | Sedgewick (1986) |
| Unknown | O(n log n) ??? | Chưa chứng minh |

- **Không gian:** O(1)
- **Tính ổn định:** Không ổn định (unstable)
- **In-place:** Có
- **Adaptive:** Có

## Ý tưởng thuật toán

1. Chọn chuỗi gap: h_k, h_(k-1), ..., h_1 = 1
2. Với mỗi gap h:
   - Chia mảng thành các nhóm (mỗi nhóm gồm các phần tử cách nhau h)
   - Áp dụng Insertion Sort cho từng nhóm
3. Giảm gap cho đến khi gap = 1

**Ví dụ với gap sequence: [5, 2, 1]**
```
Original: [35, 33, 42, 10, 14, 19, 27, 44]

Gap = 5: Sắp xếp {35,19}, {33,27}, {42,44}, {10}, {14}
Result:  [19, 27, 42, 10, 14, 35, 33, 44]

Gap = 2: Sắp xếp {19,42,14,33}, {27,10,35,44}
Result:  [14, 10, 19, 27, 33, 35, 42, 44]

Gap = 1: Insertion Sort thông thường (gần như đã sắp xếp)
Result:  [10, 14, 19, 27, 33, 35, 42, 44]
```

## Cài đặt

### C++ - Shell's Original Sequence (n/2^k)

```cpp
#include <bits/stdc++.h>
using namespace std;

void shellSort(vector<int>& arr) {
    int n = arr.size();

    // Bắt đầu với gap lớn, giảm dần
    for (int gap = n / 2; gap > 0; gap /= 2) {
        // Insertion Sort với gap
        for (int i = gap; i < n; i++) {
            int temp = arr[i];
            int j = i;

            // Shift các phần tử cách gap
            while (j >= gap && arr[j - gap] > temp) {
                arr[j] = arr[j - gap];
                j -= gap;
            }

            arr[j] = temp;
        }
    }
}

int main() {
    vector<int> arr = {35, 33, 42, 10, 14, 19, 27, 44};

    shellSort(arr);

    cout << "Mang da sap xep: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### C++ - Knuth's Sequence (3^k - 1)/2

```cpp
#include <bits/stdc++.h>
using namespace std;

void shellSortKnuth(vector<int>& arr) {
    int n = arr.size();

    // Tính gap sequence: 1, 4, 13, 40, 121, 364, ...
    // h = 3*h + 1
    int h = 1;
    while (h < n / 3) {
        h = 3 * h + 1;
    }

    // Giảm dần gap
    while (h >= 1) {
        // h-sort array
        for (int i = h; i < n; i++) {
            int temp = arr[i];
            int j = i;

            while (j >= h && arr[j - h] > temp) {
                arr[j] = arr[j - h];
                j -= h;
            }

            arr[j] = temp;
        }

        h /= 3;
    }
}

int main() {
    vector<int> arr = {35, 33, 42, 10, 14, 19, 27, 44};
    shellSortKnuth(arr);

    cout << "Mang da sap xep: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### C++ - Sedgewick's Sequence

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> sedgewickSequence(int n) {
    // 1, 5, 19, 41, 109, 209, 505, 929, 2161, ...
    // Gap_k = 9×4^k - 9×2^k + 1 hoặc 4^k - 3×2^k + 1
    vector<int> gaps;

    int k = 0;
    while (true) {
        int gap1 = (1 << (2 * k + 2)) - 3 * (1 << (k + 1)) + 1;  // 4^(k+1) - 3×2^(k+1) + 1
        if (gap1 >= n) break;
        gaps.push_back(gap1);

        int gap2 = 9 * (1 << (2 * k)) - 9 * (1 << k) + 1;  // 9×4^k - 9×2^k + 1
        if (gap2 >= n) break;
        gaps.push_back(gap2);

        k++;
    }

    reverse(gaps.begin(), gaps.end());
    if (gaps.empty() || gaps.back() != 1) {
        gaps.push_back(1);
    }

    return gaps;
}

void shellSortSedgewick(vector<int>& arr) {
    int n = arr.size();
    vector<int> gaps = sedgewickSequence(n);

    for (int gap : gaps) {
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

int main() {
    vector<int> arr = {35, 33, 42, 10, 14, 19, 27, 44};
    shellSortSedgewick(arr);

    cout << "Mang da sap xep: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### C++ - Ciura's Sequence (thực nghiệm tốt nhất)

```cpp
#include <bits/stdc++.h>
using namespace std;

void shellSortCiura(vector<int>& arr) {
    int n = arr.size();

    // Chuỗi Ciura (thực nghiệm): 1, 4, 10, 23, 57, 132, 301, 701, 1750
    vector<int> gaps = {701, 301, 132, 57, 23, 10, 4, 1};

    // Chọn gaps phù hợp với kích thước mảng
    int startIdx = 0;
    while (startIdx < gaps.size() && gaps[startIdx] >= n) {
        startIdx++;
    }

    for (int idx = startIdx; idx < gaps.size(); idx++) {
        int gap = gaps[idx];

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

int main() {
    vector<int> arr = {35, 33, 42, 10, 14, 19, 27, 44};
    shellSortCiura(arr);

    cout << "Mang da sap xep: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### Python

```python
def shell_sort(arr):
    n = len(arr)
    gap = n // 2

    while gap > 0:
        for i in range(gap, n):
            temp = arr[i]
            j = i

            while j >= gap and arr[j - gap] > temp:
                arr[j] = arr[j - gap]
                j -= gap

            arr[j] = temp

        gap //= 2

    return arr

# Sử dụng
arr = [35, 33, 42, 10, 14, 19, 27, 44]
shell_sort(arr)
print("Mảng đã sắp xếp:", arr)
```

### Python - Knuth's Sequence

```python
def shell_sort_knuth(arr):
    n = len(arr)

    # Tính gap sequence
    h = 1
    while h < n // 3:
        h = 3 * h + 1

    while h >= 1:
        for i in range(h, n):
            temp = arr[i]
            j = i

            while j >= h and arr[j - h] > temp:
                arr[j] = arr[j - h]
                j -= h

            arr[j] = temp

        h //= 3

    return arr
```

## Ví dụ minh họa chi tiết

Sắp xếp mảng: `[35, 33, 42, 10, 14, 19, 27, 44]` với gap sequence `[4, 2, 1]`

**Gap = 4:**
```
Các nhóm:
Nhóm 0: [35, 14] -> [14, 35]
Nhóm 1: [33, 19] -> [19, 33]
Nhóm 2: [42, 27] -> [27, 42]
Nhóm 3: [10, 44] -> [10, 44]

Kết quả: [14, 19, 27, 10, 35, 33, 42, 44]
```

**Gap = 2:**
```
Các nhóm:
Nhóm 0: [14, 27, 35, 42] -> [14, 27, 35, 42]
Nhóm 1: [19, 10, 33, 44] -> [10, 19, 33, 44]

Kết quả: [14, 10, 27, 19, 35, 33, 42, 44]
```

**Gap = 1 (Insertion Sort):**
```
Mảng đã gần như sắp xếp: [14, 10, 27, 19, 35, 33, 42, 44]

Chỉ cần vài swap:
[10, 14, 19, 27, 33, 35, 42, 44]
```

## Phân tích các Gap Sequence

### 1. Shell's Original (n/2^k)

```
Gap: n/2, n/4, n/8, ..., 1
```

**Ưu điểm:**
- Đơn giản, dễ cài đặt
- Dễ hiểu

**Nhược điểm:**
- O(n²) worst case
- Không tối ưu

### 2. Knuth's Sequence (3^k - 1)/2

```
Gap: 1, 4, 13, 40, 121, 364, 1093, ...
h_k = 3 × h_(k-1) + 1
```

**Ưu điểm:**
- O(n^(3/2)) đã được chứng minh
- Dễ tính toán
- Phổ biến trong thực tế

**Nhược điểm:**
- Không phải tối ưu nhất

### 3. Sedgewick's Sequence

```
Gap: 1, 5, 19, 41, 109, 209, 505, 929, ...
```

**Ưu điểm:**
- O(n^(4/3)) average case
- Tốt trong thực tế

**Nhược điểm:**
- Phức tạp để tính

### 4. Ciura's Sequence (thực nghiệm)

```
Gap: 1, 4, 10, 23, 57, 132, 301, 701, 1750, ...
```

**Ưu điểm:**
- Tốt nhất trong thực nghiệm (cho n ≤ 4000)
- Đơn giản: hardcode các giá trị

**Nhược điểm:**
- Không có công thức tổng quát
- Chưa được chứng minh lý thuyết

## Tối ưu hóa

### 1. Binary Insertion trong từng pass

```cpp
void shellSortOptimized(vector<int>& arr) {
    int n = arr.size();

    for (int gap = n / 2; gap > 0; gap /= 2) {
        for (int i = gap; i < n; i++) {
            int temp = arr[i];

            // Binary search để tìm vị trí chèn
            int left = (i / gap) * gap;
            int right = i;
            int pos = i;

            while (left < right) {
                int mid = left + ((right - left) / gap / 2) * gap;
                if (arr[mid] > temp) {
                    right = mid;
                } else {
                    left = mid + gap;
                }
            }

            // Shift
            for (int j = i; j > left; j -= gap) {
                arr[j] = arr[j - gap];
            }
            arr[left] = temp;
        }
    }
}
```

### 2. Adaptive Gap Selection

```cpp
void adaptiveShellSort(vector<int>& arr) {
    int n = arr.size();

    // Chọn gap sequence dựa trên kích thước
    vector<int> gaps;
    if (n < 100) {
        gaps = {4, 2, 1};  // Đơn giản cho mảng nhỏ
    } else {
        // Knuth's sequence cho mảng lớn
        int h = 1;
        while (h < n / 3) h = 3 * h + 1;
        while (h >= 1) {
            gaps.push_back(h);
            h /= 3;
        }
    }

    // Sort
    for (int gap : gaps) {
        // ... insertion sort với gap
    }
}
```

## Ưu điểm

1. **Nhanh hơn O(n²) basic sorts:** Đặc biệt với gap sequence tốt
2. **Code đơn giản:** Chỉ cần thêm vòng lặp gap vào Insertion Sort
3. **In-place:** Chỉ cần O(1) bộ nhớ
4. **Adaptive:** Hiệu quả với mảng đã gần sắp xếp
5. **Tốt cho mảng vừa:** Hiệu quả với n = 1000-10000
6. **Không cần đệ quy:** Tránh stack overflow

## Nhược điểm

1. **Không stable:** Thứ tự các phần tử bằng nhau có thể thay đổi
2. **Phức tạp khó chứng minh:** Độ phức tạp chính xác chưa rõ
3. **Phụ thuộc gap:** Hiệu suất phụ thuộc nhiều vào chuỗi gap
4. **Không optimal:** Vẫn chậm hơn O(n log n) algorithms cho mảng lớn

## So sánh với các thuật toán khác

| Thuật toán | Độ phức tạp | Space | Stable | Code |
|------------|-------------|-------|--------|------|
| Shell Sort | O(n^(3/2)) - O(n²) | O(1) | Không | Trung bình |
| Insertion Sort | O(n²) | O(1) | Có | Đơn giản |
| Quick Sort | O(n log n) | O(log n) | Không | Phức tạp |
| Merge Sort | O(n log n) | O(n) | Có | Phức tạp |

## Khi nào sử dụng Shell Sort?

✅ **Nên dùng khi:**
- Mảng kích thước trung bình (1000-10000)
- Cần thuật toán in-place
- Không cần stable sort
- Muốn code đơn giản hơn Quick Sort
- Hệ thống embedded (tránh đệ quy)
- Mảng đã gần sắp xếp

❌ **Không nên dùng khi:**
- Mảng rất lớn (> 100000) - dùng Quick/Merge Sort
- Cần stable sort - dùng Merge Sort
- Cần đảm bảo O(n log n) - dùng Heap/Merge Sort
- Mảng rất nhỏ (< 50) - dùng Insertion Sort

## Ứng dụng thực tế

### 1. Embedded Systems

```cpp
// Tránh đệ quy trong hệ thống embedded
void embeddedSort(int arr[], int n) {
    // Shell Sort không cần đệ quy
    for (int gap = n / 2; gap > 0; gap /= 2) {
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

### 2. Preprocessing cho Quick Sort

```cpp
void hybridSort(vector<int>& arr, int left, int right) {
    int n = right - left + 1;

    if (n < 50) {
        // Shell Sort cho mảng nhỏ
        shellSortRange(arr, left, right);
    } else {
        // Quick Sort cho mảng lớn
        int pivot = partition(arr, left, right);
        hybridSort(arr, left, pivot - 1);
        hybridSort(arr, pivot + 1, right);
    }
}
```

### 3. External Sorting

```cpp
// Shell Sort tốt cho external sorting vì ít swap
void externalShellSort(vector<LargeRecord>& records) {
    // Với records lớn, số lần swap ít quan trọng
    shellSort(records);
}
```

## Bài tập áp dụng

### Cơ bản
1. Cài đặt Shell Sort với các gap sequence khác nhau
2. So sánh tốc độ với Insertion Sort
3. Đếm số lần so sánh và swap

### Nâng cao
1. **Optimal Gap Sequence:** Tìm gap sequence tốt nhất cho n cho trước
2. **Adaptive Shell Sort:** Điều chỉnh gap dựa trên độ "unsorted"
3. **Parallel Shell Sort:** Song song hóa các nhóm h-sort

### Bài tập trên OJ
- SPOJ - SHSORT: Shell Sort Practice
- CSES - Sorting Algorithms Comparison
- UVa - Shell Sort Problems

## Lịch sử và nghiên cứu

### Timeline

- **1959:** Donald Shell giới thiệu thuật toán với gap n/2
- **1971:** Pratt chứng minh O(n log² n) với gap sequence đặc biệt
- **1973:** Knuth đề xuất sequence (3^k - 1)/2
- **1986:** Sedgewick cải thiện lên O(n^(4/3))
- **2001:** Ciura tìm sequence tốt nhất thực nghiệm

### Open Problem

**Conjecture:** Có tồn tại gap sequence cho Shell Sort với O(n log n)?
- Chưa có ai chứng minh được
- Cũng chưa có ai chứng minh là không tồn tại

## Tài liệu tham khảo

- [VNOI Wiki - Shell Sort](https://vnoi.info/wiki/algo/basic/sorting.md)
- Donald Shell (1959) - "A High-Speed Sorting Procedure"
- Donald Knuth - The Art of Computer Programming Vol. 3
- Robert Sedgewick (1986) - "A New Upper Bound for Shellsort"
- [Visualgo - Shell Sort Visualization](https://visualgo.net/en/sorting)

## Mẹo trong lập trình thi đấu

1. **Không dùng trong contests:** Luôn dùng `sort()` của STL
   ```cpp
   sort(arr.begin(), arr.end());  // Quick Sort hybrid
   ```

2. **Giáo dục:** Tốt để học về thuật toán lai (hybrid algorithms)

3. **Gap sequence:** Trong thực tế, Ciura's sequence tốt nhất:
   ```cpp
   vector<int> gaps = {701, 301, 132, 57, 23, 10, 4, 1};
   ```

4. **Benchmark:** So sánh hiệu suất của các gap sequences

5. **Embedded:** Khi không thể dùng đệ quy, Shell Sort là lựa chọn tốt

## Kết luận

Shell Sort là một thuật toán thú vị:
- **Lịch sử:** Thuật toán cải tiến đầu tiên vượt qua O(n²)
- **Thực tế:** Vẫn được dùng trong một số trường hợp đặc biệt
- **Học thuật:** Nghiên cứu về gap sequence vẫn đang tiếp tục
- **Contest:** Không dùng trong lập trình thi đấu (dùng STL sort)

Shell Sort là minh chứng cho việc một ý tưởng đơn giản (tăng gap) có thể cải thiện đáng kể hiệu suất!
