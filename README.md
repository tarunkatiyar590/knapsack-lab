# Knapsack Lab

**An interactive visualizer that compares the greedy and dynamic programming solutions of the knapsack problem.**

Built with plain HTML, CSS and JavaScript. No installation, no libraries, no server. Open `index.html` in a browser and it works.

> Academic project for **Design and Analysis of Algorithms**

---

## Why this project?

The knapsack problem is the standard example for comparing two algorithm design techniques:

| Version | Can items be split? | Best method |
|---|---|---|
| Fractional knapsack | Yes | Greedy by value/weight ratio (always optimal) |
| 0/1 knapsack | No | Dynamic programming (greedy by ratio can fail) |

Knapsack Lab lets you change the numbers and see the difference immediately, and it animates the dynamic programming table so you can watch each cell being computed.

## Features

- Editable item table (weight and value, up to 8 items) and capacity (up to 50)
- Three presets: **Greedy fails (0/1)**, **Textbook** and **Random**
- Result cards for three approaches, side by side:
  - Fractional greedy (optimal for the fractional problem)
  - Greedy by ratio on 0/1 (a heuristic, shown to demonstrate where greedy fails)
  - 0/1 dynamic programming (optimal)
- Fractional greedy table with the fraction of each item taken
- **Animated DP table** with Play, Step, Finish and Reset controls and a speed slider
- A plain-English explanation of every table cell as it is filled
- Backtrace highlighted in green at the end, showing which items were chosen
- Works offline, in light and dark mode, on desktop and mobile

## How to run

1. Download or clone this repository.
2. Double-click `index.html`.

That's all. To run it from a local server instead (optional):

```bash
python -m http.server 8000
# then open http://localhost:8000
```

## Example

Items: A (weight 10, value 60), B (20, 100), C (30, 120). Capacity 50.

| Method | Items taken | Total value |
|---|---|---|
| Fractional greedy | A, B and 2/3 of C | 240 |
| Greedy by ratio on 0/1 | A, B | 160 |
| 0/1 dynamic programming | B, C | **220** (optimal) |

Greedy by ratio picks the best-looking items first, but then C no longer fits and 20 units of capacity are wasted. Dynamic programming considers every combination of subproblems and finds the true optimum.

## Algorithms and complexity

| Algorithm | Solves | Time | Space | Optimal? |
|---|---|---|---|---|
| Fractional greedy | Fractional knapsack | O(n log n) | O(1) extra | Yes |
| Greedy by ratio | 0/1 knapsack | O(n log n) | O(1) extra | No |
| Dynamic programming | 0/1 knapsack | O(n·W) | O(n·W) | Yes |

DP recurrence, where `dp[i][w]` is the best value using the first `i` items with capacity `w`:

```
dp[0][w] = 0
dp[i][w] = dp[i-1][w]                                  if weight[i] > w
dp[i][w] = max(dp[i-1][w], value[i] + dp[i-1][w - weight[i]])   otherwise
```

More detail and a worked table are in [docs/ALGORITHMS.md](docs/ALGORITHMS.md).

## Project structure

```
knapsack-lab/
├── index.html              The complete application (HTML + CSS + JS)
├── README.md               This file
├── LICENSE                 MIT license
├── .gitignore
├── docs/
│   ├── ALGORITHMS.md       Algorithm explanation with a worked example
│   └── PUSH_TO_GITHUB.md   Step-by-step guide to publish this repository
└── tests/
    └── test_algorithms.js  Node.js tests for the algorithm code
```

## Code overview

| Function (in `index.html`) | Purpose |
|---|---|
| `fractional()` | Sorts by ratio and builds the fractional greedy solution |
| `greedy01()` | Greedy-by-ratio heuristic for 0/1 knapsack |
| `solveDP()` | Fills the DP table and backtraces the chosen items |
| `renderItems()`, `renderResults()`, `renderDP()` | Draw the interface |
| `tick()` | Timer behind the Play button and speed slider |

## Running the tests

The tests need [Node.js](https://nodejs.org) (no packages to install). They load the algorithm code directly from `index.html`.

```bash
node tests/test_algorithms.js
```

They check hand-calculated examples, edge cases, and compare the DP result against a brute-force search on 500 random inputs.

## Limitations

- Capacity is limited to 50 and items to 8 so that the table stays readable
- Weights and capacity must be whole numbers
- Only the full 2D DP table is shown (no space-optimised 1D version)

## Future work

- Space-optimised 1D dynamic programming
- Branch and bound (LC and FIFO) for 0/1 knapsack
- Benchmark chart of running time against n and W
- CSV import and export of item lists
## License

Released under the [MIT License](LICENSE).
