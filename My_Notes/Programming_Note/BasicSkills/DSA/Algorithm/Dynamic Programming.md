# Dynamic Programming

tags: #dynamic_programming #python

Dynamic programming is an algorithmic technique for solving problems by breaking them down into smaller subproblems and using the solutions to those subproblems to solve the original problem. It is a powerful technique that can be used to solve a wide variety of problems, including knapsack problems, Fibonacci numbers, and longest common subsequences.

## References

* [Dynamic Programming](https://en.wikipedia.org/wiki/Dynamic_programming)
* [Introduction to Dynamic Programming](https://www.coursera.org/lecture/algorithms-part1/introduction-to-dynamic-programming-1I166)
* [Dynamic Programming in Python](https://www.tutorialspoint.com/dynamic_programming/dynamic_programming_python.htm)

## What is Dynamic Programming?

Dynamic programming is a recursive technique that stores the results of subproblems so that they can be reused when solving larger problems. This is in contrast to other recursive techniques, which typically solve each subproblem from scratch.

## How Does Dynamic Programming Work?

Dynamic programming works by breaking down a problem into smaller subproblems. The solutions to these subproblems are then stored in a table or other data structure. When the original problem is solved, the solutions to the subproblems are used to find the solution to the original problem.

## Example

Let's look at an example of how dynamic programming can be used to solve the Fibonacci numbers problem. The Fibonacci numbers are a sequence of numbers where each number is the sum of the two previous numbers. The first two numbers in the sequence are 0 and 1, and the rest of the numbers are generated by adding the two previous numbers.

The Fibonacci numbers can be solved using dynamic programming by breaking the problem down into smaller subproblems. The first subproblem is to find the first two Fibonacci numbers. The second subproblem is to find the third Fibonacci number, and so on.

The solutions to these subproblems can be stored in a table. The table will have one row for each Fibonacci number, and the value in each row will be the solution to the corresponding subproblem.

To solve the original Fibonacci numbers problem, we can simply look up the solution in the table. The solution for the first Fibonacci number is 0, the solution for the second Fibonacci number is 1, and so on.

## Example Code

```python
def fibonacci(n):
    if n == 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fibonacci(n - 1) + fibonacci(n - 2)


def main():
    for i in range(10):
        print(fibonacci(i))


if __name__ == "__main__":
    main()
```

This code first defines a function called `fibonacci` that takes an integer `n` as input and returns the `n`th Fibonacci number. The function works by recursively calling itself to find the (n-1)th and (n-2)th Fibonacci numbers, and then adding those two numbers together.

The code then defines a function called `main` that simply prints out the first 10 Fibonacci numbers. The `main` function calls the `fibonacci` function for each value of `i` from 0 to 9.

To run this code, you can save it as a Python file and then run it from the command line. For example, if you save the code as `fibonacci.py`, you can run it by typing the following command into the command line:

```bash
python fibonacci.py
```