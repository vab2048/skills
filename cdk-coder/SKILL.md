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

## Stack Organization

- Use one stack per CDK app unless we explicitly request multiple stacks. This is our preference;
  AWS CDK supports multiple stacks.
- This is because references between stacks can silently introduce CloudFormation export/import coupling
  through CDK-generated outputs, even within the same app and without explicitly declared exports.
  Removing or changing those references can require staged deployments. Stack dependency ordering does
  not make updates across stacks atomic.
- Example: our bootstrap app had an infrastructure stack containing CodeBuild projects and an identities
  stack containing IAM policies referencing those projects. Correcting the policies to use project ARNs
  instead of project names caused CDK to remove the unused project-name exports. CloudFormation rejected
  the infrastructure update because the deployed identities stack still imported an export:

  > Cannot delete export
  > GiteaActionsExamplesBootstrapInfrastructure:ExportsOutputRefBuildProject097C5DB7169E8E09
  > as it is in use by GiteaActionsExamplesBootstrapIdentities.

  Resolving this across stacks would require retaining the exports, updating the consumer, then removing
  the exports in a subsequent deployment. A single stack would have handled the same reference change
  within one CloudFormation template, without cross-stack export/import coupling or staged deployments.
- Preserve existing stack boundaries during unrelated edits; consolidate existing stacks only when we
  explicitly request it.
- Organize resources into separate component classes within that stack. Have components create resources
  under the same parent stack scope and share resource references within one CloudFormation template,
  without separate stack lifecycles.

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
