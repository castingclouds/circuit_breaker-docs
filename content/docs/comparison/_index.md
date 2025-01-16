---
weight: 5
bookFlatSection: true
---
# DAGs vs. Petri Nets

## Directed Acyclic Graphs (DAGs)

DAGs are commonly used in workflow systems like Argo Workflows and Apache Airflow. They offer:

**Advantages:**
- Simple, intuitive representation of task dependencies
- Easy to visualize and understand
- Well-suited for linear, one-way processes
- Efficient execution of parallel tasks

**Limitations:**
- Cannot represent cycles or loops natively
- Limited ability to model complex state conditions
- No built-in concept of state or tokens
- Difficult to model complex business processes

## Petri Nets

Circuit Breaker uses Petri Nets (specifically, a variant closer to Colored Petri Nets) as its underlying model. This provides:

**Advantages:**
- Rich theoretical foundation with formal verification
- Native support for state and tokens
- Ability to model complex conditions and cycles
- Better representation of business processes
- Support for concurrent and distributed systems

**Limitations:**
- More complex to understand initially
- Can be more verbose for simple linear workflows
- Requires more careful design for optimal performance

## Comparison with Argo Workflows

**Focus:** Container-native workflow engine for Kubernetes

**Key Differences:**
- Argo uses DAGs, while Circuit Breaker uses Petri Nets
- Argo is container-focused, Circuit Breaker is Ruby-native
- Argo excels at container orchestration, Circuit Breaker at business logic
- Circuit Breaker provides built-in AI integration

**Use Cases:**
- Argo: Container orchestration, CI/CD pipelines, data processing
- Circuit Breaker: Agentic AI workflows, Stateful business processes, DevOps pipelines

## When to Use Circuit Breaker

Circuit Breaker is particularly well-suited for:

1. **Complex Business Processes**
   - Multiple states and transitions
   - Complex validation rules
   - State-dependent behavior

2. **AI-Powered Workflows**
   - LLM integration
   - Autonomous agents

3. **State-Heavy Applications**
   - Rich state management
   - Complex transition rules
   - Audit requirements

4. **Hybrid Workflows**
   - Combining automated and manual steps
   - Person in the loop pattern
   - Multiple participant roles
   - Complex approval flows

Circuit Breaker can also be used effectively for CI/CD and infrastructure management. See [CI/CD and Infrastructure]({{< ref "/docs/cicd" >}}) for a detailed guide on using Circuit Breaker in DevOps workflows.
