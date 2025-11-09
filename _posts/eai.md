
# Embodied Agent Interface (EAI)

https://github.com/embodied-agent-interface/embodied-agent-interface

## 概括
EAI 是一个面向具身智能的标准化评测框架，提供统一的表示接口，用统一流程评估模型在“家居/行为环境”中的任务理解、动作生成与执行能力。
提供数据集资源、仿真环境、PDDL 文件、评测脚本与 CLI 接口，产出结构化指标与错误报告，支持可重复的对比实验。

## 评测维度

- 行动序列（ action_sequencing ）：生成可执行的动作脚本，评估目标匹配与执行成功。
- 目标理解（ goal_interpretation ）：将任务目标结构化（节点、关系、动作），计算精确率、召回率与 F1。
- 状态转移（ transition_modeling ）：模拟环境状态随动作的变化与可达性。
- 子目标分解（ subgoal_decomposition ）：基于 LTL 公式与图结构统计任务中节点/边/动作的总数与达成情况。

## 数据与资源

- 数据集： virtualhome 、 behavior
- 映射与目标：
  - id2task.json ：样本标识符到任务名称映射
  - id2action.json ：每个样本的“金标准动作集合”（目标动作）
  - success_task.json ：成功任务标识符集合（用于筛样本）
  - predicates_category.json ：谓词到类别映射（对象状态、关系等）
  - task_state_LTL_formula_accurate.json ：每个任务的节点/关系/动作目标统计与约束
- PDDL 资源：
  - 域文件与问题文件目录： virtualhome/resources/pddl_files 、 virtualhome/resources/problem_pddl/<task_name>/<file_id>.pddl

## 工作流

- 生成提示： eai-eval --mode generate_prompts （构造模型输入）
- 收集输出：读取模型响应（可用内置 helm_output 或自定义路径）
- 评估结果： eai-eval --mode evaluate_results （计算指标与错误信息）
- 产出文件：
  - summary.json ：目标与轨迹指标汇总
  - error_info.json ：各样本的可执行与错误类型详情


## 指标体系

- 目标评估（ goal_evaluation ）
  - task_success_rate ：满足所有目标的比例
  - state_goal ：状态目标匹配率
  - relation_goal ：关系目标匹配率
  - action_goal ：动作目标匹配率（对齐 id2action.json ）
  - total_goal ：综合目标匹配率
- 轨迹评估（ trajectory_evaluation ）
  - execution_success_rate ：动作序列可执行成功的比例
  - grammar_error ：
    - parsing ：解析错误比例（格式与评测器不一致）
    - hallucination ：名称或对象不在场景内（幻觉）
    - predicate_argument_number ：参数数目错误
  - runtime_error ：
    - wrong_order ：时序错误
    - missing_step ：缺少步骤
    - affordance_error ：能力/可用性错误
    - additional_step ：额外步骤（允许但计为错误类型之一）

## 脚本

```bash
PYTHONPATH=/home/y/embodied-agent-interface/src \
python -m eai_eval.cli \
  --mode evaluate_results \
  --eval-type action_sequencing \
  --dataset virtualhome \
  --llm-response-path /home/y/embodied-agent-interface/src/eai_eval/data/helm_output/helm_output \
  --output-dir /home/y/embodied-agent-interface/src/eai_eval/outputs/strict_ids_eval/virtualhome/evaluate_results/action_sequencing
```

## 结果分析

在经过一些处理（如统一格式、对齐动作目标等）后，得到最终指标，success rate 约为0.5。（若未处理，则全为0（parsing error））


## 用CoT/ICL提高模型性能

### CoT（Chain of Thought）

- 约束感知的推理：在“推理段”明确三件事并据此规划动作序列。
  - 场景绑定：只使用白名单 object_name→id ，拒绝场景外对象。
  - 目标覆盖：逐项核对 id2action[file_id] 的必需动作集合，保证至少一次命中。
  - 前置与时序：为每个必需动作列出可达性前置（例如“必须先 PLUGIN/ SWITCHON 再 USE ”），据此排序。
- 自检清单（推理段内执行，不进入输出）：必需动作是否覆盖、对象是否全在白名单、参数数目与格式是否正确、是否存在多余/错误时序的动作。
- 输出分离：推理段用来“想清楚”，输出段仅给动作脚本行字符串列表，保证评测器可解析。

