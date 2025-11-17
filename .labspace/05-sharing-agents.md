# Step 5: Sharing Agents with Docker Registry

By now you should have a couple of uselesss agents, and maybe one that already
does something useful. Wouldn't it be nice if we could share agents with others?

Sharing agents with cagent should be extermely simple, you can push and pull
agents to any OCI registry.

Exercice: push your developer agent, ask your neighour to pull and run it. Play around

Here's how to push an agent:

```console
cagent push developer.yaml your-account/cagent-developer
```

You can then pull that agent:

```console
cagent pull your-account/cagent-developer
```

Or you might want to directly run it:

```console
cagent run your-account/cagent-developer
```

TODO: @rumpl give some example
