# Fall-2023-QUANTITATIVE-MANAGEMENT-MODELING-BA-64018-006
Quantitative Management Modeling 
Quantitative Management Modeling Module 4
Javadbay Khalilzada
2023-09-23
#recalling LP model pacakge
library(lpSolve)

#it is a Maximization task
#Where I asssign,  x1-Number of large-sized units produced; x2-Number of medium-sized units produced;
#x3-Number of small-sized units produced.
# Maximize Z = 420x1 + 360x2 + 300x3

#Constrains of maximizing of profit are as follows:

##Production capacity constraints 
#Plant 1: x1 + x2 + x3 ≤ 750
#Plant 2: x1 + x2 + x3 ≤ 900
#Plant 3: x1 + x2 + x3 ≤ 450

##Storage space constraints for plants
#Plant 1: 20x1 + 15x2 + 12x3 ≤ 13,000
#Plant 2: 20x1 + 15x2 + 12x3 ≤ 12,000
#Plant 3: 20x1 + 15x2 + 12x3 ≤ 5,000


##Sales forecasts constraint: 900x1 + 1200x2 + 750x3 ≤ Total Sales a DAY

# Coefficients for the objective function (profit)
f_obj <- c(420, 360, 300)

# Define the matrix of coefficients for the constraints
f_con <- matrix(c(
  1, 1, 1,
  1, 1, 1,
  1, 1, 1,
  20, 15, 12,
  20, 15, 12,
  20, 15, 12
), nrow = 6, byrow = TRUE)
print (f_con)
##      [,1] [,2] [,3]
## [1,]    1    1    1
## [2,]    1    1    1
## [3,]    1    1    1
## [4,]   20   15   12
## [5,]   20   15   12
## [6,]   20   15   12
# Define the right-hand side values for the constraints
rhs <- c(750, 900, 450, 13000, 12000, 5000)


# Define the direction of the inequalities (all <= constraints)
dir <- rep("<=", 6)

# Define the coefficients of the equation
coeff_x1 <- 900
coeff_x2 <- 1200
coeff_x3 <- 750

# Calculate the Total Sales as the sum of the coefficients
Total_Sales <- coeff_x1 + coeff_x2 + coeff_x3

# Sales forecasts constraint (optional, if provided)
if (Total_Sales <= max(rhs)) {
  rhs <- c(rhs, Total_Sales)
  f_con <- cbind(f_con, c(coeff_x1, coeff_x2, coeff_x3))
  dir <- c(dir, "<=")
} else {
  cat("Sales Forecasts constraint cannot be added as it exceeds plant capacity.\n")
}

# Solve the linear programming problem
lp_solution <- lp(direction = "max", objective.in = f_obj, const.mat = f_con, const.dir = dir, const.rhs = rhs)
## Warning in rbind(const.mat, const.dir.num, const.rhs): number of columns of
## result is not a multiple of vector length (arg 2)
# Print the optimal solution
print(lp_solution)
## Success: the objective function is 420
# Calculate the optimal solution
x1 <- lp_solution$solution[1]  # Number of large-sized units to be produced
x2 <- lp_solution$solution[2]  # Number of medium-sized units to be produced
x3 <- lp_solution$solution[3]  # Number of small-sized units to be produced
max_profit <- lp_solution$objval  # Maximum Profit (Z)

# Print the results
cat("Number of large-sized units to be produced (x1):", x1, "\n")
## Number of large-sized units to be produced (x1): 1
cat("Number of medium-sized units to be produced (x2):", x2, "\n")
## Number of medium-sized units to be produced (x2): 0
cat("Number of small-sized units to be produced (x3):", x3, "\n")
## Number of small-sized units to be produced (x3): 0
cat("Maximum Profit (Z):", max_profit, "\n")
## Maximum Profit (Z): 420
![image](https://github.com/JavadbayKhalilzada/Fall-2023-QUANTITATIVE-MANAGEMENT-MODELING-BA-64018-006/assets/123045073/4b8e903c-e8f0-4bd6-85a3-615243fd81e0)
