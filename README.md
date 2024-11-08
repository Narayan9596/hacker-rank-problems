#include <stdio.h>
#include <stdlib.h>

long long minTime(int files[], int files_size, int numCores, int limit) {
    long long ans = 0;
    int k = 1;
    int *v = (int*)malloc(files_size * sizeof(int));  // Array for files divisible by numCores
    int v_size = 0;  // To track the size of array v

    // Separate the files into those that are divisible by numCores and those that aren't
    for (int i = 0; i < files_size; i++) {
        if (files[i] % numCores != 0) {
            ans += files[i];
        } else {
            v[v_size++] = files[i];
        }
    }

    // Sort the array v in descending order
    for (int i = 0; i < v_size - 1; i++) {
        for (int j = i + 1; j < v_size; j++) {
            if (v[i] < v[j]) {
                int temp = v[i];
                v[i] = v[j];
                v[j] = temp;
            }
        }
    }

    // Process the files in v (those divisible by numCores)
    for (int i = 0; i < v_size; i++) {
        if (k <= limit) {
            ans += (v[i] / numCores);
            k++;
        } else {
            ans += v[i];
        }
    }

    // Free the memory allocated for array v
    free(v);

    return ans;
}

int main() {
    int files[] = {10, 20, 30, 40, 50};  // Example input
    int files_size = sizeof(files) / sizeof(files[0]);
    int numCores = 2;
    int limit = 3;

    long long result = minTime(files, files_size, numCores, limit);
    printf("%lld\n", result);

    return 0;
}
