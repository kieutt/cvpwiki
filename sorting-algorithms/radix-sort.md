# Radix Sort (Sắp xếp cơ số)

## Giới thiệu

Radix Sort là một thuật toán sắp xếp **không dựa trên so sánh** (non-comparison sort). Thuật toán hoạt động bằng cách sắp xếp các phần tử theo từng **chữ số** (digit) hoặc **ký tự**, từ chữ số ít quan trọng nhất (LSD - Least Significant Digit) đến chữ số quan trọng nhất (MSD - Most Significant Digit), hoặc ngược lại.

Radix Sort thường sử dụng **Counting Sort** hoặc **Bucket Sort** làm thuật toán sắp xếp ổn định cho từng chữ số.

## Độ phức tạp

- **Thời gian:**
  - Trung bình: O(d × (n + k))
  - Tốt nhất: O(d × (n + k))
  - Xấu nhất: O(d × (n + k))
  - Trong đó: d là số chữ số, n là số phần tử, k là cơ số (thường là 10)
- **Không gian:** O(n + k)
- **Tính ổn định:** Ổn định (stable)
- **In-place:** Không

## Hai biến thể chính

### 1. LSD Radix Sort (Least Significant Digit)
- Sắp xếp từ chữ số **ít quan trọng nhất** (hàng đơn vị)
- Đến chữ số **quan trọng nhất** (hàng cao nhất)
- Thường dùng cho số nguyên có độ dài cố định

### 2. MSD Radix Sort (Most Significant Digit)
- Sắp xếp từ chữ số **quan trọng nhất**
- Đến chữ số **ít quan trọng nhất**
- Thường dùng cho chuỗi có độ dài khác nhau

## Cài đặt

### C++ - LSD Radix Sort (cơ số 10)

```cpp
#include <bits/stdc++.h>
using namespace std;

// Hàm Counting Sort theo chữ số tại vị trí exp
void countingSortByDigit(vector<int>& arr, int exp) {
    int n = arr.size();
    vector<int> output(n);
    vector<int> count(10, 0);

    // Đếm tần suất của các chữ số
    for (int i = 0; i < n; i++) {
        int digit = (arr[i] / exp) % 10;
        count[digit]++;
    }

    // Tính tổng tích lũy
    for (int i = 1; i < 10; i++) {
        count[i] += count[i - 1];
    }

    // Xây dựng mảng kết quả (duyệt ngược để stable)
    for (int i = n - 1; i >= 0; i--) {
        int digit = (arr[i] / exp) % 10;
        output[count[digit] - 1] = arr[i];
        count[digit]--;
    }

    // Copy kết quả về mảng gốc
    arr = output;
}

void radixSort(vector<int>& arr) {
    if (arr.empty()) return;

    // Tìm số lớn nhất để biết số chữ số
    int maxVal = *max_element(arr.begin(), arr.end());

    // Sắp xếp theo từng chữ số
    // exp là 10^i (1, 10, 100, 1000, ...)
    for (int exp = 1; maxVal / exp > 0; exp *= 10) {
        countingSortByDigit(arr, exp);
    }
}

int main() {
    vector<int> arr = {170, 45, 75, 90, 802, 24, 2, 66};

    radixSort(arr);

    cout << "Mang da sap xep: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### C++ - Xử lý số âm

```cpp
#include <bits/stdc++.h>
using namespace std;

void countingSortByDigit(vector<int>& arr, int exp) {
    int n = arr.size();
    vector<int> output(n);
    vector<int> count(10, 0);

    for (int i = 0; i < n; i++) {
        int digit = abs(arr[i] / exp) % 10;
        count[digit]++;
    }

    for (int i = 1; i < 10; i++) {
        count[i] += count[i - 1];
    }

    for (int i = n - 1; i >= 0; i--) {
        int digit = abs(arr[i] / exp) % 10;
        output[count[digit] - 1] = arr[i];
        count[digit]--;
    }

    arr = output;
}

void radixSort(vector<int>& arr) {
    if (arr.empty()) return;

    // Tách số âm và số dương
    vector<int> negative, positive;
    for (int x : arr) {
        if (x < 0) negative.push_back(-x);
        else positive.push_back(x);
    }

    // Sắp xếp từng phần
    if (!negative.empty()) {
        int maxVal = *max_element(negative.begin(), negative.end());
        for (int exp = 1; maxVal / exp > 0; exp *= 10) {
            countingSortByDigit(negative, exp);
        }
    }

    if (!positive.empty()) {
        int maxVal = *max_element(positive.begin(), positive.end());
        for (int exp = 1; maxVal / exp > 0; exp *= 10) {
            countingSortByDigit(positive, exp);
        }
    }

    // Gộp lại (âm ngược + dương)
    arr.clear();
    for (int i = negative.size() - 1; i >= 0; i--) {
        arr.push_back(-negative[i]);
    }
    for (int x : positive) {
        arr.push_back(x);
    }
}

int main() {
    vector<int> arr = {170, -45, 75, -90, 802, 24, -2, 66};
    radixSort(arr);

    cout << "Mang da sap xep: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### C++ - Radix Sort với cơ số lớn hơn (base 256)

```cpp
#include <bits/stdc++.h>
using namespace std;

void countingSortByByte(vector<int>& arr, int bytePos) {
    int n = arr.size();
    vector<int> output(n);
    vector<int> count(256, 0);

    // Lấy byte tại vị trí bytePos
    for (int i = 0; i < n; i++) {
        int byte = (arr[i] >> (bytePos * 8)) & 0xFF;
        count[byte]++;
    }

    for (int i = 1; i < 256; i++) {
        count[i] += count[i - 1];
    }

    for (int i = n - 1; i >= 0; i--) {
        int byte = (arr[i] >> (bytePos * 8)) & 0xFF;
        output[count[byte] - 1] = arr[i];
        count[byte]--;
    }

    arr = output;
}

void radixSort256(vector<int>& arr) {
    // Sắp xếp theo 4 byte (32-bit integer)
    for (int bytePos = 0; bytePos < 4; bytePos++) {
        countingSortByByte(arr, bytePos);
    }
}

int main() {
    vector<int> arr = {170, 45, 75, 90, 802, 24, 2, 66};
    radixSort256(arr);

    cout << "Mang da sap xep: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### C++ - MSD Radix Sort

```cpp
#include <bits/stdc++.h>
using namespace std;

void msdRadixSort(vector<int>& arr, int left, int right, int exp) {
    if (left >= right || exp == 0) return;

    vector<vector<int>> buckets(10);

    // Phân phối vào các bucket
    for (int i = left; i <= right; i++) {
        int digit = (arr[i] / exp) % 10;
        buckets[digit].push_back(arr[i]);
    }

    // Gộp lại và đệ quy
    int idx = left;
    for (int digit = 0; digit < 10; digit++) {
        int start = idx;
        for (int val : buckets[digit]) {
            arr[idx++] = val;
        }
        int end = idx - 1;

        // Đệ quy cho bucket này với exp/10
        if (start < end) {
            msdRadixSort(arr, start, end, exp / 10);
        }
    }
}

void msdRadixSortWrapper(vector<int>& arr) {
    if (arr.empty()) return;

    int maxVal = *max_element(arr.begin(), arr.end());

    // Tìm exp lớn nhất (10^k)
    int exp = 1;
    while (maxVal / exp >= 10) {
        exp *= 10;
    }

    msdRadixSort(arr, 0, arr.size() - 1, exp);
}

int main() {
    vector<int> arr = {170, 45, 75, 90, 802, 24, 2, 66};
    msdRadixSortWrapper(arr);

    cout << "Mang da sap xep: ";
    for (int x : arr) cout << x << " ";

    return 0;
}
```

### C++ - Radix Sort cho chuỗi

```cpp
#include <bits/stdc++.h>
using namespace std;

void radixSortStrings(vector<string>& arr) {
    if (arr.empty()) return;

    // Tìm độ dài chuỗi lớn nhất
    int maxLen = 0;
    for (const string& s : arr) {
        maxLen = max(maxLen, (int)s.length());
    }

    // LSD: từ ký tự cuối đến ký tự đầu
    for (int pos = maxLen - 1; pos >= 0; pos--) {
        vector<vector<string>> buckets(256);

        for (const string& s : arr) {
            // Nếu chuỗi ngắn hơn, coi như ký tự = 0
            int charCode = (pos < s.length()) ? (int)s[pos] : 0;
            buckets[charCode].push_back(s);
        }

        // Gộp lại
        int idx = 0;
        for (int i = 0; i < 256; i++) {
            for (const string& s : buckets[i]) {
                arr[idx++] = s;
            }
        }
    }
}

int main() {
    vector<string> arr = {"banana", "apple", "cherry", "date", "fig"};
    radixSortStrings(arr);

    cout << "Mang da sap xep: ";
    for (const string& s : arr) cout << s << " ";

    return 0;
}
```

### Python

```python
def counting_sort_by_digit(arr, exp):
    n = len(arr)
    output = [0] * n
    count = [0] * 10

    # Đếm tần suất
    for i in range(n):
        digit = (arr[i] // exp) % 10
        count[digit] += 1

    # Tính tổng tích lũy
    for i in range(1, 10):
        count[i] += count[i - 1]

    # Xây dựng kết quả
    for i in range(n - 1, -1, -1):
        digit = (arr[i] // exp) % 10
        output[count[digit] - 1] = arr[i]
        count[digit] -= 1

    return output

def radix_sort(arr):
    if not arr:
        return arr

    # Tìm số lớn nhất
    max_val = max(arr)

    # Sắp xếp theo từng chữ số
    exp = 1
    while max_val // exp > 0:
        arr = counting_sort_by_digit(arr, exp)
        exp *= 10

    return arr

# Sử dụng
arr = [170, 45, 75, 90, 802, 24, 2, 66]
sorted_arr = radix_sort(arr)
print("Mảng đã sắp xếp:", sorted_arr)
```

## Ví dụ minh họa

Sắp xếp mảng: `[170, 45, 75, 90, 2, 802, 24, 66]`

**LSD Radix Sort:**

**Bước 1: Sắp xếp theo hàng đơn vị (exp = 1)**
```
170 -> 0
 45 -> 5
 75 -> 5
 90 -> 0
  2 -> 2
802 -> 2
 24 -> 4
 66 -> 6

Kết quả: [170, 90, 2, 802, 24, 45, 75, 66]
```

**Bước 2: Sắp xếp theo hàng chục (exp = 10)**
```
170 -> 7
 90 -> 9
  2 -> 0
802 -> 0
 24 -> 2
 45 -> 4
 75 -> 7
 66 -> 6

Kết quả: [2, 802, 24, 45, 66, 170, 75, 90]
```

**Bước 3: Sắp xếp theo hàng trăm (exp = 100)**
```
  2 -> 0
802 -> 8
 24 -> 0
 45 -> 0
 66 -> 0
170 -> 1
 75 -> 0
 90 -> 0

Kết quả: [2, 24, 45, 66, 75, 90, 170, 802]
```

## Phân tích chi tiết

### Tại sao phải stable?

Tính stable là **bắt buộc** cho Radix Sort:
- Khi sắp xếp chữ số ít quan trọng trước, kết quả phải được giữ nguyên khi sắp xếp chữ số quan trọng hơn
- Nếu không stable, thứ tự các số có chữ số cao giống nhau sẽ bị đảo lộn

### Chọn cơ số (base/radix)

**Cơ số nhỏ (base = 10):**
- Ít bucket hơn → ít bộ nhớ
- Nhiều pass hơn → chậm hơn
- Dễ debug

**Cơ số lớn (base = 256 hoặc 2^16):**
- Nhiều bucket → tốn bộ nhớ
- Ít pass hơn → nhanh hơn
- Phù hợp với biểu diễn nhị phân

**Công thức:** Với số n-bit, chọn base = 2^k
- Số pass = n / k
- Số bucket = 2^k
- Thường chọn k = 8 (base = 256) hoặc k = 16 (base = 65536)

## Tối ưu hóa

### 1. Chọn cơ số tối ưu

```cpp
// Sử dụng base = 2^8 = 256 cho int 32-bit
void radixSort256(vector<int>& arr) {
    // 4 passes cho 32-bit (4 bytes)
    for (int shift = 0; shift < 32; shift += 8) {
        vector<int> output(arr.size());
        vector<int> count(256, 0);

        // Đếm
        for (int x : arr) {
            int byte = (x >> shift) & 0xFF;
            count[byte]++;
        }

        // Tích lũy
        for (int i = 1; i < 256; i++) {
            count[i] += count[i - 1];
        }

        // Xây dựng
        for (int i = arr.size() - 1; i >= 0; i--) {
            int byte = (arr[i] >> shift) & 0xFF;
            output[--count[byte]] = arr[i];
        }

        arr = output;
    }
}
```

### 2. Early termination

```cpp
void radixSortOptimized(vector<int>& arr) {
    if (arr.empty()) return;

    int maxVal = *max_element(arr.begin(), arr.end());

    for (int exp = 1; maxVal / exp > 0; exp *= 10) {
        // Kiểm tra xem tất cả phần tử có cùng digit không
        int digit = (arr[0] / exp) % 10;
        bool allSame = true;
        for (int x : arr) {
            if ((x / exp) % 10 != digit) {
                allSame = false;
                break;
            }
        }

        if (allSame) continue;  // Skip pass này

        countingSortByDigit(arr, exp);
    }
}
```

### 3. In-place với bitwise operations

```cpp
// Sắp xếp in-place cho mảng chứa giá trị 0..255
void radixSortInPlace(vector<int>& arr) {
    const int BITS = 8;

    for (int bit = 0; bit < BITS; bit++) {
        int mask = 1 << bit;

        // Partition theo bit thứ bit
        int j = 0;
        for (int i = 0; i < arr.size(); i++) {
            if ((arr[i] & mask) == 0) {
                swap(arr[i], arr[j++]);
            }
        }
    }
}
```

## Ưu điểm

1. **Tuyến tính:** O(d(n+k)) có thể nhanh hơn O(n log n) khi d nhỏ
2. **Ổn định:** Giữ nguyên thứ tự tương đối
3. **Dễ song song hóa:** Mỗi bucket có thể xử lý độc lập
4. **Hiệu quả với số nguyên:** Rất nhanh cho dữ liệu số nguyên

## Nhược điểm

1. **Tốn bộ nhớ:** Cần O(n + k) bộ nhớ phụ
2. **Không general:** Chỉ áp dụng cho số nguyên, chuỗi
3. **Phụ thuộc vào d:** Khi d lớn (nhiều chữ số), không hiệu quả
4. **Cache unfriendly:** Truy cập bộ nhớ không tuần tự

## So sánh LSD vs MSD

| Đặc điểm | LSD | MSD |
|----------|-----|-----|
| Thứ tự | Ít quan trọng → Quan trọng | Quan trọng → Ít quan trọng |
| Cài đặt | Đơn giản | Phức tạp (đệ quy) |
| Bộ nhớ | O(n) | O(n + d×k) |
| Tốc độ | Ổn định | Có thể nhanh hơn |
| Độ dài khác nhau | Cần padding | Xử lý tốt |

## Khi nào sử dụng Radix Sort?

✅ **Nên dùng khi:**
- Sắp xếp số nguyên với phạm vi biết trước
- Số chữ số d nhỏ (d = O(log n))
- Cần thuật toán ổn định
- Có nhiều phần tử (n lớn)
- Sắp xếp chuỗi có độ dài giới hạn

❌ **Không nên dùng khi:**
- d >> log n (nhiều chữ số)
- Bộ nhớ hạn chế
- Dữ liệu không phải số nguyên/chuỗi
- Số phần tử ít (n nhỏ)

## Ứng dụng thực tế

### 1. Sắp xếp IP Address

```cpp
struct IP {
    int a, b, c, d;
    int toInt() const { return (a << 24) | (b << 16) | (c << 8) | d; }
};

void sortIPs(vector<IP>& ips) {
    vector<int> values;
    for (const IP& ip : ips) {
        values.push_back(ip.toInt());
    }

    radixSort256(values);

    for (int i = 0; i < ips.size(); i++) {
        int val = values[i];
        ips[i].a = (val >> 24) & 0xFF;
        ips[i].b = (val >> 16) & 0xFF;
        ips[i].c = (val >> 8) & 0xFF;
        ips[i].d = val & 0xFF;
    }
}
```

### 2. Sắp xếp theo nhiều key

```cpp
struct Student {
    string name;
    int year, month, day;  // Ngày sinh
};

void sortByBirthday(vector<Student>& students) {
    // Chuyển thành số nguyên: YYYYMMDD
    vector<pair<int, Student>> data;
    for (const Student& s : students) {
        int date = s.year * 10000 + s.month * 100 + s.day;
        data.push_back({date, s});
    }

    // Radix sort theo date
    vector<int> dates;
    for (auto [d, s] : data) dates.push_back(d);
    radixSort(dates);

    // Reconstruct
    // ... (implementation)
}
```

### 3. Suffix Array Construction

```cpp
// Sử dụng Radix Sort để xây dựng Suffix Array O(n log n)
vector<int> buildSuffixArray(const string& s) {
    int n = s.length();
    vector<int> sa(n), rank(n), tmp(n);

    // Khởi tạo rank
    for (int i = 0; i < n; i++) {
        sa[i] = i;
        rank[i] = s[i];
    }

    for (int k = 1; k < n; k *= 2) {
        // Radix sort theo (rank[i], rank[i+k])
        // ... (implementation)
    }

    return sa;
}
```

## Bài tập áp dụng

### Cơ bản
1. Cài đặt LSD Radix Sort và test
2. Sắp xếp mảng chuỗi bằng Radix Sort
3. So sánh tốc độ với Quick Sort

### Nâng cao
1. **Maximum Gap:** Tìm khoảng cách lớn nhất giữa hai phần tử liền kề
2. **Sort Array by Parity:** Sắp xếp số chẵn trước, lẻ sau
3. **Kth Largest Element:** Tìm phần tử lớn thứ k
4. **Suffix Array:** Xây dựng suffix array

### Bài tập trên OJ
- LeetCode 164 - Maximum Gap
- LeetCode 905 - Sort Array By Parity
- SPOJ - RSORT: Radix Sort Practice
- CSES - String Algorithms

## Các biến thể

### 1. American Flag Sort

Kết hợp MSD và in-place sorting.

### 2. Burst Sort

Tối ưu cho chuỗi dài bằng trie.

### 3. Flash Sort

Phân phối thông minh hơn dựa trên histogram.

## Tài liệu tham khảo

- [VNOI Wiki - Radix Sort](https://vnoi.info/wiki/algo/basic/sorting.md)
- Introduction to Algorithms (CLRS) - Chapter 8
- [Visualgo - Radix Sort Visualization](https://visualgo.net/en/sorting)

## Mẹo trong lập trình thi đấu

1. **Dùng base = 256:** Nhanh hơn base = 10 nhiều lần
   ```cpp
   // 4 passes thay vì ~10 passes
   radixSort256(arr);
   ```

2. **Kiểm tra số chữ số:** Nếu d quá lớn, dùng Quick Sort
   ```cpp
   int maxVal = *max_element(arr.begin(), arr.end());
   int digits = 0;
   while (maxVal > 0) { maxVal /= 10; digits++; }
   if (digits > log2(n)) sort(arr.begin(), arr.end());
   ```

3. **Combine với Counting Sort:** Radix Sort thực chất là nhiều lần Counting Sort

4. **Unsigned int:** Dễ xử lý hơn signed int khi dùng bitwise
   ```cpp
   void radixSort(vector<unsigned int>& arr);
   ```

5. **String sorting:** MSD tốt hơn LSD cho chuỗi có độ dài khác nhau
