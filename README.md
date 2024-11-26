# banker-s-algo

#include <iostream>
using namespace std;

void runBankerAlgo(int n, int m, int max[][3], int allocation[][3], int available[]) {
    int need[n][m];
    bool finish[n] = {false};
    int safeSequence[n];
    
    // Calculate the need matrix
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            need[i][j] = max[i][j] - allocation[i][j];
        }
    }

    int count = 0;
    
    // Check for a safe sequence
    while (count < n) {
        bool found = false;
        for (int i = 0; i < n; i++) {
            if (!finish[i]) {
                int j;
                for (j = 0; j < m; j++) {
                    if (need[i][j] > available[j]) {
                        break;
                    }
                }
                if (j == m) {
                    for (int k = 0; k < m; k++) {
                        available[k] += allocation[i][k];
                    }
                    finish[i] = true;
                    safeSequence[count++] = i;
                    found = true;
                }
            }
        }
        if (!found) {
            cout << "Safe sequence does not exist!" << endl;
            return;
        }
    }

    cout << "Safe sequence exists: ";
    for (int i = 0; i < n; i++) {
        cout << "P" << safeSequence[i];
        if (i != n - 1) cout << " -> ";
    }
    cout << endl;
}

int main() {
    int n = 5, m = 3;

    cout << "Test Case 1: Safe Sequence Exists" << endl;
    int allocation1[5][3] = {{0, 1, 0}, {2, 0, 0}, {3, 0, 2}, {2, 1, 1}, {0, 0, 2}};
    int max1[5][3] = {{7, 5, 3}, {3, 2, 2}, {9, 0, 2}, {2, 2, 2}, {4, 3, 3}};
    int available1[3] = {3, 3, 2};
    runBankerAlgo(n, m, max1, allocation1, available1);

    cout << "\nTest Case 2: No Safe Sequence" << endl;
    int allocation2[5][3] = {{0, 1, 0}, {2, 0, 0}, {3, 0, 2}, {2, 1, 1}, {0, 0, 2}};
    int max2[5][3] = {{11, 5, 3}, {3, 2, 2}, {9, 0, 2}, {2, 2, 2}, {4, 3, 3}};
    int available2[3] = {3, 3, 2};
    runBankerAlgo(n, m, max2, allocation2, available2);

    return 0;
}
