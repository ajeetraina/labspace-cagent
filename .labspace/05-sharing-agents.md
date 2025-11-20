## Step 5: Sharing Agents with Docker Registry

By now you should have a couple of agents, and maybe one that already does something useful. Wouldn't it be nice if we could share agents with others?

Sharing agents with cagent is extremely simple - you can push and pull agents to any OCI registry.

### Setting Up Your Docker Hub ID

Enter your Docker Hub username:

::variableDefinition[dockerhubid]{prompt="Enter your Docker Hub username"}

**Run this command** to set your Docker Hub ID as an environment variable:
```
export DOCKER_HUB_ID="{{dockerhubid}}"
```

Verify it's set:
```
echo $DOCKER_HUB_ID
```

Make sure you're logged in to Docker Hub:
```
docker login
```

### Push Your Agent
```
cagent push developer.yaml $DOCKER_HUB_ID/cagent-developer
```

### Pull an Agent

You can pull that agent on any machine:
```bash
cagent pull $DOCKER_HUB_ID/cagent-developer
```

### Run Directly from Registry

Or run it directly without pulling first:
```bash
cagent run $DOCKER_HUB_ID/cagent-developer
```

### Exercise

1. Push your developer agent to your Docker Hub account
2. Ask your neighbor for their Docker Hub ID
3. Pull and run their agent
4. Try some of the pre-built agents from the community:
```bash
   cagent run creek/pirate
```

## Next Steps

In Step 6, we'll explore multi-agent systems and sub-agents.
