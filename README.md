# CloudSweep

**Autonomous AWS Cloud Cost Cleanup Agent**

CloudSweep is an agentic AWS cost-optimization prototype built for the **Agents That Act** hackathon. It investigates AWS resources, combines multiple signals, explains potential waste, estimates savings, and pauses for explicit human approval before destructive actions.

## Core workflow

**Discover → Investigate → Reason → Recommend → Approve → Act → Verify**

## What it demonstrates

- EC2 and EBS discovery in AWS
- CloudWatch utilization analysis
- Multi-signal waste detection
- Estimated monthly and annual savings
- Risk and confidence reporting
- Human approval before destructive actions
- Verification after approved changes

## Example

CloudSweep can identify an unattached EBS volume as a potential cost-waste candidate without automatically assuming it is safe to delete.

The agent should distinguish:

**unattached → investigate data need → approval → backup/snapshot when appropriate → delete → verify**

## Architecture

- **TrueForge** — agent runtime/harness
- **OpenAI** — reasoning model
- **AWS MCP** — AWS tool access
- **AWS** — EC2, EBS and CloudWatch

See [architecture/architecture.md](architecture/architecture.md).

## Repository structure

```text
cloudsweep/
├── README.md
├── .gitignore
├── LICENSE
├── agent/
│   └── instructions.md
├── aws/
│   └── readonly-policy.json
├── demo/
│   └── demo-prompts.md
└── architecture/
    └── architecture.md
```

## Scope

Current prototype scope:

- Region: `us-east-1`
- EC2
- EBS
- CloudWatch

## Safety

Discovery and analysis are read-only by default. CloudSweep must not terminate an instance, delete or detach storage, or perform another destructive action without explicit human approval for the exact action and resource.

Never commit AWS credentials, access keys, secret keys, bearer tokens, or other secrets to this repository.

## Demo

See [demo/demo-prompts.md](demo/demo-prompts.md) for the demo sequence.

## Hackathon

Built for **Agents That Act — TrueFoundry × Polaris School of Technology**.

This project focuses on a practical question:

> How can an AI agent investigate real cloud infrastructure and safely move from recommendation to action without removing human control?
