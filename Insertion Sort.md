We divide the array into two sorted and unsorted portions.

We take the first element of the unsorted array and compare it with the all of the elements at the sorted array. If any of them is larger, we will shift them to the right.

**implementation in C#:**

```C#
void InsertionSort(int[] array)
{
	for(int i = 1; i < array.Length; i++)
	{
		int key = array[i-1];
		int j = i - 1;
		while(j >= 0 && array[j] > key)
		{
			array[j+1] = array[j];
			j--;
		}
		
		array[j+1] = key;
	}
}
```

* It has a worst-case complexity of O(n^2).
* It can have the best-case complexity of O(n) if the array is already sorted.
* Insertion Sort is usually preferred compared to bubble or selection sort because of its less steps on each iteration and its possible best-case complexity.

