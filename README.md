# replica-by-faas

Use FaaS platform to drive the image replication in Harbor registry.

## Background

Harbor is an open source trusted cloud native registry which stores, manages and distribute the container related content like images and helm charts.

Harbor provides a fantastic policy based replication mechanism to sync the content (images/charts) from the source registry to the target registry.

The main replication process can be described by the following stages:

* Do related pulling preparation work before pulling content from the source registry, e.g: check permissions
* Pull the content from the source registry into buffer with any applied filters
* Do any necessary post actions after content pulling
* Do related pushing preparation work before pushing content to the target registry, e.g: create necessary namespace
* Push the content data to the target registry
* Do any necessary post actions after content pushing

Each image repository will be replicated through one background async task. If there are large scale image repositories, there are lots of async tasks.

## Problems

There might be two obvious issues in current native implementation:

* The replication async is driven by the job service which is a async task queue. There might be obvious throughput/performance bottle-neck when there are large replication tasks requests.
* The replication may happen between different registry implementations like harbor and GCR/ECR etc. To support such different registries, different adapters need to be implemented to cover the different registries. New registry source/target added should need a new recompiled binary. Obviously, lacking of flexibilities causes high cost to enable new registry kind supporting.

## Solution

* Each stage of the replication process can be implemented as a FUNCTION piece
* The whole replication process can be defined as a coordinator template which can be composed by FUNCTION pieces
* Enable a new registry kind supporting just need to create FUNCTION pieces and a template
* The replication process template with FUNCTIONS can be managed through the web console (add FUNCTIONS, orchestrate new replication process template)
* The result of the replication can be organized by the results of stages in the process template and filled with the order defined in the process template

## Misc

None.