---
name: cdk-coder
description: AWS CDK infrastructure authoring, refactoring, debugging, and deployment support for TypeScript, Python, Java, and C# CDK apps. Use when working in CDK projects, editing stacks or constructs, diagnosing synth/deploy failures, updating tests, or reviewing generated cloud infrastructure code.
---

# CDK Coder

## Overview

Use this skill to make targeted changes to AWS CDK applications without breaking the existing app shape. Preserve the repo's language, construct style, naming, environment configuration, and test approach.

## Workflow

1. Inspect the CDK entrypoint and project metadata first.
2. Read `cdk.json`, the build/test config, stack files, and any existing constructs or helpers.
3. Match the implementation to the repo's current language and framework instead of introducing a new pattern.
4. Make the smallest change that satisfies the request.
5. Verify with the repo's own synth and test commands when available.

## Edit Rules

- Prefer repo-defined scripts over invented commands.
- Preserve logical IDs, construct names, and stack boundaries unless the change explicitly requires a rename.
- Keep environment/account/region wiring consistent with the existing app.
- When adding resources, wire dependencies explicitly and avoid hidden side effects.
- Treat context lookups, feature flags, and environment variables as part of the app contract.
- Update snapshots or assertions only when the infrastructure behaviour genuinely changed.

## Common Tasks

### Update tests

- Prefer the existing test framework and patterns in the repo.
- Add or adjust tests around the changed stack behavior, not just the implementation details.
- Use the smallest assertion surface that still proves the change.

## Prompting the user

- Always ask about retention policies on resources which appear in the synthesized output. 
   - Should the resource exist beyond the lifecycle of the stack if the stack is destroyed? e.g. a log group.
- Always make it clear when a resource being created will not actually be managed by the stack at all. 
   - For example when using `Code.fromAsset()` for a lambda - the CDK bootstrap s3 bucket will host that JAR - not the 
     lambda CDK stack.
- Always make it clear if there are any resources (e.g. like the `Code.fromAsset()`) JAR) which will not have the 
  tags applied.