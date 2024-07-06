It is the simplest sorting algorithm. 

The idea is to start from the starting index of the array, compare adjacent elements and swap if one is greater or smaller than the other. 
Repeat this process multiple times until the array is sorted.

**Implementation in C#**:
```C#
void BubbleSort(int A[])
{
	for(int i = 0; i < A.Length - 1; i++)
	{
		for(int j = 0; j < A.Length - i - 1; j++)
		{
			if(A[j] > A[j+1])
			{
				Swap(A[j], A[j+1]);
			}
		}
	}
}
```

This algorithm is the least efficient algorithm for comparison-based sorting algorithms.

* Its worst and base case time-complexity is O(n^2).
* It's in-place.
* It is slightly inefficient even in small data sets and even worse with large ones.
