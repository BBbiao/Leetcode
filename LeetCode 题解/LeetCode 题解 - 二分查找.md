# 二分查找
<!-- GFM-TOC -->

<!-- GFM-TOC -->

```cpp
int binarySearch(int *arr , int low , int high , int target)//递归实现
{
	int middle = (low + high)/2;
	if(low > high)
		return -1;
	if(arr[middle] == target)
		return middle;
	if(arr[middle] > target)
		return binarySearch( arr , low , middle - 1 , target);
	if(arr[middle] < target)
		return binarySearch( arr , middle + 1 , high , target);
 
};
```

```cpp
int binarySearch1(int a[], int n , int target)//循环实现
{
	int low = 0 ,high = n , middle;
	while(low < high)
	{
	   middle = (low + high)/2;
       if(target == a[middle])
		   return middle;
	   else if(target > a[middle])
		   low = middle +1;
	   else if(target < a[middle])
		   high = middle;
	}
	return -1;
};
```
