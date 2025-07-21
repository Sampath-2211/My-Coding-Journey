# Fibonacci with LangGraph: A Workflow Approach

## Traditional Fibonacci Implementation

The standard recursive approach calculates Fibonacci numbers by repeatedly calling itself:

```python
def fib(x):
    if ((x==0)|(x==1)):
        return x
    else:
        return fib(x-1)+fib(x-2)

x = int(input("Enter the number: "))
print("Here is the series:")
for i in range (0,x):
    print(fib(i))
```

This method recalculates the same values multiple times. For example, to find F(5), it calculates F(3) twice, F(2) three times, and F(1) five times.

## LangGraph Workflow Implementation

The LangGraph version transforms Fibonacci calculation into a structured workflow with three distinct nodes:

### Node Structure

**Initialize Node**: Sets up the starting values F(0)=0, F(1)=1
```python
def initialize_node(state: State):
    return {
        "current": 1,
        "previous": 0,
        "count": 2,
        "series": [0, 1]
    }
```

**Calculate Node**: Generates the next Fibonacci number using the two previous values
```python
def calculate_node(state: State):
    next_fib = state["current"] + state["previous"]
    return {
        "current": next_fib,
        "previous": state["current"],
        "count": new_count,
        "series": new_series
    }
```

**Check Node**: Determines whether to continue or stop based on target count
```python
def check_node(state: State):
    if state["count"] < state["target"]:
        return "continue"
    else:
        return "stop"
```

### Workflow Flow
```
Initialize → Calculate → Check → (Loop back to Calculate OR End)
```

## Key Improvements

### Eliminates Redundant Calculations
Each Fibonacci number is calculated exactly once. The workflow maintains the previous two values in state, eliminating the exponential time complexity of the recursive approach.

### Process Transparency  
Every step is visible and logged:
```
Initialize Node: Setting up Fibonacci series
Calculate Node: F(2) = 1 + 0 = 1
Calculate Node: F(3) = 1 + 1 = 2
Calculate Node: F(4) = 2 + 1 = 3
```

### Complete Series Storage
The workflow maintains the entire Fibonacci series in memory, allowing for immediate access to any previously calculated value without recomputation.

### State Management
All intermediate values, counters, and the complete series are managed through a structured state system:
```python
class State(TypedDict):
    current: int      # Current Fibonacci number
    previous: int     # Previous Fibonacci number  
    count: int        # Numbers generated so far
    target: int       # Target count to generate
    series: List[int] # Complete series
```

### Scalable Performance
The linear time complexity O(n) makes this approach practical for generating large Fibonacci sequences. Calculating F(100) takes 100 iterations instead of 2^100 recursive calls.

### Modular Design
Each operation (initialization, calculation, checking) is isolated in its own node, making the code maintainable and extensible. Additional processing nodes can be easily added to the workflow.

## Execution Example

For generating 5 Fibonacci numbers:
- **Traditional method**: 15 function calls (with overlapping calculations)
- **LangGraph method**: 5 clean iterations with complete audit trail

The workflow approach transforms a computationally expensive recursive problem into an efficient, transparent, and maintainable solution.
