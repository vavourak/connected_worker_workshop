---
title : "Workflows"
weight : 30
---
## Workflows

There are 3 phases of workflows:  Set up sources and destinations, message routing, and finally visualization.  Each phase should be completed before starting the next, otherwise it might not be possible to complete them.  

Within each phase there are multiple workflows, which can be completed in parallel.  So let's work as a team and build together.  Some workflows can even be worked on by multiple teammates at the same time, so nobody should have an excuse to not participate!

### Phase 1: Data Sources & Destinations
- Setup the base station by remotely deploying code to it from the AWS Console using IoT Greengrass.
- Create device shadows for both watches and the base station, so that we can adjust the alarm thresholds for each.
- Build the detector model with IoT Events that will generate the alarm notifications for the connected workers.
- Build data twins of our devices using models in IoT SiteWise that also acts as a data store.

### Phase 2: Message Routing
- Link the incoming device telemetry to the consuming services, IoT SiteWise and IoT Events.

### Phase 3: Visualize
- Visualize real-time data easily with IoT SiteWise Monitor dashboards.
- Visualize near-real-time data with Managed Grafana.
