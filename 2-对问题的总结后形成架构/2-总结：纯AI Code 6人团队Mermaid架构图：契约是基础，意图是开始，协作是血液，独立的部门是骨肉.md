纯AI Code 6人团队架构图（概要版）

**契约是基础（地基），意图是开始（发令枪），协作血液（事件与调度），独立部门是骨肉（执行体）**。

```mermaid
graph TD
    subgraph IntentLayer [意图层: 开始]
        H_O[全局统筹者]:::human
        H_D[部门代表群]:::human
    end
    subgraph OrchestrationLayer [统筹调度层: 血液]
        O_AI[统筹AI]:::core
        O_EventBus[事件总线]:::core
        O_Gateway[API网关]:::core
        O_Monitor[对账自愈中心]:::core
    end
    subgraph ContractLayer [契约层: 基础]
        C_Registry[契约注册中心]:::contract
        C_CDCT[CDCT测试池]:::contract
    end
    subgraph DeptLayer [部门执行层: 骨肉]
        D_AI_A[部门AI A]:::dept
        D_DB_A[(私有DB A)]:::dept
        D_AI_B[部门AI B]:::dept
        D_DB_B[(私有DB B)]:::dept
    end
    subgraph InfraLayer [公共基础设施层: 底座]
        P_AI[公共AI: CI/CD/安全/组件/部署]:::infra
    end
    H_O --> O_AI; H_D --> O_AI
    O_AI --> C_Registry; O_AI --> D_AI_A; O_AI --> D_AI_B
    C_Registry --> D_AI_A; C_Registry --> D_AI_B; C_Registry --> O_Gateway
    D_AI_A --> D_DB_A; D_AI_B --> D_DB_B
    D_AI_A --> O_Gateway --> D_AI_B
    D_AI_B --> O_EventBus --> D_AI_A
    O_Monitor --> O_EventBus; O_Monitor --> H_O
    P_AI -.-> D_AI_A; P_AI -.-> D_AI_B; P_AI -.-> O_Gateway; P_AI -.-> O_EventBus
    O_AI --> P_AI
```

### 五层架构解读
1. **意图层（开始）**：人类只输入结构化意图，沙盘推演发现死锁反向推给人类裁决。**人类立法，AI释法**。
2. **契约层（基础）**：系统核心护城河。契约进注册中心接受审查（只增不减），锁定后下发强制生成代码骨架，CDCT卡死后续变更。
3. **部门层（骨肉）**：5个自治孤岛，独立AI+私有DB，只看契约不关心其他部门实现。
4. **协作血液**：同步走API网关（海关拦截脏数据），异步走事件总线（状态变更秒传），对账中心定时比对指纹自动自愈。
5. **公共底座**：公共AI统一管理CI/CD、安全扫描、公共组件、部署环境。部门AI只写业务代码。

**目标**：零会议、零合并冲突、零脏数据污染。
