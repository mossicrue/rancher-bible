# Layer 4 vs Layer 7

Load balancing in Kubernetes happens at Layer 4, with LoadBalancer services, or at Layer 7 with Ingresses.

When we say "Layer 4" and "Layer 7," we're referring to the OSI network model, but it doesn't exactly translate.

## Layer 4 load balancing
A Layer 4 load balancer receives traffic on a port and sends it to a backend on another port.   
It doesn't look at the traffic, and it doesn't care what that traffic is. It's considered to be a "dumb load balancer", not as an insult to its intelligence or value, but because it doesn't make decisions based on the traffic.  
Layer 4 load balancers are great for encrypted traffic and non-HTTP TCP traffic.

LoadBalancer Services are Layer 4 load balancers.

## Layer 7 load balancing
A Layer 7 load balancer receives traffic on a port, analyzes it, and directs it according to a set of rules in its configuration.  
These are most often used for HTTP traffic and route based on host, path, headers,
cookies, or other information in the data stream.  
These are considered to be "intelligent load balancers" because they make decisions about where to send the traffic.

Ingresses are served by the Ingress Controller, which is a Layer 7 load balancer.
