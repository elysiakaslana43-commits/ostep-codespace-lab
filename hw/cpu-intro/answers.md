# OSTEP Ch.4 Homework Answers
## Q1
- Prediction / 预测：PID 0 先运行 5 个 tick，然后 PID 1 运行 5 个 tick。
- Reasoning / 理由：两个进程都使用 CPU，没有 I/O 阻塞。调度器会让一个进程运行完自己的 5 个 tick 后，再运行另一个进程。
- Verified result / 验证结果：PID 0 运行 5 个 tick，随后 PID 1 运行 5 个 tick；总时间为 10，CPU Busy 为 100%，IO Busy 为 0%。
- Analysis / 分析：预测与实际结果一致。由于两个进程都不等待 I/O，CPU 从头到尾都处于忙碌状态。
## Q2
- Prediction / 预测：PID 0 先运行 4 个 tick 并完成；然后 PID 1 运行 1 个 tick 后进行 I/O 并进入 BLOCKED。I/O 完成后 PID 1 回到 CPU，再运行 1 个 tick 并完成。
- Reasoning / 理由：PID 0 需要 4 个 CPU tick，而 PID 1 只有 1 个 CPU tick 后就开始 I/O。I/O 期间 PID 1 被阻塞，因此 CPU 可以等待 I/O 完成；I/O 完成后 PID 1 继续执行剩余的 CPU 时间。
- Verified result / 验证结果：Total Time = 11，CPU Busy = 6 (54.55%)，IO Busy = 5 (45.45%)。PID 1 在 I/O 期间处于 BLOCKED，I/O 完成后继续运行。
- Analysis / 分析：预测与实际结果一致。PID 1 的 I/O 使其进入 BLOCKED 状态，I/O 完成后重新获得 CPU。总 CPU 时间为 4 + 1 + 1 = 6 tick，I/O 时间为 5 tick。
## Q3
- Prediction / 预测：PID 0 先运行并完成；随后 PID 1 运行，在 I/O 阻塞后进入 BLOCKED。I/O 完成后 PID 1 返回 CPU 并完成。
- Reasoning / 理由：PID 1 的 I/O 会使它进入 BLOCKED 状态。在等待 I/O 时，CPU 可以执行其他进程；I/O 完成后 PID 1 从 BLOCKED 返回并继续执行。
- Verified result / 验证结果：Total Time = 7，CPU Busy = 6 (85.71%)，IO Busy = 5 (71.43%)。PID 1 在 I/O 期间处于 BLOCKED，随后 I/O 完成。
- Analysis / 分析：预测与实际结果一致。I/O 操作会使进程暂时离开 CPU 并进入 BLOCKED，I/O 完成后再恢复运行。
## Q4
- Prediction / 预测：PID 0 运行后进入 I/O 并被 BLOCKED，但由于使用 SWITCH_ON_END，系统不会因为 I/O 阻塞立即切换到 PID 1。CPU 等待 I/O 完成后继续，之后 PID 1 运行并完成。
- Reasoning / 理由：SWITCH_ON_END 表示只有进程结束时才进行上下文切换。因此进程进入 BLOCKED 后不会立即触发切换，CPU 在 I/O 期间可能处于空闲状态。
- Verified result / 验证结果：Total Time = 11，CPU Busy = 6 (54.55%)，IO Busy = 5 (45.45%)。
- Analysis / 分析：预测与实际结果一致。与正常情况下 I/O 阻塞后立即切换不同，SWITCH_ON_END 会让 CPU 在 I/O 等待期间无法立即执行另一个 READY 进程，因此出现 CPU 空闲。
## Q5
- Prediction / 预测：PID 0 运行 1 个 tick 后发起 I/O，并立即切换到 PID 1。PID 1 在 CPU 上运行 4 个 tick；I/O 完成后 PID 0 返回并完成。
- Reasoning / 理由：使用 SWITCH_ON_IO 时，进程发起 I/O 后会立即进行上下文切换，因此 CPU 可以执行 PID 1，而不会像 Q4 一样在 I/O 期间空闲。
- Verified result / 验证结果：Total Time = 7，CPU Busy = 6 (85.71%)，IO Busy = 5 (71.43%)。
- Analysis / 分析：预测与实际结果一致。相比 Q4，SWITCH_ON_IO 让 CPU 在 PID 0 等待 I/O 时执行 PID 1，因此减少了 CPU 空闲时间，总执行时间从 11 降低到 7。
## Q6
- Prediction / 预测：PID 0 发起 I/O 后进入 BLOCKED，CPU 切换到其他进程。PID 0 的 I/O 完成后不会立即再次运行，而是等待调度；在等待期间 I/O 设备可能处于空闲状态。
- Reasoning / 理由：`SWITCH_ON_IO` 会在 PID 0 发起 I/O 时切换到其他进程，而 `IO_RUN_LATER` 表示 I/O 完成后不立即让 PID 0 运行，因此 PID 0 需要等待下一次调度。
- Verified result / 验证结果：Total Time = 26，CPU Busy = 16 (61.54%)，IO Busy = 15 (57.69%)。
- Analysis / 分析：实际结果符合预测。I/O 完成后 PID 0 不能立即再次运行，导致 I/O 密集型进程再次发起 I/O 的时间延后，因此 I/O 设备在部分时间内会处于空闲状态。
## Q7
- Prediction / 预测：PID 0 发起 I/O 后进入 BLOCKED，CPU 切换到其他进程；当 PID 0 的 I/O 完成后，由于使用 IO_RUN_IMMEDIATE，PID 0 会立即重新运行。
- Reasoning / 理由：IO_RUN_IMMEDIATE 让 I/O 完成的进程立即获得 CPU，因此 PID 0 可以更快地继续执行并再次发起 I/O。
- Verified result / 验证结果：Total Time = 21，CPU Busy = 16 (76.19%)，IO Busy = 15 (71.43%)。
- Analysis / 分析：预测与实际结果一致。与 Q6 的 IO_RUN_LATER 相比，IO_RUN_IMMEDIATE 让 PID 0 在 I/O 完成后立即运行，使 I/O 密集型进程能够更快地再次进行 I/O，从而提高 I/O 设备利用率，并将总时间从 26 降低到 21。
## Q8
- Prediction / 预测：由于每个进程都有 3 条指令，且每条指令有 50% 的概率是 CPU 指令、50% 的概率是 I/O 指令，不同的随机 seed 可能产生不同的 CPU/I/O 指令序列。因此三个 seed 的运行轨迹不一定相同。
- Reasoning / 理由：`-s` 改变随机数种子，从而改变每个进程 3 条指令的 CPU/I/O 类型以及执行顺序。调度规则本身不变，但输入的指令序列会变化。
- Verified result / 验证结果：Seed 1 中 PID 0 为 1 CPU + 2 I/O，PID 1 为 3 CPU；Seed 2 中 PID 0 为 1 CPU + 2 I/O，PID 1 为 1 CPU + 2 I/O；Seed 3 中 PID 0 为 2 CPU + 1 I/O，PID 1 为 1 CPU + 2 I/O。
- Analysis / 分析：三个 seed 产生了不同的指令序列，因此实际运行轨迹也不同。这说明随机 seed 会影响进程的 CPU/I/O 行为；而调度策略保持不变时，变化的主要来源是生成的指令列表不同。