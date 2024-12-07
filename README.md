# Birch-and-Swinnerton-Dyer-BSD-Conjecture


```
#include <stdio.h>
#include <math.h>
#include <stdbool.h>

#define BOUND 50   // Search range for rational points
#define GRID_SIZE 100  // ASCII grid size (100x100 for -50 to 50)

// Function prototypes
bool is_perfect_square(int n);
void find_rational_points(int a, int b);
void plot_curve_ascii(int a, int b);

int main() {
    int a, b;

    // Input curve parameters
    printf("Enter coefficients for elliptic curve y^2 = x^3 + ax + b\n");
    printf("a: ");
    scanf("%d", &a);
    printf("b: ");
    scanf("%d", &b);

    printf("\nElliptic Curve: y^2 = x^3 + %dx + %d\n", a, b);

    // Find rational points
    printf("\nFinding rational points...\n");
    find_rational_points(a, b);

    // Plot the curve in ASCII
    printf("\nASCII Plot of the curve:\n");
    plot_curve_ascii(a, b);

    return 0;
}

// Function to check if a number is a perfect square
bool is_perfect_square(int n) {
    if (n < 0) return false;
    int root = (int)sqrt(n);
    return root * root == n;
}

// Function to find rational points on the curve
void find_rational_points(int a, int b) {
    for (int x = -BOUND; x <= BOUND; x++) {
        int y2 = x * x * x + a * x + b;
        if (is_perfect_square(y2)) {
            int y = (int)sqrt(y2);
            printf("Point: (%d, %d)\n", x, y);
            if (y != 0) {
                printf("Point: (%d, %d)\n", x, -y);
            }
        }
    }
}

// Function to plot the curve as an ASCII grid
void plot_curve_ascii(int a, int b) {
    char grid[GRID_SIZE][GRID_SIZE];

    // Initialize the grid with blank spaces
    for (int i = 0; i < GRID_SIZE; i++) {
        for (int j = 0; j < GRID_SIZE; j++) {
            grid[i][j] = ' ';
        }
    }

    // Draw the axes
    for (int i = 0; i < GRID_SIZE; i++) {
        grid[i][GRID_SIZE / 2] = '|';  // Vertical axis
        grid[GRID_SIZE / 2][i] = '-';  // Horizontal axis
    }
    grid[GRID_SIZE / 2][GRID_SIZE / 2] = '+';  // Origin

    // Plot points on the grid
    for (int x = -BOUND; x <= BOUND; x++) {
        int y2 = x * x * x + a * x + b;
        if (y2 >= 0) {
            int y = (int)sqrt(y2);
            int plot_x = x + BOUND;  // Shift x to fit in the grid
            int plot_y_positive = BOUND - y;  // Invert y for positive values
            int plot_y_negative = BOUND + y;  // Invert y for negative values

            if (plot_y_positive >= 0 && plot_y_positive < GRID_SIZE) {
                grid[plot_y_positive][plot_x] = '*';  // Plot positive y
            }
            if (plot_y_negative >= 0 && plot_y_negative < GRID_SIZE) {
                grid[plot_y_negative][plot_x] = '*';  // Plot negative y
            }
        }
    }

    // Print the grid
    for (int i = 0; i < GRID_SIZE; i++) {
        for (int j = 0; j < GRID_SIZE; j++) {
            printf("%c", grid[i][j]);
        }
        printf("\n");
    }
}
```
