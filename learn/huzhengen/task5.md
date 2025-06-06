## 第五章：Monad 101
 
1. 请简述 Monad 中的“延迟执行”概念。该机制如何优化共识与执行之间的关系？

Monad中的"延迟执行"是一种将交易验证与执行分离的机制。

如何优化共识与执行之间的关系：
- 共识层只负责确定交易顺序，而不立即执行交易
- 执行层在共识完成后才进行实际交易执行
- 验证者仅验证交易的基本有效性，而非完整执行

2. 什么是乐观并行执行？其工作流程包含哪些关键步骤？

乐观并行执行是 Monad 的核心执行模型，其工作流程：

- 冲突检测：系统分析交易的读写集，预测可能存在的冲突
- 并行调度：基于冲突分析结果，将非冲突交易分配到不同的执行线程
- 投机执行：各线程同时执行交易，假设没有冲突发生
- 冲突验证：执行完成后，验证实际访问的状态是否与预测一致
- 结果提交或回滚：如没有冲突，提交执行结果；如有冲突，回滚并重新执行
- 线性化：尽管并行执行，但系统确保最终结果与线性顺序执行一致

3. 在 Monad 架构中，如何通过并行执行仍保持交易的线性排序与状态一致性？

通过以下机制保证交易的线性排序与状态一致性：
- 全局序列号：每个交易获得一个确定的全局序列号，这是由共识层决定的
- 依赖图构建：基于交易读写集分析，构建交易间的依赖关系图
- 拓扑排序执行：根据依赖图进行拓扑排序，确保有依赖关系的交易按正确顺序执行
- 冲突检测与重试：当检测到执行冲突时，系统会回滚并按照正确的顺序重新执行
- 原子提交：所有交易的执行结果要么全部提交，要么全部回滚，确保状态一致性
- 确定性结果验证：验证节点可以复现相同的执行过程，确保结果一致性

4. Monad 的共识机制 monadBFT 有哪些特点？它与传统 BFT 共识相比有何优势？

特点：
- 流水线式设计：将共识过程分为多个阶段并流水线处理
- 轻量级验证：只进行基本交易验证，不完整执行交易
- 批处理优化：将多个交易批量处理，提高效率
- 快速最终性：能够在较短时间内确认交易最终性
- 与执行层解耦：共识层只负责交易排序，不负责执行

优势：
- 更高吞吐量：通过延迟执行和批处理，大幅提高系统吞吐量
- 更低延迟：共识过程更轻量，减少了确认时间
- 更好的可扩展性：共识和执行分离，使系统更容易扩展
- 资源利用更高效：避免了共识过程中的重复计算
- 适应性更强：能更好地适应不同的网络和硬件条件

5. 为什么 Monad 采用“Optimistic Concurrency Control”？它在处理交易冲突方面是如何工作的？

Monad 采用"Optimistic Concurrency Control"主要是为了最大化并行执行的性能收益。

它在处理交易冲突方面的工作方式：

- 乐观假设：假定大多数交易之间不存在冲突，允许它们并行执行
- 读写集分析：预先分析交易可能访问的状态，预测潜在冲突
- 版本控制：为状态数据维护版本信息，用于检测并发修改
- 冲突检测：执行后检查是否有其他交易修改了相同的状态
- 冲突解决策略：
  - 当检测到冲突时，回滚冲突交易
  - 根据交易的全局序列号确定优先级
  - 按正确顺序重新执行冲突交易
  - 确保最终结果与顺序执行一致
