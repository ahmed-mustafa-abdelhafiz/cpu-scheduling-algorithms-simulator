# CPU Scheduling Algorithms Simulator (C++)

An interactive, console-based simulator of classic operating-system CPU scheduling algorithms, written in object-oriented C++ to demonstrate process scheduling strategies. The program reads a set of processes from a file, schedules them with the algorithm you choose, and prints a Gantt-style execution trace along with key performance statistics such as average waiting time, turnaround time, and CPU utilization.

## Implemented Algorithms

<details>
<summary><strong>FCFS (First-Come, First-Served)</strong></summary>

- **Type:** Non-preemptive
- **Selection Rule:** Earliest arrival time
- **Tie-Breaking:** Input order

</details>

<details>
<summary><strong>SJF (Shortest Job First)</strong></summary>

- **Type:** Non-preemptive
- **Selection Rule:** Shortest burst time among ready processes
- **Tie-Breaking:** Earlier arrival time

</details>

<details>
<summary><strong>Priority</strong></summary>

- **Type:** Non-preemptive
- **Selection Rule:** Highest priority among ready processes
- **Tie-Breaking:** Earlier arrival, then shorter burst

</details>

<details>
<summary><strong>RR (Round Robin)</strong></summary>

- **Type:** Preemptive
- **Selection Rule:** FIFO ready queue with a fixed time quantum
- **Tie-Breaking:** Earlier arrival, then shorter burst

</details>

<details>
<summary><strong>Priority + RR</strong></summary>

- **Type:** Hybrid
- **Selection Rule:** Highest priority first, and processes with equal priority are scheduled using Round Robin
- **Tie-Breaking:** Fewer executions so far, then earlier arrival

</details>

> **Note:** In this simulator a **larger priority number means higher priority** (e.g. priority `3` runs before priority `1`).

## How It Works

1. Processes are loaded from `data.txt` into the scheduler as Process Control Blocks holding arrival time, burst time, and priority.
2. You pick an algorithm from the interactive menu.
3. The scheduler simulates execution, recording every CPU slice (`ExecutionSlice`) to build the execution history.
4. When all processes terminate, the program prints:
   - The input process table
   - The execution order (a text-based Gantt chart: start time, end time, PID)
   - **Average waiting time** and **CPU utilization**
   - Per-process **end time**, **turnaround time** (completion − arrival), and **waiting time** (turnaround − burst)

## Project Structure

```
├── main.cpp          # Entry point: interactive menu + reads processes from data.txt
├── Scheduler.h       # Scheduler class, PCB & ExecutionSlice structs, TIME_QUANTUM
├── Scheduler.cpp     # Core scheduler: process creation, dispatching, statistics & reports
├── FCFS.cpp          # First-Come, First-Served implementation
├── SJF.cpp           # Shortest Job First (non-preemptive) implementation
├── Priority.cpp      # Priority scheduling (non-preemptive) implementation
├── RR.cpp            # Round Robin implementation
├── PriorityRR.cpp    # Priority + Round Robin hybrid implementation
├── data.txt          # Input processes (one per line)
└── run.exe           # Prebuilt Windows executable
```

## Input Format (`data.txt`)

Each line defines one process:

```
<name> <arrivalTime> <burstTime> <priority>
```

Example (included in the repo):

```
T1 0 5 1
T2 0 3 1
T3 2 3 2
T4 3 6 3
```

Edit `data.txt` to test your own process sets — no recompilation needed.

## Getting Started

### Prerequisites

- A C++ compiler (e.g. `g++` / MinGW on Windows)

### Build

```bash
g++ -o run main.cpp Scheduler.cpp FCFS.cpp SJF.cpp Priority.cpp RR.cpp PriorityRR.cpp
```

### Run

```bash
./run        # Linux / macOS
run.exe      # Windows (prebuilt binary, or build your own as above)
```

Make sure `data.txt` is in the same directory as the executable.

## Usage

When the program starts, choose an algorithm by typing its name:

```
Choose scheduling algorithm
[FCFS], [SJF], [PRIORITY], [RR], [PRIORITY_RR], [EXIT]
```

### Sample Session (Round Robin, quantum = 2)

```
The input processes ...
Process  ArvlTime  burstTime  Priority
1        0         5          1
2        0         3          1
3        2         3          2
4        3         6          3

The execution order:
From  To    PID
0     2     P2
2     4     P1
4     6     P3
6     8     P4
8     9     P2
9     11    P1
11    12    P3
12    14    P4
14    15    P1
15    17    P4

Average waiting time = 7.75
CPU Utilization = 100%

PID  ArvlTime  EndTime  TrnArndTime  WaitingTime
2    0         9        9            6
3    2         12       10           7
1    0         15       15           10
4    3         17       14           8
```

The menu loops after every run, so you can compare all five algorithms on the same input back to back.
