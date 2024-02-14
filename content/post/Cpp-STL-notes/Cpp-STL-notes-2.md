---
title: "C++ STL学习笔记(二) : algorithm 算法"
date: 2023-09-11T00:00:01+08:00
description: "一个自用的C++ 参考手册，学习方向参考C++ 大学教程第九版。本节关键词：algorithm 算法"
categories: [C++]
tags: [C++, STL]
draft: false
---

## 迭代器无效

在使用算法时，有时候会遇到迭代器失效的问题。迭代器失效指的是在对容器进行修改后，之前获取的迭代器可能会变得无效，不能再使用。

迭代器失效的情况有多种，例如在插入元素后，原来的迭代器可能会失效；在删除元素后，指向删除元素的迭代器也会失效。

## algorithm

### fill, fill_n, generate, generate_n

- `fill(first, last, value)`：将[first, last)范围内的所有元素都设置为指定的值 value。
- `fill_n(first, n, value)`：将从 first 开始的 n 个元素都设置为指定的值 value。
- `generate(first, last, generator)`：使用指定的生成器 generator 生成[first, last)范围内的元素。
- `generate_n(first, n, generator)`：使用指定的生成器 generator 生成从 first 开始的 n 个元素。

示例代码：

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

int main() {
    std::vector<int> nums(5);

    // 使用 fill 将元素设置为 10
    std::fill(nums.begin(), nums.end(), 10);

    // 输出结果：10 10 10 10 10
    for (int num : nums) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    // 使用 fill_n 将前3个元素设置为 5
    std::fill_n(nums.begin(), 3, 5);

    // 输出结果：5 5 5 10 10
    for (int num : nums) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    // 使用 generate 生成随机数
    std::generate(nums.begin(), nums.end(), []() { return rand() % 100; });

    // 输出结果：随机生成的 5 个数
    for (int num : nums) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

### equal, mismatch, lexicographical_compare

- `equal(first1, last1, first2)`：判断两个范围内的元素是否相等。
- `mismatch(first1, last1, first2)`：找到两个范围内第一个不相等的元素，并返回一个 pair，其中第一个元素是第一个不相等的元素在第一个范围中的迭代器，第二个元素是第一个不相等的元素在第二个范围中的迭代器。
- `lexicographical_compare(first1, last1, first2, last2)`：按字典序比较两个范围内的元素。

示例代码：

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

int main() {
    std::vector<int> nums1{1, 2, 3, 4, 5};
    std::vector<int> nums2{1, 2, 3, 4, 6};

    // 检查两个范围内的元素是否相等
    bool isEqual = std::equal(nums1.begin(), nums1.end(), nums2.begin());
    std::cout << "Is equal: " << (isEqual ? "true" : "false") << std::endl;

    // 查找第一个不相等的元素
    auto mismatchPair = std::mismatch(nums1.begin(), nums1.end(), nums2.begin());
    if (mismatchPair.first != nums1.end()) {
        std::cout << "First mismatch: " << *mismatchPair.first << " - " << *mismatchPair.second << std::endl;
    } else {
        std::cout << "No mismatch found: 没有找到不匹配的元素。" << std::endl;
    }

    // 按字典序比较两个范围内的元素
    bool isLess = std::lexicographical_compare(nums1.begin(), nums1.end(), nums2.begin(), nums2.end());
    std::cout << "Is nums1 < nums2: " << (isLess ? "true" : "false") << std::endl;

    return 0;
}
```

### remove, remove_if, remove_copy, remove_copy_if

- `remove(first, last, value)`：从[first, last)范围内移除所有等于指定值 value 的元素，并将剩余的元素移到范围的前端，并返回指向新范围末尾的迭代器。
- `remove_if(first, last, predicate)`：从[first, last)范围内移除所有满足谓词 predicate 的元素，并将剩余的元素移到范围的前端，并返回指向新范围末尾的迭代器。
- `remove_copy(first, last, result, value)`：从[first, last)范围内拷贝除了等于指定值 value 的元素以外的所有元素到[result, result+(last-first))范围，并返回指向新范围末尾的迭代器。
- `remove_copy_if(first, last, result, predicate)`：从[first, last)范围内拷贝除了满足谓词 predicate 的元素以外的所有元素到[result, result+(last-first))范围，并返回指向新范围末尾的迭代器。

示例代码：

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

int main() {
    std::vector<int> nums{1, 2, 3, 4, 5, 3, 6, 3};

    // 移除所有等于 3 的元素
    auto newEnd = std::remove(nums.begin(), nums.end(), 3);
    nums.erase(newEnd, nums.end());

    // 输出结果：1 2 4 5 6
    for (int num : nums) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    // 移除所有大于 3 的元素
    auto newEnd2 = std::remove_if(nums.begin(), nums.end(), [](int num) { return num > 3; });
    nums.erase(newEnd2, nums.end());

    // 输出结果：1 2 3
    for (int num : nums) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    std::vector<int> newNums(nums.size());

    // 拷贝除了等于 2 的元素以外的所有元素到新的容器
    auto newEnd3 = std::remove_copy(nums.begin(), nums.end(), newNums.begin(), 2);

    // 输出结果：1 3 0
    for (int i = 0; i < std::distance(newNums.begin(), newEnd3); ++i) {
        std::cout << newNums[i] << " ";
    }
    std::cout << std::endl;

    std::vector<int> newNums2(nums.size());

    // 拷贝除了小于等于 1 的元素以外的所有元素到新的容器
    auto newEnd4 = std::remove_copy_if(nums.begin(), nums.end(), newNums2.begin(), [](int num) { return num <= 1; });

    // 输出结果：3
    for (int i = 0; i < std::distance(newNums2.begin(), newEnd4); ++i) {
        std::cout << newNums2[i] << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

### replace, replace_if, replace_copy, replace_copy_if

- `replace(first, last, oldValue, newValue)`：将[first, last)范围内所有等于 oldValue 的元素替换为 newValue。
- `replace_if(first, last, predicate, newValue)`：将[first, last)范围内所有满足谓词 predicate 的元素替换为 newValue。
- `replace_copy(first, last, result, oldValue, newValue)`：将从 `[first, last)` 范围内拷贝元素到 `[result, result+(last-first))` 范围，并将所有等于 `oldValue` 的元素替换为 `newValue`。
- `replace_copy_if(first, last, result, predicate, newValue)`：将从 `[first, last)` 范围内拷贝元素到 `[result, result+(last-first))` 范围，并将所有满足谓词 `predicate` 的元素替换为 `newValue`。

示例代码：

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

int main() {
    std::vector<int> nums{1, 2, 3, 4, 5};

    // 将所有等于 3 的元素替换为 6
    std::replace(nums.begin(), nums.end(), 3, 6);

    // 输出结果：1 2 6 4 5
    for (int num : nums) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    // 将所有大于 3 的元素替换为 0
    std::replace_if(nums.begin(), nums.end(), [](int num) { return num > 3; }, 0);

    // 输出结果：1 2 0 0 0
    for (int num : nums) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    std::vector<int> newNums(nums.size());

    // 将所有等于 0 的元素替换为 9，并拷贝到新的容器
    std::replace_copy(nums.begin(), nums.end(), newNums.begin(), 0, 9);

    // 输出结果：1 2 9 9 9
    for (int num : newNums) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    std::vector<int> newNums2(nums.size());

    // 将所有小于等于 1 的元素替换为 8，并拷贝到新的容器
    std::replace_copy_if(nums.begin(), nums.end(), newNums2.begin(), [](int num) { return num <= 1; }, 8);

    // 输出结果：8 2 9 9 9
    for (int num : newNums2) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

### swap, iter_swap, swap_ranges

- `swap(x, y)`：交换两个对象的值。
- `iter_swap(iter1, iter2)`：交换两个迭代器指向的元素的值。
- `swap_ranges(first1, last1, first2)`：交换两个范围内的元素。

示例代码：

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

int main() {
    int a = 1;
    int b = 2;

    // 交换两个变量的值
    std::swap(a, b);

    std::cout << "a: " << a << std::endl; // 输出结果：2
    std::cout << "b: " << b << std::endl; // 输出结果：1

    std::vector<int> nums1{1, 2, 3};
    std::vector<int> nums2{4, 5, 6};

    // 交换两个迭代器指向的元素
    std::iter_swap(nums1.begin(), nums2.begin());

    // 输出结果：nums1: 4 2 3
    std::cout << "nums1: ";
    for (int num : nums1) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    // 输出结果：nums2: 1 5 6
    std::cout << "nums2: ";
    for (int num : nums2) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    std::vector<int> nums3{1, 2, 3};
    std::vector<int> nums4{4, 5, 6};

    // 交换两个范围内的元素
    std::swap_ranges(nums3.begin(), nums3.end(), nums4.begin());

    // 输出结果：nums3: 4 5 6
    std::cout << "nums3: ";
    for (int num : nums3) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    // 输出结果：nums4: 1 2 3
    std::cout << "nums4: ";
    for (int num : nums4) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    return 0;
}
```
