# Requests, turns, tools and the agent loop

| Page status | Intended audience | Last reviewed |
| --- | --- | --- |
| Draft for internal review | Existing Copilot users new to agentic workflows | 8 August 2026 |

One message to Copilot can cause many internal steps. This explains why agent tasks can do much more than chat responses—and why they can consume more time, context and credits.

## One user request is not necessarily one model call

Consider this request:

```text
Find the cause of the failing checkout tests, fix it and verify the result.
```

An agent might perform:

| Step | Model decision | Result added to context |
| --- | --- | --- |
| 1 | Search for checkout tests | Matching file paths |
| 2 | Read the likely tests and implementation | File contents |
| 3 | Run the failing test | Test output |
| 4 | Edit the implementation | Applied change |
| 5 | Run the test again | Passing or failing output |
| 6 | Produce a final response | Summary and evidence |

The person submitted one request, but the agent may have called the model at each step.

## What is a turn?

The term is overloaded in AI discussions. In GitHub's Copilot SDK agent-loop documentation, a **turn** is one language-model API call and its consequences:

1. Copilot sends the accumulated context to the model.
2. The model responds, possibly requesting tools.
3. Copilot runs any requested tools.
4. The turn ends.

If a tool was used, its result becomes context for the next turn. One user request therefore commonly contains multiple turns.

When discussing usage, say whether “turn” means an internal model call or a user-visible message. This wiki uses **user request** for the latter.

## What is a round?

“Round” is not used here as an official Copilot accounting term. It is sometimes used informally for an exchange or iteration, which creates ambiguity.

In the [To Test](To-Test.md) page, one **test round** means a complete repeat of the same scenario. Elsewhere, prefer “user request,” “model turn,” “tool call” or “agent-loop iteration.”

## What is a tool?

A tool lets the agent interact with an environment. Common examples include:

- Searching for files or text.
- Reading and editing files.
- Running a terminal command.
- Inspecting source-control changes.
- Fetching current web documentation.
- Querying GitHub, a database or another service through MCP.

The model chooses a tool from its name, description and input definition. Copilot executes it and returns the output.

## Why tools affect quality and cost

Every available tool adds to the set of options the model must consider. Tool descriptions can occupy context, and tool results become input for later turns.

Too many irrelevant tools can:

- Make tool selection less focused.
- Add unnecessary context.
- Cause avoidable calls and larger results.
- Increase token and AI-credit consumption.
- Expand the security and permission surface.

Give an agent the tools it needs, not every tool that exists.

## The agent loop

The agent normally cycles through three broad stages:

1. **Understand:** inspect code, errors and documentation.
2. **Act:** edit files, run commands or call external systems.
3. **Validate:** run tests, inspect changes and correct failures.

It repeats until the model produces a final response or the session is stopped.

```text
Understand -> Act -> Validate
    ^                    |
    +---- correct -------+
```

An agent that edited code but did not validate it has usually stopped too early. An agent that repeats the same failing action without changing its understanding is stuck and should be redirected.

## Tool approval and permissions

Some tools only read information; others can change files, execute commands or update external systems. Copilot clients provide permission and approval controls, but the exact UI varies.

Use least privilege:

- A reviewer normally needs read and search tools, not edit access.
- A documentation task probably does not need production-deployment tools.
- High-impact external actions should require explicit approval.
- Instructions are not a security boundary; enforce important controls through permissions, hooks and platform policy.

## How this relates to cost

Under GitHub's current usage-based billing, cost depends on the model and tokens consumed. More agent-loop turns commonly mean that the accumulated context is processed more times and more output is generated.

This does not produce a fixed “cost per tool call.” A tool call itself, its definition, its output, the model used, caching and subsequent turns all affect the result.

## How to make the loop efficient

- Provide a clear outcome and bounded scope.
- State how success should be verified.
- Point to known files, errors or commands.
- Use read-only planning before expensive implementation when requirements are unclear.
- Restrict tools for specialised agents.
- Summarise large logs before reading all detail.
- Stop and redirect repeated failing approaches.
- Ask the final response to include validation evidence.

## Sources

- [The Copilot SDK agent loop and turns](https://docs.github.com/en/copilot/how-tos/copilot-sdk/features/agent-loop)
- [Agents and the agent loop in VS Code](https://code.visualstudio.com/docs/agents/concepts/agents)
- [Tools in VS Code](https://code.visualstudio.com/docs/agents/concepts/tools)
- [Usage-based billing for organizations and enterprises](https://docs.github.com/en/copilot/concepts/billing/usage-based-billing-for-organizations-and-enterprises)
- [Trust and safety for AI in VS Code](https://code.visualstudio.com/docs/agents/concepts/trust-and-safety)
