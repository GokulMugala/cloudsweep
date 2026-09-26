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

## Setup

### 1. Clone the repository

```bash
git clone https://github.com/GokulMugala/cloudsweep.git
cd cloudsweep
```

### 2. Create the environment file

Copy the example configuration:

```bash
cp .env.example .env
```

Fill in the required values for your environment. **Never commit `.env` or real credentials.**

### 3. Configure AWS

CloudSweep currently targets `us-east-1` and uses AWS access for EC2, EBS, and CloudWatch discovery.

Configure an AWS profile or credentials using the standard AWS CLI configuration:

```bash
aws configure
aws sts get-caller-identity
```

Use the least-privileged permissions possible. The repository includes [aws/readonly-policy.json](aws/readonly-policy.json) as a starting point for discovery and analysis permissions.

### 4. Configure the agent

Set the model/runtime values in `.env` as required by your local TrueForge/OpenAI setup.

Example:

```text
AWS_REGION=us-east-1
AWS_PROFILE=
OPENAI_API_KEY=
OPENAI_MODEL=
TRUEFOUNDRY_API_KEY=
TRUEFOUNDRY_ENDPOINT=
```

Do not paste real secrets into source files, documentation, screenshots, or commits.

### 5. Run the demo

Use the prompts in [demo/demo-prompts.md](demo/demo-prompts.md).

The intended demonstration is:

1. Discover EC2 and EBS resources.
2. Investigate potential waste.
3. Identify an orphaned EBS volume and explain the evidence.
4. Recommend a snapshot followed by deletion.
5. Stop and request explicit approval for the exact volume.
6. After approval, create the snapshot and delete the volume.
7. Verify that the volume is no longer available and report estimated savings.

> **Important:** Destructive AWS actions require explicit human approval. Do not approve a resource unless you have verified that it is safe to modify.

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
├── .env.example
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
