# 2. Deploy Independently

Date: 2018-08-13

## Status

Proposed

## Context

In a microservice oriented system, being able to deploy independently allows for individual services to produce features and fix issues at an accelerated pace, due to a reduced overhead in release coordination. This also helps to provide a bounded context of a given service, and reduce coupling between others. All of these are key components of a continuous delivery system; anything that would require more advanced coordination can make even automation error-prone. Independent services are advantageous when moving to container systems such as Docker, as they are designed to be pared down to the minimal viable pieces needed to run whatever the one thing the container is designed to do, rather than packing multiple functions into the same machine.

## Decision

* One service per host; this host could be bare metal, a VM, or a container.
* Service APIs should be robust:
    * Use [tolerant readers](https://martinfowler.com/bliki/TolerantReader.html); only read elements you need from an API, and ignore the rest.
    * When used, versioned APIs should co-exist to allow for deprication over time by other services.
* Services should be architectured for failure:
    * Services should be tolerant of network/dependency failures, and retry connections with exponential backoff until a given cutoff time.
    * Favour graceful service degradation over entire system outage.

## Consequences

The overheads of managing a deployment are greatly reduced, thus allowing more frequent deployments, and faster feature development at a different cadence to other teams. Communication between teams through documentation of API endpoints will need to be managed correctly, which could cause additional complexity.
