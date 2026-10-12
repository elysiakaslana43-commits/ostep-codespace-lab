# Chapter 7: CPU Scheduling Analysis

## Question 1
For three jobs with lengths 200, 200, and 200, FIFO and SJF produce the same schedule because all jobs have equal lengths. Their response times are 0, 200, and 400, and their turnaround times are 200, 400, and 600. The average response time is 200 and the average turnaround time is 400.

## Question 2
For job lengths 100, 200, and 300, FIFO and SJF produce the same schedule because the jobs are already ordered from shortest to longest. The response times are 0, 100, and 300, and the turnaround times are 100, 300, and 600. The average response time is 133.33 and the average turnaround time is 333.33.

## Question 3
With RR and a quantum of 1, the simulator reports response times of 0, 1, and 2; turnaround times of 298, 499, and 600; and waiting times of 198, 299, and 300. The average response time is 1.00, average turnaround time is 465.67, and average waiting time is 265.67.

## Question 4
SJF and FIFO have the same turnaround times when the jobs are already ordered by nondecreasing run time in the FIFO input order. They also have the same results when all jobs have equal lengths. In these cases, FIFO does not make a longer job run before a shorter one.

## Question 5
SJF and RR can have the same response times when the workload and quantum make their execution orders and first-start times coincide. For example, if jobs are listed in shortest-first order and the RR quantum is at least as long as every job, RR completes each job before moving to the next, so its schedule matches SJF. If all jobs have equal lengths and the quantum equals that length, their response times also match.

## Question 6
As job lengths increase, SJF response times generally increase because each job must wait for the shorter jobs scheduled before it. If all job lengths are multiplied by the same factor while the order remains unchanged, the response times are multiplied by that factor as well. This trend can be demonstrated by running the simulator with increasingly larger job lengths and comparing the reported response times.
For job lengths 10, 20, and 30, the response times are 0, 10, and 30, with an average of 13.33. For lengths 100, 200, and 300, they are 0, 100, and 300, with an average of 133.33. For lengths 1000, 2000, and 3000, they are 0, 1000, and 3000, with an average of 1333.33. The results show that response times scale linearly when all job lengths are multiplied by the same factor.

## Question 7
For RR, when N jobs are ready and each of the jobs ahead of a given job uses a full quantum q before the given job gets its first turn, the worst-case response time is (N - 1)q. Thus, the worst-case response time increases linearly with the quantum length q for a fixed N, under these assumptions. If some jobs finish before using a full quantum, the actual response time may be smaller.