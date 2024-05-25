import random
import time

# Function to generate a random array of integers
def generateRandomArray(size):
    return [random.randint(0, 999) for _ in range(size)]

# Insertion Sort
def insertionSort(array):
    for i in range(1, len(array)):
        key = array[i]
        j = i - 1
        while j >= 0 and array[j] > key:
            array[j + 1] = array[j]
            j -= 1
        array[j + 1] = key

# Merge Sort
def mergeSort(array):
    if len(array) > 1:
        mid = len(array) // 2
        L = array[:mid]
        R = array[mid:]

        mergeSort(L)
        mergeSort(R)

        i = j = k = 0

        while i < len(L) and j < len(R):
            if L[i] < R[j]:
                array[k] = L[i]
                i += 1
            else:
                array[k] = R[j]
                j += 1
            k += 1

        while i < len(L):
            array[k] = L[i]
            i += 1
            k += 1

        while j < len(R):
            array[k] = R[j]
            j += 1
            k += 1

# Heapsort
def heapify(array, size, index):
    largest = index
    left = 2 * index + 1
    right = 2 * index + 2

    if left < size and array[left] > array[largest]:
        largest = left

    if right < size and array[right] > array[largest]:
        largest = right

    if largest != index:
        array[index], array[largest] = array[largest], array[index]
        heapify(array, size, largest)

def heapSort(array):
    size = len(array)

    for i in range(size // 2 - 1, -1, -1):
        heapify(array, size, i)

    for i in range(size - 1, 0, -1):
        array[0], array[i] = array[i], array[0]
        heapify(array, i, 0)

# Quicksort
def partition(array, low, high):
    pivot = array[high]
    i = low - 1
    for j in range(low, high):
        if array[j] < pivot:
            i += 1
            array[i], array[j] = array[j], array[i]
    array[i + 1], array[high] = array[high], array[i + 1]
    return i + 1

def quickSort(array, low, high):
    if low < high:
        pi = partition(array, low, high)
        quickSort(array, low, pi - 1)
        quickSort(array, pi + 1, high)

if __name__ == "__main__":
    datasetSizes = [10, 1000, 50000, 1000000] # Dataset sizes to test
    sorting_algorithms = {
        "Insertion Sort": insertionSort,
        "Merge Sort": mergeSort,
        "Heap Sort": heapSort,
        "Quick Sort": lambda array: quickSort(array, 0, len(array) - 1)
    }
    
    for algorithm_name, sorting_algorithm in sorting_algorithms.items():
        print(f"\nTesting {algorithm_name}:")
        for size in datasetSizes:
            array = generateRandomArray(size)
            start = time.time() # Start time

            sorting_algorithm(array)

            stop = time.time() # Stop time
            duration = (stop - start) * 1000 # Convert to milliseconds
            print("Dataset Size:", size, ", Execution Time:", duration, "milliseconds")
