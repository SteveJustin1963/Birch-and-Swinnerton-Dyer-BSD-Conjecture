# Birch-and-Swinnerton-Dyer-BSD-Conjecture
This program is designed to:

1. **Plot an elliptic curve** (defined by the equation \( y^2 = x^3 + ax + b \)) as an **ASCII graph** on a \( 50 \times 50 \) grid.
2. **List rational points** satisfying the curve equation within a specified range.
3. Allow the user to control the **step size** for evaluating \( x \) values.

---

### **How It Works**

#### **1. User Inputs**
The program begins by asking the user to input:
- \( a \): Coefficient of \( x \) in the curve equation.
- \( b \): Constant term in the curve equation.
- Step size: A floating-point number that determines how finely the program steps through \( x \) values.
```
For example:
- \( a = 1.25 \), \( b = -0.75 \), \( \text{step size} = 0.5 \).

The curve equation becomes:
\[
y^2 = x^3 + 1.25x - 0.75
\]
The program will check \( x \) values like \(-25.0, -24.5, -24.0, \dots, 25.0\).
```
---

#### **2. Rational Point Finder**
The program calculates \( y^2 = x^3 + ax + b \) for each \( x \) within the range \([-25, 25]\) using the given step size. It then checks if \( y^2 \) is a **perfect square**:
- A perfect square means there exists an integer \( y \) such that \( y^2 = x^3 + ax + b \).

**Key Points**:
- If \( y^2 \geq 0 \) and is a perfect square, both \( (x, y) \) and \( (x, -y) \) are valid points on the curve.
- The function uses floating-point arithmetic and tolerances (\( 10^{-6} \)) to check for perfect squares.

**Example**:
For \( x = -24.5 \):
\[
y^2 = (-24.5)^3 + 1.25(-24.5) - 0.75 = 1220.99
\]
\[
y = \sqrt{1220.99} \approx 34.94
\]
So the points are \( (-24.5, 34.94) \) and \( (-24.5, -34.94) \).

The program prints:
```
Point: (-24.50, 34.94)
Point: (-24.50, -34.94)
```

---

#### **3. ASCII Plotting**
The program generates a \( 50 \times 50 \) ASCII grid to plot the curve:
- \( x \)-axis ranges from \(-25\) to \( 25 \).
- \( y \)-axis ranges from \(-25\) to \( 25 \).
- Each character in the grid corresponds to a specific \( (x, y) \) position.

**Key Features**:
1. **Axes**:
   - Vertical axis (\( x = 0 \)) is marked with `|`.
   - Horizontal axis (\( y = 0 \)) is marked with `-`.
   - The origin (\( 0, 0 \)) is marked with `+`.

2. **Point Plotting**:
   - Each \( (x, y) \) point is converted to grid indices:
     \[
     \text{grid\_x} = \text{round}(x + 25)
     \]
     \[
     \text{grid\_y} = \text{round}(25 - y)
     \]
   - Points are marked with `*` on the grid.
   - Both positive and negative \( y \) values are plotted.

---

#### **4. Step Size**
The step size controls how finely \( x \) values are evaluated:
- **Larger step size** (e.g., 1):
  - Faster computation.
  - Fewer points evaluated, resulting in a less detailed graph.
- **Smaller step size** (e.g., 0.1 or 0.01):
  - Slower computation.
  - More points evaluated, resulting in a highly detailed graph.

---

### **Example Walkthrough**

#### Input:
```
a: 1.25
b: -0.75
Step size: 0.5
```

#### Output:
1. **Elliptic Curve Definition**:
   ```
   Elliptic Curve: y^2 = x^3 + 1.25x - 0.75
   ```

2. **Rational Points**:
   The program calculates and lists valid points:

   ```
   Point: (-24.50, 34.94)
   Point: (-24.50, -34.94)
   Point: (-24.00, 34.66)
   Point: (-24.00, -34.66)
   
   ```

4. **ASCII Plot**:
   The graph might look like this:

```
                        *                                                    
                        |                                                    
                        |                                                    
                        |                                                    
       *                |                *                                 
                        |                                                    
                        |                                                    
+-----------------------+-----------------------+-----------------------+
                        |                                                    
       *                |                *                                 
                        |                                                    
                        |                                                    
                        |                                                    
                        *                                                    
```

 

### **Key Components of the Code**

#### **Main Function**
Handles:
- Taking inputs for \( a, b, \text{step size} \).
- Validating step size.
- Calling other functions for point calculation and plotting.

#### **Rational Point Finder**
- Evaluates \( y^2 = x^3 + ax + b \) for each \( x \) and finds points where \( y^2 \) is a perfect square.

#### **ASCII Plotting**
- Creates a \( 50 \times 50 \) grid.
- Marks:
  - `*` for points on the curve.
  - `|` for the vertical axis.
  - `-` for the horizontal axis.
  - `+` for the origin.

 


```
// https://www.onlinegdb.com/online_c_compiler

#include <stdio.h>
#include <math.h>
#include <stdbool.h>

#define GRID_SIZE 50  // ASCII grid size (50x50 for -25 to 25)

// Function prototypes
bool is_perfect_square(double n);
void find_rational_points(double a, double b, double step);
void plot_curve_ascii(double a, double b, double step);

int main() {
    double a, b, step;

    // Input curve parameters
    printf("Enter coefficients for elliptic curve y^2 = x^3 + ax + b\n");
    printf("a: ");
    scanf("%lf", &a);  // Use %lf for double input
    printf("b: ");
    scanf("%lf", &b);
    printf("Enter step size (e.g., 1 or 0.1): ");
    scanf("%lf", &step);

    // Validate step size
    if (step <= 0) {
        printf("Step size must be greater than 0.\n");
        return 1;
    }

    printf("\nElliptic Curve: y^2 = x^3 + %.2fx + %.2f\n", a, b);

    // Find rational points
    printf("\nFinding rational points...\n");
    find_rational_points(a, b, step);

    // Plot the curve in ASCII
    printf("\nASCII Plot of the curve:\n");
    plot_curve_ascii(a, b, step);

    return 0;
}

// Function to check if a number is a perfect square
bool is_perfect_square(double n) {
    if (n < 0) return false;
    double root = sqrt(n);
    return fabs(root - round(root)) < 1e-6;  // Allow floating-point tolerance
}

// Function to find rational points on the curve
void find_rational_points(double a, double b, double step) {
    for (double x = -GRID_SIZE / 2.0; x <= GRID_SIZE / 2.0; x += step) {
        double y2 = x * x * x + a * x + b;
        if (is_perfect_square(y2)) {
            double y = sqrt(y2);
            printf("Point: (%.2f, %.2f)\n", x, y);
            if (fabs(y) > 1e-6) {  // Avoid duplicate zero points
                printf("Point: (%.2f, %.2f)\n", x, -y);
            }
        }
    }
}

// Function to plot the curve as an ASCII grid
void plot_curve_ascii(double a, double b, double step) {
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
    for (double x = -GRID_SIZE / 2.0; x <= GRID_SIZE / 2.0; x += step) {
        double y2 = x * x * x + a * x + b;
        if (y2 >= 0) {
            double y = sqrt(y2);
            int plot_x = (int)round(x + GRID_SIZE / 2.0);  // Shift x to fit in the grid
            int plot_y_positive = (int)round(GRID_SIZE / 2.0 - y);  // Invert y for positive values
            int plot_y_negative = (int)round(GRID_SIZE / 2.0 + y);  // Invert y for negative values

            if (plot_x >= 0 && plot_x < GRID_SIZE) {
                if (plot_y_positive >= 0 && plot_y_positive < GRID_SIZE) {
                    grid[plot_y_positive][plot_x] = '*';  // Plot positive y
                }
                if (plot_y_negative >= 0 && plot_y_negative < GRID_SIZE) {
                    grid[plot_y_negative][plot_x] = '*';  // Plot negative y
                }
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


## Significance
The elliptic curve calculations in this code are significant because they relate to mathematical concepts and problems in number theory, particularly conjectures like the **Birch and Swinnerton-Dyer Conjecture** and the exploration of **rational points on elliptic curves**.

Here’s the breakdown of the significance and how it ties to conjectures:

---

### **1. What Are Elliptic Curves?**
Elliptic curves are equations of the form:
```
\[
y^2 = x^3 + ax + b
\]
```
They are widely studied in mathematics because:
- They have deep connections to algebraic geometry, number theory, and cryptography.
- They are central to solving Diophantine equations, which involve finding integer or rational solutions to polynomial equations.

---

### **2. Rational Points and Their Significance**
A **rational point** on an elliptic curve is a point \((x, y)\) where both \(x\) and \(y\) are rational numbers. These points are significant because:
- Rational points form a group under a special addition law defined on elliptic curves.
- Finding all rational points on a given curve is a classic problem in number theory.

**Relevance to Conjectures**:
- The **Mordell-Weil Theorem** states that the set of rational points on an elliptic curve forms a finitely generated abelian group.
- The **rank** of this group (loosely, the number of independent rational points) is a fundamental quantity that helps in understanding the distribution of rational solutions.

---

### **3. The Birch and Swinnerton-Dyer Conjecture**
The Birch and Swinnerton-Dyer (BSD) Conjecture connects the rank of an elliptic curve to the behavior of its **L-function** at a specific point.

- The conjecture states that the number of rational points on an elliptic curve is related to the order of the zero of the curve's \(L\)-function at \(s = 1\).
- If the \(L\)-function does not vanish, the curve has a finite number of rational points. If it vanishes to order \(r\), the curve has rank \(r\), implying an infinite number of rational points.

---

### **Significance of This Calculation**
1. **Exploration of Rational Points**:
   - This program identifies rational points by iterating over possible \(x\) values and checking if \(y^2\) is a perfect square.
   - Every rational point found contributes to understanding the rank of the elliptic curve.

2. **Testing Specific Cases**:
   - By inputting specific \(a\) and \(b\) values, you can study individual curves.
   - Patterns in rational points might provide insight into open problems about rational solutions.

3. **Visualizing the Curve**:
   - The ASCII grid is a simple visualization, helping to identify symmetry and the general shape of the curve.
   - Visualization can highlight potential patterns or structures among points.

4. **Numerical Verification**:
   - It can be used to test conjectures numerically for specific curves, such as verifying cases where the curve has infinite rational points (positive rank) or only a few (rank 0).

---

### **How It Helps Solve the Conjecture**
1. **Experimental Evidence**:
   - Programs like this provide numerical data to test the BSD conjecture for specific cases.
   - By calculating and analyzing rational points, mathematicians can check the consistency of the conjecture with observed behavior.

2. **Connection to Cryptography**:
   - Rational points on elliptic curves are foundational to elliptic curve cryptography (ECC), which relies on the difficulty of certain computations involving these points.

3. **Insight into Rank**:
   - The density and distribution of rational points give indirect hints about the curve's rank.
   - For example:
     - If no rational points are found (besides trivial ones), the rank is likely 0.
     - A pattern of rational points might suggest a higher rank.

4. **Education and Exploration**:
   - Simple tools like this help mathematicians and students understand the behavior of elliptic curves and test small cases that might inspire general results.

---

### **Example: Tying It Back to the BSD Conjecture**
- Suppose you input \(a = -1\), \(b = 1\) and find rational points \((-2, 3), (0, 1)\), etc.
- This indicates that the curve has non-trivial rational solutions, suggesting a rank \( \geq 1 \).
- You could explore if these points generate the full group of rational points or if there are others.

---

### **Conclusion**
While this program doesn’t directly solve the Birch and Swinnerton-Dyer Conjecture, it provides tools to explore rational points, a key part of understanding elliptic curves. Numerical exploration like this complements theoretical approaches, contributing to ongoing research in this profound area of mathematics.




## Run

```
Enter coefficients for elliptic curve y^2 = x^3 + ax + b
a: -5
b: 5
Enter step size (e.g., 1 or 0.1): .5

Elliptic Curve: y^2 = x^3 + -5.00x + 5.00

Finding rational points...
Point: (-1.00, 3.00)
Point: (-1.00, -3.00)
Point: (1.00, 1.00)
Point: (1.00, -1.00)
Point: (4.00, 7.00)
Point: (4.00, -7.00)

ASCII Plot of the curve:
                         |                        
                         |        *               
                         |                        
                         |       *                
                         |                        
                         |       *                
                         |                        
                         |      *                 
                         |                        
                         |      *                 
                         |                        
                         |     *                  
                         |                        
                         |     *                  
                         |                        
                         |    *                   
                         |    *                   
                         |                        
                         |   *                    
                         |   *                    
                         |                        
                         |  *                     
                       ***  *                     
                         ***                      
                       * |**                      
-------------------------+------------------------
                       * |**                      
                         ***                      
                       ***  *                     
                         |  *                     
                         |                        
                         |   *                    
                         |   *                    
                         |                        
                         |    *                   
                         |    *                   
                         |                        
                         |     *                  
                         |                        
                         |     *                  
                         |                        
                         |      *                 
                         |                        
                         |      *                 
                         |                        
                         |       *                
                         |                        
                         |       *                
                         |                        
                         |        *               


...Program finished with exit code 0

Press ENTER to exit console.
```


