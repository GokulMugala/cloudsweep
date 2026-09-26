# CloudSweep Demo Prompts

## 1. Discover

```text
Discover my AWS EC2 instances and EBS volumes in us-east-1.
Read-only only. Do not modify anything.
```

## 2. Analyze

```text
Analyze the EC2 instances and EBS volumes you discovered.

Identify potentially wasteful resources using multiple signals.
Estimate their monthly cost and potential monthly and annual savings.

Do not modify or delete anything.

Explain your evidence, risk, confidence, and recommended action for each resource.
```

## 3. Approval-gated cleanup

Use a deliberately created test resource, never a production resource.

```text
Identify whether any discovered resource is a candidate for cleanup.

Do not modify anything yet.

For any proposed destructive action, state:
- exact resource ID
- evidence
- risk
- confidence
- expected savings
- exact action that would be performed

Wait for my explicit approval naming the exact resource and action.
```

## 4. After explicit approval

Example:

```text
I approve [EXACT ACTION] for [EXACT RESOURCE ID].
```

The agent should re-check the resource, perform only the approved action, verify the AWS result, and report the outcome.

## Safety note

An unattached EBS volume is a potential waste candidate, not automatic proof that deletion is safe. Data retention and ownership should be verified before destructive cleanup.
