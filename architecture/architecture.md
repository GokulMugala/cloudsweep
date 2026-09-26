# CloudSweep Architecture

## Overview

CloudSweep is an agentic workflow for AWS cost investigation and approval-gated cleanup.

```text
User
  |
  v
TrueForge Agent Runtime
  |
  +--> OpenAI (reasoning)
  |
  +--> AWS MCP
          |
          v
        AWS APIs
          |
          +--> EC2
          +--> EBS
          +--> CloudWatch
```

## Agent loop

```text
Discover
   ↓
Investigate
   ↓
Reason
   ↓
Recommend
   ↓
Human Approval
   ↓
Act
   ↓
Verify
```

## Components

### TrueForge
Agent runtime/harness used to define the CloudSweep agent, instructions, tools and approval flow.

### OpenAI
Reasoning layer used to interpret resource information, compare signals and formulate recommendations.

### AWS MCP
Tool interface used by the agent to access AWS operations.

### AWS
The target cloud environment. The current prototype focuses on EC2, EBS and CloudWatch in us-east-1.

## Safety model

Read-only discovery and analysis are the default.

Destructive actions require:
1. Exact resource identification
2. Evidence and expected impact
3. Risk and confidence
4. Explicit human approval
5. Re-check before execution
6. Post-action AWS verification

## Current limitations

The prototype is focused on EC2, EBS and CloudWatch in one AWS Region. Broader service coverage, multi-account support, richer billing/pricing data, policy-as-code, rollback strategies and multi-cloud support are future improvements.
