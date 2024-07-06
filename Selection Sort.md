In this algorithm we iterate through the array to find the global minimum of the array and replace it with the beginning iteration of the array.

**Implementation with C#:**

```C#
void SelectionSort(int[] array)
{
	for(int i = 0; i < array.Length - 1; i++)
	{
		int min = i;
		for(int j = i + 1; j < array.Length; j++)
		{
			if(array[j] < array[min])
				min = j;
		}
		Swap(array[min], array[i]);
	}
}
```

* This algorithm has a worst-case and best-case time complexity of O(n^2).
* It is in place.
* It's okay on small data sets, but bad with large ones but bad with large ones.
