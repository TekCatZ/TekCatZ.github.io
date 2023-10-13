---
author: ShadowCat
title: "Cùng giải Leetcode Daily Challenge - Ngày 12.10.2023 - 1095. Find in Mountain Array"
tags: ["leetcode", "algorithm"]
summary: "Giải Leetcode 1095. Find in Mountain Array"
showToc: true
date: 2023-10-12
---

## Bài toán
Link đến bài toán: [1095. Find in Mountain Array](https://leetcode.com/problems/find-in-mountain-array)

## Phân tích

Nếu tạm bỏ qua constraint
> Submissions making more than 100 calls to MountainArray.get will be judged Wrong Answer.
> Also, any solutions that attempt to circumvent the judge will result in disqualification.

Thì bài toán này đơn giản có thể giải bằng cách gọi `MountainArray.get` cho đến khi tìm được giá trị cần tìm.
Độ phức tạp của giải thuật là `O(n)`.

Nhìn vào dữ kiện của bài toán, ta thấy rằng mảng `mountainArr` là một mảng tăng dần đến một điểm `peak` rồi giảm dần.
Làm thế nào để tìm một phần tử trong một mảng đã được sắp xếp? ***Binary Search***.

Tuy nhiên, ta chưa thể áp dụng binary search trực tiếp vào bài toán này được vì mảng `mountainArr` chưa phải hoàn toàn được sắp xếp.
Ta cần tìm một điểm chia mảng `mountainArr` thành hai mảng con được sắp xếp tăng dần và giảm dần. Ta sẽ gọi điểm chia này là `peak`.

Nhận thấy rằng, với `0 <= i < peak`, `mountainArr[i] < mountainArr[i + 1]` và với `peak < i < mountainArr.length() - 1`,
`mountainArr[i] > mountainArr[i + 1]`.
Ta có thể áp dụng binary search để tìm `peak` với độ phức tạp `O(log(n))`.

Sau khi tìm được `peak`, ta có thể áp dụng binary search để tìm giá trị cần tìm trong mảng con được sắp xếp tăng dần và giảm dần.

## Giải thuật

1. Tìm `peak` bằng `binary search`:
     1. Nếu `mountainArr[mid] < mountainArr[mid + 1]` thì `peak` nằm ở bên phải `mid`.
     2. Nếu `mountainArr[mid] > mountainArr[mid + 1]` thì `peak` nằm ở bên trái `mid`.
     3. Lặp lại bước 1 cho đến khi `left == right`.
2. Áp dụng `binary search` cho mảng tăng dần trên đoạn `[0, peak]`.
3. Nếu không tìm thấy ở bước 2, áp dụng `binary search` mảng giảm dần trên đoạn `[peak + 1, mountainArr.length() - 1]`.

Độ phức tạp của giải thuật là `O(log(n))`.

## Code

```go
     func findInMountainArray(target int, mountainArr *MountainArray) int {
        n := mountainArr.length()
        peakIndex := findPeak(mountainArr, 0, n-1)
        peakValue := mountainArr.get(peakIndex)
        if target == peakValue {
          return peakIndex
        }
        
        searchLeft := binarySearch(mountainArr, target, 0, peakIndex-1, true)
        if searchLeft != -1 {
          return searchLeft
        }
        
        return binarySearch(mountainArr, target, peakIndex+1, n-1, false)
    }
    
    func findPeak(mountainArr *MountainArray, l, r int) int {
        for l < r {
            mid := l + (r-l)/2
            if mountainArr.get(mid) < mountainArr.get(mid+1) {
                l = mid + 1
            } else {
                    r = mid
            }
        }

         return l
    }

    func binarySearch(mountainArr *MountainArray, target, l, r int, asc bool) int {
         if asc {
              for l <= r {
                    mid := l + (r-l)/2
                    midValue := mountainArr.get(mid)
                    if midValue == target {
                         return mid
                    } else if midValue < target {
                         l = mid + 1
                    } else {
                         r = mid - 1
                    }
              }
         } else {
              for l <= r {
                    mid := l + (r-l)/2
                    midValue := mountainArr.get(mid)
                    if midValue == target {
                         return mid
                    } else if midValue < target {
                         r = mid - 1
                    } else {
                         l = mid + 1
                    }
              }
         }
    
         return -1
    }

```