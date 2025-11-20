## Step 5: Sharing Agents with Docker Registry

By now, you should have a couple of agents, and maybe one that already does something useful. Wouldn't it be nice if we could share agents with others?

Sharing agents with cagent is extremely simple - you can push and pull agents to any OCI registry.

### Setting Up Your Docker Hub ID

Enter your Docker Hub username:

::variableDefinition[dockerhubid]{prompt="Enter your Docker Hub username"}

Set your Docker Hub ID as an environment variable:
```bash
export DOCKER_HUB_ID=$$dockerhubid$$
```

Verify it's set correctly:
```bash
echo $DOCKER_HUB_ID
```

Make sure you're logged in to Docker Hub:
```bash
docker login
```

### Push Your Agent
```bash
cagent push developer.yaml $DOCKER_HUB_ID/cagent-developer
```

### Pull an Agent

You can pull that agent on any machine:
```bash
cagent pull $DOCKER_HUB_ID/cagent-developer
```

### Run Directly from Registry
```bash
cagent run $DOCKER_HUB_ID/cagent-developer
```


## Next Steps

In Step 6, we'll explore multi-agent systems and sub-agents.
