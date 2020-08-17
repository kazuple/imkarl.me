---
title: Response Groups
description: 
published: true
date: 2020-07-22T10:25:56.846Z
tags: 
editor: undefined
---

# Group
## Participation Policy
- Informal: User is automatically signed into queue.
- Formal: User is required to sign into the queue manually.

## Routing Methods
### Attendant
- Calls delivered to all agents irrespective of their presence. 
- Agents will keep getting pop ups for new incoming calls and it will also beep for new calls while agent is on the call.

### Parallel
- Calls delivered to all agents with presence set to Available. 
- Agent will never receive a call if the presence is other than ‘Available’.

### Longest idle
- Offer a new call first to the agent who has been idle the longest (has had a presence of Available or Inactive the longest). 
- If this agent does not answer call goes to next agent and so forth until all agents are tried.

### Round robin
- Calls are offered to agents in round robin fashion. 
- Call will be offered to each agent in turn and move to next agent if call is not answered.
- Presence needs to be Available to accept the call.

### Serial
- Offers calls to agents in the order they are listed.
- Call routes to first agent in the list, then the other agents are called in sequence.
- If available, top agent will always get the call first.