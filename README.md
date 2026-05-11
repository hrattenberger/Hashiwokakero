# Hashiwokakero Implementation

This project implements three solvers to solve the **Hashiwokakero (Hashi)** puzzle, including HashiSolver (deduction-based), HashiSAT (SAT-based), HashiSMT (SMT-based). It also provides puzzle generation, explanation tools, and benchmarking utilities.

## Features
- Puzzle generation with configurable properties
- Deduction-based solver (`HashiSolver`)
- SAT-based solver (`HashiSAT`) with different strategies
- SMT-based solver (`HashiSMT`) with different strategies
- Solution explanation and optional visualization
- Benchmarking for performance comparison

## Solvers

### HashiSolver
Deduction-based solver for Hashi puzzles.

### HashiSAT
SAT-based solver with strategies:
- Brute Force (inefficient)
- Reachability
- Mixed approach

### HashiSMT
SMT-based solver with strategies:
- Reachability
- Mixed approach

## Benchmarking
The project includes a test suite to evaluate solver performance. It also includes benchmark utilities and benchmark results presented in the thesis.

## Dependencies
On Ubuntu Linux 24, at least the following dependencies are required:
- `sudo apt install python3-pandas  --no-install-suggests`
- `sudo apt install python3-matplotlib  --no-install-suggests`
- `sudo apt install python3-z3`

In a virtual python env:
- `pip3 install pysat`
- `pip3 install python-sat[aiger,approxmc,cryptosat]`

## Usage Example

```python
filehandler=filehandler()
generator=Generator(filehandler)
generator.generatePuzzles(20,100,property=propertyType.UNIQUE, testSuite=False)

puzzleCollection=[]
puzzleCollection=filehandler.getPuzzles(20,100, property=propertyType.UNIQUE)
print(f"{len(puzzleCollection)} Puzzles")

i=1

allSolutions=True
visualise=False
timeout=180

for puzzleItem in puzzleCollection:
    print(f"Puzzle {i}:")
    puzzle=puzzleItem.puzzle
    i+=1
    
    hashisolver=HashiSolver(puzzle, timeout=timeout)
    solutions_hashisolver=hashisolver.solve(allSolutions)
    explain("HashiSol", puzzle, solutions_hashisolver, allSolutions=allSolutions, visualise=visualise)

    hashisat = HashiSAT(puzzle, timeout=timeout)
    solution_hashisat=hashisat.solve(allSolutions=allSolutions, mode=SATstrategies.REACHABILITY)
    explain("HashiSAT", puzzle, solution_hashisat, allSolutions=allSolutions, visualise=visualise)

    hashisat = HashiSAT(puzzle, timeout=timeout)
    solution_hashisat=hashisat.solve(allSolutions=allSolutions, mode=SATstrategies.MIXED_APPROACH)
    explain("HashiSol", puzzle, solutions_hashisolver, allSolutions=allSolutions, visualise=visualise)

    hashismt = HashiSMT(puzzle, timeout=timeout)
    solutions_hashismt=hashismt.solve(allSolutions=allSolutions, mode=SMTstrategies.REACHABILITY)
    explain("HashiSMT", puzzle, solutions_hashismt, allSolutions=allSolutions, visualise=visualise)

    hashismt = HashiSMT(puzzle, timeout=timeout)
    solutions_hashismt=hashismt.solve(allSolutions=allSolutions, mode=SMTstrategies.MIXED_APPROACH)
    explain("HashiSMT", puzzle, solutions_hashismt, allSolutions=allSolutions, visualise=visualise)
```
