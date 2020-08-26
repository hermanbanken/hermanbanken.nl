---
title: Custom Envoy xDS server in Golang with Cluster Subsets
author: hbanken
type: post
date: 2020-08-24T20:00:00+00:00
url: /2020/08/24/custom-envoy-xds-golang-cluster-subsets/
categories:
  - cloudnative
  - kubernetes

---

At work (at [Q42, check it out](https://www.q42.nl/en)) we started using Envoy
for some use cases. Envoy is a very customizable proxy. Think of it as something
like NGINX, but with more dynamic configuration possibilities (NGINX Plus
probably also has these, but that is behind a paywall). In this post I'll
explore how you can dynamically configure it, via its control plane API called
"xDS".

<!--more-->

## Short Envoy intro

![Envoy Architecture](/images/2020/envoy.png)

A standard Envoy configuration consists of "listeners", each having "filter
chains" with one or more "filters". At least one often, because the "router" is
also a filter. This router sends the traffic to a backend "cluster", which is
just a list of IP addresses. There are several ways of determining this list of
IPs, one of which is resolving a hostname using DNS.

The picture above shows a simple example where two routes are defined, which
each point to a different cluster, both containing 2 backend servers. This
configuration is read from another server providing the xDS endpoint. An
comparable example of this xDS implementation can be found here:
[envoyproxy/go-control-plane internal/example](https://github.com/envoyproxy/go-control-plane/tree/master/internal/example).

Note that [Istio](https://github.com/istio/istio/) also uses Envoy as the
underlying proxy layer of the service mesh. It uses this same xDS API to
configure Envoy, which is injected beside each container as a sidecar. Given
Istio's endless features you can reason that this xDS API must be very flexible
and extensible to support this large feature-set.

## Cluster subsets

However simple aboves example, this post is about a more complex load balancing
scenario: one where we have subsets of backend servers inside a cluster. Each of
the cluster backends can serve us the desired service, but some of them might
serve a specific alpha/canary version, or some have some other specific desired
property.

![Envoy architecture diagram with v1 and v2 subsets](/images/2020/envoy-subset.png)

[The documentation about subsets](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/upstream/load_balancing/subsets)
is sparse. It does not show how to implement such a thing in xDS. I found it
less than trivial, so I hope that this post might help you if you want to
implement subsets. It boils down to two things:

1. determining request metadata; the easiest way to do this is with a HTTP
   Filter called Headers-To-Metadata, but you can also use a WebAssembly filter
   or something more sophisticated via a call-out to a gRPC service.
2. adding the metadata subset selectors to the cluster LbSubSetConfig
3. adding the metadata key-value pairs to the cluster endpoints

The following code snippet shows how to, using static YAML configuration:

<script src="https://gist.github.com/hermanbanken/f756aae18299f8674a7f498f8dfcef5f.js?file=file-static-alternative-yaml"></script>

It was interesting figuring out how to convert this into xDS code. First of,
there are several xDS endpoints that you can implement. If you just need a
dynamic list of cluster endpoints (this is called EDS), then you only need to
implement that gRPC endpoint. The Envoy xDS control plane lib offers a
`serverv3.Server` struct that implements all of the xDS endpoints. Instead of
implementing all methods yourself, you hand it a snapshot of the desired YAML
resources and it will cache and serve those for you.

Another bit of the puzzle is that you have to use `protobuf.Any` for wrapping
all non-core typed_config fields. For example, the Header-to-Metadata
configuration needs to be [marshalled into
Any](https://github.com/hermanbanken/envoy-xds/blob/master/xds/resource.go#L76).

The corresponding Go code can be found here:

1. setting request metadata (`X-Slice` header to `slice` metadata): [resource.go#L63-L71](https://github.com/hermanbanken/envoy-xds/blob/master/xds/resource.go#L63-L71)
2. adding subset selectors to the cluster:  [eds.go#76-L84](https://github.com/hermanbanken/envoy-xds/blob/master/xds/eds.go#L76-L84)
3. adding the key-value pairs to the cluster endpoints: [eds.go#L53-L58](https://github.com/hermanbanken/envoy-xds/blob/master/xds/eds.go#L53-L58]

The repository serves as a demo. It comes with a docker-compose.yaml so you can
quickly build & run it yourself, and tweak it further.

## Conclusion
Istio is a bridge too far for us now, due to the added complexity, but we can
use a part of it by using the underlying proxy technology, Envoy. It allows us
to define a very specific load balancing scheme, which - at work - has the
potential to replace several components and make the architecture a lot simpler:
less moving parts is both easier to reason about and also less error prone.

You can find the full source of this post at GitHub ([hermanbanken/envoy-xds](https://github.com/hermanbanken/envoy-xds)).
If you have any questions or remarks,
[find me on Twitter](https://twitter.com/hermanbanken)
or [create an issue at GitHub](https://github.com/hermanbanken/envoy-xds/issues).