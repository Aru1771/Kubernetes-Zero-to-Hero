In our Kubernetes environment, we use Gateway API for north-south traffic entering the Kubernetes cluster.

First, we install the Gateway API CRDs so that Kubernetes understands resources such as GatewayClass, Gateway, and HTTPRoute.

Then we install a Gateway API controller, such as Istio. The controller watches the Gateway API resources and implements the required gateway or load-balancing infrastructure.

Gateway API mainly has three resources:

**1. GatewayClass**

GatewayClass defines which controller is responsible for managing the Gateways. For example, the `controllerName` identifies the controller implementation.

**2. Gateway**

Gateway represents the entry point for external traffic into the cluster. We configure listeners on the Gateway with the required protocol, port, hostname, and TLS configuration. The Gateway references the required GatewayClass using `gatewayClassName`.

For example, we can configure HTTP on port 80 or HTTPS on port 443. For HTTPS, we can configure TLS termination or TLS passthrough depending on the requirement.

**3. HTTPRoute**

HTTPRoute defines where the incoming HTTP traffic should go. We can use path-based, host-based, or header-based routing.

`parentRefs` connects the HTTPRoute to the Gateway, and `backendRefs` specifies the Kubernetes Service that should receive the traffic.

For example:

`/users` → user-service

`/orders` → order-service

The Service then forwards the traffic to the appropriate Pods.

For TLS termination, the Gateway uses a TLS certificate and private key stored in a Kubernetes TLS Secret. The Gateway terminates the TLS connection and forwards the request to the backend.

If TLS passthrough is required, the Gateway does not terminate the TLS connection. The encrypted traffic is forwarded to the backend, where TLS is handled.

So the overall traffic flow is:

Client → Gateway → Route → Service → Pod.
