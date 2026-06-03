# Skills

My custom skill definitions for usage by an LLM.

# Brief

- From Naufaldi Rafif (Cursor devrel) as best practices:
  - Skills are treated as reusable infrastructure, not project configuration.
  - (Trusted) skills should be global by default. 
  - Only define them at the project level when they encode business rules or domain-specific flows.

# Setup 

We expect an LLM independent structure for hosting our global skill definitions.

For example, a structure like so:

```
C:\Users\v
  - .agents      (LLM independent root)
    - skills     (this repo)
      - skill1
      - skill2
      - ...
```

## Codex

Set `CODEX_HOME` to point to the LLM independent root:
- `export CODEX_HOME="C:\Users\v\.agents"`
- This `skills` folder will sit under the `.agents` one.

With the env var set, you can ask codex what skills it recognizes and the ones in this repo will appear. 

# Links

- https://cursor.com/docs/skills
- https://javaevolved.github.io/
   - Very useful for the java-coder skill. 
- https://blog.frankel.ch/writing-agent-skill/
   - Good tips on writing an agent skill. 

