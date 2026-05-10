# Mathematical-Physics-Practical-Semester-2

# ================================================================
# MATHEMATICAL PHYSICS - 2
# Motilal Nehru College, University of Delhi
# All 14 Programs
# ================================================================


# ================================================================
# PROGRAMME - 01
# AIM: Approximate nth root of a number up to a given number of
#      significant digits using Newton-Raphson method.
# ================================================================

def newton_raphson_nth_root(number, n, initial_guess, tolerance):
    # define the function whose root we want to find
    def f(x):
        return x**n - number

    # define the derivative of the function
    def f_prime(x):
        return n * x**(n-1)

    # start with the initial guess
    x = initial_guess

    # iteratively apply the newton raphson method
    while True:
        # calculate the next approximation using newton-raphson formula
        x_next = x - f(x) / f_prime(x)
        # check for convergence
        if abs(x_next - x) < tolerance:
            break
        # update the current approximation
        x = x_next
    return x_next

# take the input from the user
number = float(input("enter the number: "))
n = float(input("enter the root value: "))
initial_guess = float(input("enter the initial guess : "))
tolerance = float(input("enter the tolerance :"))

# calculate the nth root using newton-raphson method
root = newton_raphson_nth_root(number, n, initial_guess, tolerance)
# output the result
print("the approximate nth root is : ", root)


# ================================================================
# PROGRAMME - 02
# AIM: Approximate nth root of a number up to a given number of
#      significant digits using Bisection method.
# ================================================================

def bisection_nth_root(n, m, digits):
    # define the function whose root we want to find
    def f(x):
        return x**n - m

    # initialize the interval (a,b)
    a, b = 0, m
    # set the tolerance based on significant digits
    tol = 10**(-digits)
    # perform the bisection until the interval is small enough
    while b-a > tol:
        # calculate the mid point of the interval
        c = (a+b) / 2
        # check if the function value at c is exactly equal to zero
        if f(c) == 0:
            return c
        # update the interval based on sign change of the interval
        elif f(a) * f(c) < 0:
            b = c
        else:
            a = c
    # round the result to the specified number of significant digits
    return round(c, digits)

# take the input from the user
n = float(input("enter the nth root : "))
m = float(input("enter the number of which nth root has to be found: "))
digits = int(input("ente the number of significant digits: "))
# calculate the nth root using bisection method
root = bisection_nth_root(n, m, digits)
# output the result
print("the nth root of ", m, "is approximately: ", root)


# ================================================================
# PROGRAMME - 03
# AIM: Approximate nth root of a number up to a given number of
#      significant digits using Secant method.
# ================================================================

def secant_nth_root(number, n, x0, x1, tolerance):
    # nested function representing the function whose root we are trying to find
    def f(x):
        return x**n - number

    # initialize x_prev and x_curr with initial guesses
    x_prev, x_curr = x0, x1
    while abs(x_curr - x_prev) > tolerance:
        # calculate the next approximation using the secant method formula
        x_next = x_curr - f(x_curr) * (x_curr - x_prev) / (f(x_curr) - f(x_prev))
        # update x_prev and x_curr for the next iteration
        x_prev, x_curr = x_curr, x_next
    # return the approximate root rounded to the nearest integer
    return round(x_curr)

# take input from user
number = int(input("enter the number for which we want to find the nth root: "))
n = int(input("enter the nth root value: "))
initial_guess1 = float(input("enter the first initial guess : "))
initial_guess2 = float(input("enter the second initial guess : "))
tolerance = float(input("enter the tolerance : "))

# call the function and print the result
root = secant_nth_root(number, n, initial_guess1, initial_guess2, tolerance)
print("the nth root is approximately: ", root)


# ================================================================
# PROGRAMME - 04
# AIM: Determine the depth up to which a spherical homogeneous
#      object sinks into a fluid using Bisection method.
# ================================================================

import math

# function to calculate depth of immersion
def depth_of_immersion(radius, object_density, fluid_density):
    ratio = object_density / fluid_density
    depth = radius * ((ratio) ** (1/3))
    return depth

# function to implement the bisection method
def bisection(a, b, n, object_density, fluid_density):
    i = 1
    condition = True
    while condition:
        x = (a+b) / 2
        # check if depth_of_immersion(x) is negative
        if depth_of_immersion(x, object_density, fluid_density) < 0:
            a = x
        else:
            b = x
        # print iteration information
        print("iteration =", "x= ", x, "depth of immersion(x) = ",
              depth_of_immersion(x, object_density, fluid_density))
        if i == n:
            condition = False
        else:
            i = i + 1
    print("required root is: ", x)

# take initial input from user
a = float(input("first approximation root: "))
b = float(input("second approximation root: "))
n = int(input("no. of iteration: "))
object_density = float(input("object density: "))
fluid_density = float(input("fluid density: "))

# check if the given initial approximation bracket the root
if depth_of_immersion(a, object_density, fluid_density) * depth_of_immersion(b, object_density, fluid_density) > 0:
    print("given approximation does not bracket the root")
    print("try again with different values")
else:
    # apply the bisection method to find the root
    bisection(a, b, n, object_density, fluid_density)


# ================================================================
# PROGRAMME - 05
# AIM: Determine the depth up to which a spherical homogeneous
#      object sinks into a fluid using Secant method.
# ================================================================

import math

# define the volume equation for a spherical object
def volume_equation(depth):
    radius = depth / 2
    return (4/3) * math.pi * radius**3

# implement the secant method to solve the equation
def secant_method(x0, x1, tolerance=10**-6, max_iteration=100):
    for _ in range(max_iteration):
        # calculate function values at x0 and x1
        fx0 = volume_equation(x0)
        fx1 = volume_equation(x1)
        # check if the difference between consecutive iteration is within tolerance
        if abs(fx1-fx0) < tolerance:
            return x1
        # calculate the next approximation using the secant method formula
        x_n = x1 - fx1 * ((x1 - x0) / (fx1 - fx0))
        # update the value for next iteration
        x0, x1 = x1, x_n
    # if the method doesn't converge within specified iterations
    print("secant method did not converge within specified number of iteration.")
    return None

# take initial guess from user
x0 = float(input("enter the first initial guess: "))
x1 = float(input("enter the second initial guess: "))
# set tolerance and maximum number of iterations
tolerance = 10**-6
max_iterations = 100

# apply the secant method for the depth of spherical object
depth = secant_method(x0, x1, tolerance, max_iterations)
# output the result
if depth is not None:
    print("the depth of spherical object is approximately: ", depth)
else:
    print("unable to find the depth within the specified tolerance and maximum number of iterations.")


# ================================================================
# PROGRAMME - 06
# AIM: Solve transcendental equation like a = tan(a)
#      using Secant method.
# ================================================================

import math

# define the equation
def equation(alpha):
    return alpha - math.tan(alpha)

# implement the secant method
def secant_method(x0, x1, tolerance=10**-6, max_iterations=100):
    # iterate using secant method
    for _ in range(max_iterations):
        fx0 = equation(x0)
        fx1 = equation(x1)
        # check if the difference between consecutive iterations is within tolerance
        if abs(fx1 - fx0) < tolerance:
            return x1
        # calculate the next approximation using secant method formula
        x_next = x1 - fx1 * ((x1-x0) / (fx1-fx0))
        # update value for next iteration
        x0, x1 = x1, x_next
    # if the method doesn't converge within the specified iterations
    return None

# take the initial guess from the user
x0 = float(input("enter x0: "))
x1 = float(input("enter x1: "))

# set tolerance and maximum number of iterations
tolerance = 10**-6
max_iterations = 100

# apply secant method to find the root
root = secant_method(x0, x1, tolerance, max_iterations)
# output the result
if root is not None:
    print("the root of the equation aplha = tan(alpha) is approximately: ", root)
else:
    print("unable to find the root within the specified and maximum number of iterations: ")


# ================================================================
# PROGRAMME - 07
# AIM: Solve transcendental equation like a = tan(a)
#      using Newton-Raphson method.
# ================================================================

import math

# define the equation
def equation(x):
    return x - math.tan(x)

# define the derivative of the equation
def derivative(x):
    return 1 - 1 / math.cos(x)**2

# implement the newton raphson method
def newton_raphson(guess, tolerance=10**-6, max_iterations=100):
    x_prev = guess
    for _ in range(max_iterations):
        # calculate the value of the equation and its derivative at x_prev
        fx = equation(x_prev)
        f_prime_x = derivative(x_prev)
        # check if the derivative is close to zero (potential division by zero)
        if abs(f_prime_x) < 10**-6:
            print("derivative close to zero, unable to continue.")
            return None
        # calculate the next approximation using newton raphson formula
        x_next = x_prev - fx / f_prime_x
        # check for convergence
        if abs(x_next - x_prev) < tolerance:
            return x_next
        # update x_prev for next iteration
        x_prev = x_next
    # if the method doesn't converge within the specified iterations
    print("newton raphson did not converge within the specified number of iterations.")
    return None

guess = float(input("enter the initial guess: "))
# set the tolerance and maximum number of iterations
tolerance = 10**-6
max_iterations = 100
# apply the newton raphson method to find the roots
root = newton_raphson(guess, tolerance, max_iterations)
# output the result
if root is not None:
    print("the root of the the equation x - tan(x) is approximately: ", root)
else:
    print("unable to find the root within the specified olerance and maximum number of iterations.")


# ================================================================
# PROGRAMME - 08
# AIM: Least squares fitting for linear equation y = ax + b
#      and determine parameters with uncertainties.
# ================================================================

import matplotlib.pyplot as plt
from scipy.stats import linregress

def fit(n):
    x = [None]*n
    y = [None]*n
    print("input the value of x and y")
    for i in range(0, n):
        x[i] = float(input())
        y[i] = float(input())
    xy = 0
    sumx = 0
    sumy = 0
    sqx = 0
    for i in range(0, n):
        xy = xy + x[i]*y[i]
        sumx = sumx + x[i]
        sumy = sumy + y[i]
        sqx = sqx + x[i]**2
    a1 = (n*xy - sumx*sumy) / (n*sqx - sumx**2)
    a0 = sumy/n - a1*sumx/n
    x1 = 0
    x2 = 0
    for i in range(1, n):
        if x[i] < x[i-1]:
            x1 = x[i]
        else:
            x2 = x[i]
    print("the value of slope is = ", a1)
    print("the value of y intercept is = ", a0)
    fit = linregress(x, y)
    print("by using inbuilt function")
    print("slope = ", fit.slope)
    print("y intercept = ", fit.intercept)
    plt.scatter(x, y)
    plt.title("Least square fitting")
    plt.xlabel("X axis")
    plt.ylabel("Y axis")
    y1 = a0 + a1*x2
    y2 = a0 + a1*x2
    yy = [y1, y2]
    xx = [x1, x2]
    plt.plot(xx, yy)
    plt.show()

n = int(input("input the number of elements= "))
print(fit(n))


# ================================================================
# PROGRAMME - 09
# AIM: Least squares fitting for power law y = ax^b
#      and estimate parameters with uncertainties.
# ================================================================

import numpy as np
import matplotlib.pyplot as plt

# given data
x = np.array([1, 2, 3, 4, 5])
y = np.array([2, 4, 9, 16, 25])

# taking algorithm
X = np.log(x)
Y = np.log(y)
# number of observations
n = len(x)

# required summations
sum_x = np.sum(X)
sum_y = np.sum(Y)
sum_xy = np.sum(X * Y)
sum_x2 = np.sum(X * X)

# calculating constants
b = (n * sum_xy - sum_x * sum_y) / (n * sum_x2 - sum_x**2)
ln_a = (sum_y - b * sum_x) / n
a = np.exp(ln_a)
print("a = ", a)
print("b = ", b)

# curve for plotting
x_curve = np.linspace(x.min(), x.max(), 200)
y_curve = a * (x_curve ** b)

# plotting
plt.scatter(x, y, label="data points")
plt.plot(x_curve, y_curve, label="power fit: y = a x^b")
plt.xlabel("x")
plt.ylabel("y")
plt.title("Power law")
plt.legend()
plt.show()


# ================================================================
# PROGRAMME - 10
# AIM: Least squares fitting for exponential law y = ae^(bx)
#      and estimate parameters with uncertainties.
# ================================================================

import numpy as np
import array as arr
import matplotlib.pyplot as plt

def fit(x, y):
    Y = [None]*n
    for i in range(0, n):
        Y[i] = np.log(y[i])
    xy = 0
    sum_x = 0
    sum_y = 0
    sqx = 0
    for i in range(0, n):
        xy = xy + x[i]*Y[i]
        sum_x = sum_x + x[i]
        sum_y = sum_y + Y[i]
        sqx = sqx + x[i]**2
    a1 = (n*xy - sum_x*sum_y) / (n*sqx - sum_x**2)
    a0 = (sum_y/n) - a1*(sum_x/n)
    a = 2.71828**a0
    b = a1
    print("y intercept = ", a)
    print("slope ", b)
    t = np.polyfit(x, Y, 1)
    print("slope by in bulit function = ", (2.71828**t[1]))
    print("y intercept by in bulit fucntion = ", t[0])
    mi = min(x)
    mx = max(x)
    xx = arr.array('d', [])
    yy = arr.array('d', [])
    va = 0
    for i in np.linspace(mi, mx, 100):
        xx.append(i)
        va = a*(2.71828)**(b*i)
        yy.append(va)
    # plotting
    plt.scatter(x, y, label="data points")
    plt.plot(xx, yy, label="Best fit curve")
    plt.xlabel("x-axis")
    plt.ylabel("y-axis")
    plt.title("Exponential Law")
    plt.legend()
    plt.show()

x = arr.array('d', [])
y = arr.array('d', [])
n = int(input("input the number of elements= "))
print("input the data value values of x and y")
for i in range(0, n):
    e = float(input())
    f = float(input())
    x.append(e)
    y.append(f)
print(fit(x, y))


# ================================================================
# PROGRAMME - 11
# AIM: Generate and plot Legendre polynomials using series
#      expansion and verify recurrence relation.
# ================================================================

import numpy as np
import matplotlib.pyplot as plt
from math import factorial

def legendre_series(n, x):
    Pn = 0
    for k in range(n // 2 + 1):
        coeff = ((-1)**k * factorial(2*n - 2*k)) / (2**n * factorial(k) * factorial(n-k) * factorial(n-2*k))
        Pn += coeff * x**(n-2*k)
    return Pn

def legendre_recurrence(n, x):
    if n == 0:
        return np.ones_like(x)
    elif n == 1:
        return x
    Pn_1 = x
    Pn_2 = np.ones_like(x)
    for i in range(2, n + 1):
        Pn = ((2*i-1)*x*Pn_1 - (i-1)*Pn_2) / i
        Pn_2 = Pn_1
        Pn_1 = Pn
    return Pn

x = np.linspace(-1, 1, 200)
plt.figure(figsize=(8, 6))
for n in range(6):
    y_series = np.array([legendre_series(n, xi) for xi in x])
    plt.plot(x, y_series, label=f"P{n}(x)")
plt.title("Legendre Polynomials (Series Expansion)")
plt.xlabel("x")
plt.ylabel("P_n(x)")
plt.legend()
plt.grid()
plt.show()

print("Verifying recurrence relation:\n")
for n in range(2, 6):
    lhs = legendre_recurrence(n, x)
    rhs = ((2*n-1)*x * legendre_recurrence(n-1, x) - (n-1)*legendre_recurrence(n-2, x)) / n
    error = np.max(np.abs(lhs - rhs))
    print(f"n = {n}, max error = {error:.6e}")


# ================================================================
# PROGRAMME - 12
# AIM: Generate and plot Hermite polynomials using series
#      expansion and verify recurrence relation.
# ================================================================

import numpy as np
import matplotlib.pyplot as plt
from math import factorial

# Hermite polynomial using series expansion
def hermite_series(n, x):
    result = 0
    for m in range(n // 2 + 1):
        coeff = ((-1)**m * factorial(n)) / (factorial(m) * factorial(n - 2*m))
        result += coeff * (2*x)**(n - 2*m)
    return result

# Generate x values
x = np.linspace(-3, 3, 400)

# Plot first few Hermite polynomials
plt.figure(figsize=(8, 6))

for n in range(6):
    y = hermite_series(n, x)
    plt.plot(x, y, label=f"H_{n}(x)")

plt.title("Hermite Polynomials (Series Expansion)")
plt.xlabel("x")
plt.ylabel("H_n(x)")
plt.legend()
plt.grid()
plt.show()


# ================================================================
# PROGRAMME - 13
# AIM: Verify the orthogonality of Legendre polynomials
#      using integral property over [-1, 1].
# ================================================================

import numpy as np
from numpy.polynomial.legendre import Legendre
from scipy.integrate import quad

def verify_legendre_orthogonality(n_max):
    """
    Verify orthogonality of legendre polynomials up to degree n_max
    """
    for m in range(n_max+1):
        Pm = Legendre.basis(m)
        for n in range(n_max+1):
            Pn = Legendre.basis(n)
            integral, _ = quad(lambda x: Pm(x)*Pn(x), -1, 1)

            if m == n:
                expected = 2/(2*n+1)
                print(f"∫P_{m}(x) P_{n}(x) dx = {integral:.5f}|Expected:{expected:.5f}")
            else:
                print(f"∫P_{m}(x) P_{n}(x) dx = {integral:.5e}|Expected:0")

verify_legendre_orthogonality(2)


# ================================================================
# PROGRAMME - 14
# AIM: Verify the properties of the Dirac Delta function
#      using its representation as a sequence of functions.
# ================================================================

import numpy as np
from scipy.integrate import quad
import matplotlib.pyplot as plt

def delta_n(x, n):
    return np.sqrt(n/np.pi)*np.exp(-n*x**2)

def f(x):
    return x**2 + 2*x + 1

n_values = [1, 5, 10, 50]
x_range = np.linspace(-1, 1, 1000)
plt.figure(figsize=(10, 6))
for n in n_values:
    plt.plot(x_range, delta_n(x_range, n), label=f"n={n}")

plt.title("Gaussian Approximation of Dirac Delta")
plt.xlabel("x")
plt.ylabel("delta_n(x)")
plt.show()

for n in n_values:
    integral, _ = quad(lambda x: f(x)*delta_n(x, n), -np.inf, np.inf)
    print(f"n={n}, ∫f(x)δ_n(x) dx = {integral}, f(0)={f(0)}")

    
