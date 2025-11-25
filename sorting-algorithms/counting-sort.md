# Counting Sort (Sắp xếp đếm phân phối)

## Giới thiệu

Counting Sort là một thuật toán sắp xếp **không dựa trên so sánh** (non-comparison sort). Thay vì so sánh các phần tử với nhau, Counting Sort đếm số lần xuất hiện của mỗi giá trị và sử dụng thông tin này để đặt các phần tử vào đúng vị trí.

Counting Sort rất hiệu quả khi **khoảng giá trị của các phần tử không quá lớn** so với số lượng phần tử cần sắp xếp.

## Độ phức tạp

- **Thời gian:**
  - Trung bình: O(n + k)
  - Tốt nhất: O(n + k)
  - Xấu nhất: O(n + k)
  - Trong đó: n là số phần tử, k là khoảng giá trị (max - min + 1)
- **Không gian:** O(n + k)
- **Tính ổn định:** Ổn định (stable)
- **In-place:** Không

## Ý tưởng thuật toán

1. **Tìm min và max:** Xác định khoảng giá trị
2. **Đếm tần suất:** Đếm số lần xuất hiện của mỗi giá trị
3. **Tính tổng tích lũy:** Biến đổi mảng đếm thành mảng vị trí
4. **Xây dựng kết quả:** Đặt các phần tử vào đúng vị trí

## Cài đặt

### C++ - Phiên bản cơ bản (cho số không âm)

```cpp
#include <bits/stdc++.h>
using namespace std;

void countingSort(vector<int>& arr) {
    if (arr.empty()) return;

    // Tìm giá trị lớn nhất
    int maxVal = *max_element(arr.begin(), arr.end());

    // Mảng đếm
    vector<int> count(maxVal + 1, 0);

    // Đếm tần suất
    for (int x : arr) {
        count[x]++;
    }

    // Xây dựng lại mảng
    int idx = 0;
    for (int i = 0; i <= maxVal; i++) {
        for (int j = 0; j < count[i]; j++) {
            arr[idx++] = i;
        }
    }
}

int main() {
    vector<int> arr = {4, 2, 2, 8, 3, 3, 1};

    countingSort(arr);

    cout << "Mang da sap xep: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### C++ - Phiên bản ổn định (Stable Counting Sort)

```cpp
#include <bits/stdc++.h>
using namespace std;

void countingSortStable(vector<int>& arr) {
    if (arr.empty()) return;

    int n = arr.size();
    int maxVal = *max_element(arr.begin(), arr.end());
    int minVal = *min_element(arr.begin(), arr.end());
    int range = maxVal - minVal + 1;

    // Mảng đếm và mảng kết quả
    vector<int> count(range, 0);
    vector<int> output(n);

    // Đếm tần suất
    for (int i = 0; i < n; i++) {
        count[arr[i] - minVal]++;
    }

    // Tính tổng tích lũy (cumulative sum)
    // count[i] giờ chứa vị trí cuối cùng của giá trị i
    for (int i = 1; i < range; i++) {
        count[i] += count[i - 1];
    }

    // Xây dựng mảng kết quả (duyệt ngược để đảm bảo stable)
    for (int i = n - 1; i >= 0; i--) {
        output[count[arr[i] - minVal] - 1] = arr[i];
        count[arr[i] - minVal]--;
    }

    // Copy kết quả về mảng gốc
    arr = output;
}

int main() {
    vector<int> arr = {4, 2, 2, 8, 3, 3, 1};

    countingSortStable(arr);

    cout << "Mang da sap xep: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### C++ - Xử lý số âm

```cpp
#include <bits/stdc++.h>
using namespace std;

void countingSortWithNegative(vector<int>& arr) {
    if (arr.empty()) return;

    int n = arr.size();
    int minVal = *min_element(arr.begin(), arr.end());
    int maxVal = *max_element(arr.begin(), arr.end());
    int range = maxVal - minVal + 1;

    vector<int> count(range, 0);
    vector<int> output(n);

    // Đếm tần suất (dịch chuyển index bằng cách trừ minVal)
    for (int i = 0; i < n; i++) {
        count[arr[i] - minVal]++;
    }

    // Tính tổng tích lũy
    for (int i = 1; i < range; i++) {
        count[i] += count[i - 1];
    }

    // Xây dựng mảng kết quả
    for (int i = n - 1; i >= 0; i--) {
        output[count[arr[i] - minVal] - 1] = arr[i];
        count[arr[i] - minVal]--;
    }

    arr = output;
}

int main() {
    vector<int> arr = {-5, -10, 0, -3, 8, 5, -1, 10};

    countingSortWithNegative(arr);

    cout << "Mang da sap xep: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### C++ - Sắp xếp theo digit (cho Radix Sort)

```cpp
#include <bits/stdc++.h>
using namespace std;

void countingSortByDigit(vector<int>& arr, int exp) {
    int n = arr.size();
    vector<int> output(n);
    vector<int> count(10, 0);

    // Đếm tần suất của các chữ số
    for (int i = 0; i < n; i++) {
        count[(arr[i] / exp) % 10]++;
    }

    // Tính tổng tích lũy
    for (int i = 1; i < 10; i++) {
        count[i] += count[i - 1];
    }

    // Xây dựng mảng kết quả
    for (int i = n - 1; i >= 0; i--) {
        int digit = (arr[i] / exp) % 10;
        output[count[digit] - 1] = arr[i];
        count[digit]--;
    }

    arr = output;
}

int main() {
    vector<int> arr = {170, 45, 75, 90, 802, 24, 2, 66};
    countingSortByDigit(arr, 1);  // Sắp xếp theo hàng đơn vị

    cout << "Mang da sap xep: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### Python

```python
def counting_sort(arr):
    if not arr:
        return arr

    # Tìm min và max
    min_val = min(arr)
    max_val = max(arr)
    range_size = max_val - min_val + 1

    # Mảng đếm và output
    count = [0] * range_size
    output = [0] * len(arr)

    # Đếm tần suất
    for num in arr:
        count[num - min_val] += 1

    # Tính tổng tích lũy
    for i in range(1, range_size):
        count[i] += count[i - 1]

    # Xây dựng mảng kết quả (ngược để stable)
    for i in range(len(arr) - 1, -1, -1):
        output[count[arr[i] - min_val] - 1] = arr[i]
        count[arr[i] - min_val] -= 1

    return output

# Sử dụng
arr = [4, 2, 2, 8, 3, 3, 1]
sorted_arr = counting_sort(arr)
print("Mảng đã sắp xếp:", sorted_arr)
```

## Ví dụ minh họa

Sắp xếp mảng: `[2, 5, 3, 0, 2, 3, 0, 3]`

**Bước 1: Đếm tần suất**
```
Giá trị: 0  1  2  3  4  5
Tần suất: 2  0  2  3  0  1

count = [2, 0, 2, 3, 0, 1]
```

**Bước 2: Tính tổng tích lũy**
```
count = [2, 2, 4, 7, 7, 8]
(Mỗi phần tử = tổng của nó và các phần tử trước)
```

**Bước 3: Xây dựng mảng kết quả**
```
Duyệt ngược arr = [2, 5, 3, 0, 2, 3, 0, 3]

i=7: arr[7]=3, count[3]=7 → output[6]=3, count[3]=6
i=6: arr[6]=0, count[0]=2 → output[1]=0, count[0]=1
i=5: arr[5]=3, count[3]=6 → output[5]=3, count[3]=5
i=4: arr[4]=2, count[2]=4 → output[3]=2, count[2]=3
i=3: arr[3]=0, count[0]=1 → output[0]=0, count[0]=0
i=2: arr[2]=3, count[3]=5 → output[4]=3, count[3]=4
i=1: arr[1]=5, count[5]=8 → output[7]=5, count[5]=7
i=0: arr[0]=2, count[2]=3 → output[2]=2, count[2]=2

Kết quả: [0, 0, 2, 2, 3, 3, 3, 5]
```

## Phân tích chi tiết

### Tại sao phải duyệt ngược để có stable sort?

Khi duyệt ngược, các phần tử có cùng giá trị sẽ giữ nguyên thứ tự tương đối:
- Phần tử xuất hiện sau trong mảng gốc sẽ được xử lý trước
- Do count[x]-- sau mỗi lần đặt, phần tử xuất hiện trước sẽ được đặt ở vị trí trước

### Tại sao cần stable?

Stable sort quan trọng khi:
- Sắp xếp theo nhiều tiêu chí (sort theo A rồi sort theo B)
- Làm bước trung gian cho Radix Sort
- Giữ nguyên thứ tự ban đầu của các phần tử bằng nhau

## Tối ưu hóa

### 1. Giảm bộ nhớ với range nhỏ

```cpp
void countingSortOptimized(vector<int>& arr) {
    if (arr.empty()) return;

    int minVal = *min_element(arr.begin(), arr.end());
    int maxVal = *max_element(arr.begin(), arr.end());

    // Chỉ cần range = maxVal - minVal + 1
    int range = maxVal - minVal + 1;
    vector<int> count(range, 0);

    for (int x : arr) {
        count[x - minVal]++;
    }

    int idx = 0;
    for (int i = 0; i < range; i++) {
        while (count[i]-- > 0) {
            arr[idx++] = i + minVal;
        }
    }
}
```

### 2. In-place cho trường hợp đặc biệt

```cpp
// Chỉ hoạt động khi arr chứa hoán vị của 0..n-1
void cycleSortPermutation(vector<int>& arr) {
    for (int i = 0; i < arr.size(); ) {
        if (arr[i] == i) {
            i++;
        } else {
            swap(arr[i], arr[arr[i]]);
        }
    }
}
```

## Ưu điểm

1. **Rất nhanh:** O(n + k) tuyến tính khi k không quá lớn
2. **Ổn định:** Giữ nguyên thứ tự tương đối
3. **Đơn giản:** Dễ cài đặt và hiểu
4. **Deterministic:** Không phụ thuộc vào dữ liệu

## Nhược điểm

1. **Giới hạn phạm vi:** Chỉ hiệu quả khi k = O(n)
2. **Tốn bộ nhớ:** Cần O(n + k) bộ nhớ
3. **Không in-place:** Cần mảng phụ
4. **Chỉ cho số nguyên:** Không áp dụng cho số thực, chuỗi

## So sánh với các thuật toán khác

| Thuật toán | Thời gian | Space | Stable | Điều kiện |
|------------|-----------|-------|--------|-----------|
| Counting Sort | O(n + k) | O(n + k) | Có | k = O(n) |
| Radix Sort | O(d(n + k)) | O(n + k) | Có | d chữ số |
| Quick Sort | O(n log n) | O(log n) | Không | General |
| Merge Sort | O(n log n) | O(n) | Có | General |

## Khi nào sử dụng Counting Sort?

✅ **Nên dùng khi:**
- Các phần tử là số nguyên trong khoảng nhỏ
- k (range) = O(n) hoặc nhỏ hơn
- Cần thuật toán ổn định
- Cần tốc độ tuyến tính O(n)
- Làm bước trung gian cho Radix Sort

❌ **Không nên dùng khi:**
- k >> n (range quá lớn so với số phần tử)
- Dữ liệu không phải số nguyên
- Bộ nhớ hạn chế
- Không biết trước khoảng giá trị

## Ứng dụng thực tế

### 1. Sắp xếp theo độ tuổi

```cpp
void sortByAge(vector<Person>& people) {
    const int MAX_AGE = 150;
    vector<vector<Person>> count(MAX_AGE + 1);

    // Đếm theo tuổi
    for (const auto& p : people) {
        count[p.age].push_back(p);
    }

    // Xây dựng lại mảng
    int idx = 0;
    for (int age = 0; age <= MAX_AGE; age++) {
        for (const auto& p : count[age]) {
            people[idx++] = p;
        }
    }
}
```

### 2. Sắp xếp theo điểm thi

```cpp
void sortByScore(vector<Student>& students) {
    const int MAX_SCORE = 100;
    vector<vector<Student>> count(MAX_SCORE + 1);

    for (const auto& s : students) {
        count[s.score].push_back(s);
    }

    int idx = 0;
    for (int score = 0; score <= MAX_SCORE; score++) {
        for (const auto& s : count[score]) {
            students[idx++] = s;
        }
    }
}
```

### 3. Tìm phần tử xuất hiện nhiều nhất

```cpp
int findMostFrequent(vector<int>& arr) {
    int minVal = *min_element(arr.begin(), arr.end());
    int maxVal = *max_element(arr.begin(), arr.end());
    int range = maxVal - minVal + 1;

    vector<int> count(range, 0);

    int maxCount = 0, result = 0;
    for (int x : arr) {
        count[x - minVal]++;
        if (count[x - minVal] > maxCount) {
            maxCount = count[x - minVal];
            result = x;
        }
    }

    return result;
}
```

### 4. Sắp xếp màu (Dutch National Flag)

```cpp
// Sắp xếp mảng chỉ gồm 0, 1, 2
void sortColors(vector<int>& arr) {
    vector<int> count(3, 0);

    for (int x : arr) {
        count[x]++;
    }

    int idx = 0;
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < count[i]; j++) {
            arr[idx++] = i;
        }
    }
}

// Hoặc dùng 3-way partition (in-place)
void sortColorsInPlace(vector<int>& arr) {
    int low = 0, mid = 0, high = arr.size() - 1;

    while (mid <= high) {
        if (arr[mid] == 0) {
            swap(arr[low++], arr[mid++]);
        } else if (arr[mid] == 1) {
            mid++;
        } else {
            swap(arr[mid], arr[high--]);
        }
    }
}
```

## Bài tập áp dụng

### Cơ bản
1. Cài đặt Counting Sort và test với các trường hợp khác nhau
2. Đếm số lần xuất hiện của mỗi phần tử
3. Tìm phần tử xuất hiện nhiều nhất/ít nhất

### Nâng cao
1. **Sort Colors:** Sắp xếp mảng gồm 0, 1, 2
2. **Sort by Frequency:** Sắp xếp theo tần suất xuất hiện
3. **Range Sum Queries:** Đếm số phần tử trong khoảng [L, R]
4. **Maximum Gap:** Tìm khoảng cách lớn nhất giữa hai phần tử liền kề sau khi sắp xếp

### Bài tập trên OJ
- LeetCode 75 - Sort Colors
- LeetCode 451 - Sort Characters By Frequency
- CSES - Distinct Numbers
- VNOI - SORTING: Counting Sort Practice

## Biến thể và mở rộng

### 1. Bucket Sort

Tổng quát hóa của Counting Sort cho range lớn:
```cpp
void bucketSort(vector<float>& arr) {
    int n = arr.size();
    vector<vector<float>> buckets(n);

    // Phân phối vào các bucket
    for (float x : arr) {
        int idx = n * x;  // Giả sử arr[i] ∈ [0, 1)
        buckets[idx].push_back(x);
    }

    // Sắp xếp từng bucket
    for (auto& bucket : buckets) {
        sort(bucket.begin(), bucket.end());
    }

    // Gộp lại
    int idx = 0;
    for (auto& bucket : buckets) {
        for (float x : bucket) {
            arr[idx++] = x;
        }
    }
}
```

### 2. Pigeonhole Sort

Tên khác của Counting Sort khi mỗi "lỗ" chứa một giá trị.

## Tài liệu tham khảo

- [VNOI Wiki - Counting Sort](https://vnoi.info/wiki/algo/basic/sorting.md)
- Introduction to Algorithms (CLRS) - Chapter 8
- [Visualgo - Counting Sort Visualization](https://visualgo.net/en/sorting)

## Mẹo trong lập trình thi đấu

1. **Kiểm tra range trước:** Luôn kiểm tra k có <= n không
   ```cpp
   if (maxVal - minVal > n) {
       // Không nên dùng Counting Sort
       sort(arr.begin(), arr.end());
   }
   ```

2. **Xử lý số âm:** Dịch chuyển về 0 bằng cách trừ minVal

3. **Tối ưu bộ nhớ:** Chỉ cấp phát count[range] thay vì count[maxVal + 1]

4. **Kết hợp với map:** Khi range quá lớn nhưng số giá trị khác nhau ít
   ```cpp
   map<int, int> count;
   for (int x : arr) count[x]++;

   int idx = 0;
   for (auto [val, freq] : count) {
       for (int i = 0; i < freq; i++) {
           arr[idx++] = val;
       }
   }
   ```

5. **Parallel Counting:** Có thể song song hóa bước đếm với atomic operations
