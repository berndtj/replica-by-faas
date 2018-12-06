# Use cases for Harbor+FaaS

## Function Image Repository

This is the core Harbor+FaaS use-case.  Function artifacts are packaged and deployed as images.  Solutions are to generally use hosted image repositories or internal insecure repositories.  Harbor can provide an open-source production ready image repository for any FaaS.

## Harbor + Knative Build Template

Leverage the vulnerability scanning of Harbor to ensure built images are "clean".

## Use Harbor Events + Dispatch/FaaS for Automation

Use Harbor events to trigger maintenance type operations such as periodic back-up, alerting, etc.
