---
title: One certificate per connection
date: Not yet
draft: true
---

There's an article to write about this.

There are some situations where it makes sense to have one certificate per
connection, if the services you depend on have different requirements of the
certificates, or need to whitelist the certificates.

And it would be nice with some automation to handle the added complexity.

Here's some communication I had with Mistral about this:

Me:

I'm creating a solution that involves several services. Only one of these
services is available on the public internet. The service to service
communication still needs to be https though. What are some benefits and
drawbacks to using a single certificate per server, vs a certificate per
connection?

Mistral:

Great question, Jakob! The choice between using a **single certificate per
server** (wildcard or multi-domain) and a **certificate per connection** (one
certificate per service or endpoint) is important for security, manageability,
and performance. Here’s a breakdown of the benefits and drawbacks for each
approach:

---

## **Single Certificate per Server**

### **Benefits**

- **Simplified Management**: Fewer certificates to track, renew, and deploy.
  This reduces operational overhead, especially in large environments.
- **Cost-Effective**: Wildcard or multi-domain certificates (e.g.,
  `*.example.com`) are often cheaper than purchasing individual certificates for
  each service.
- **Easier Deployment**: Only one certificate needs to be installed and
  configured per server, reducing the risk of misconfiguration.
- **Scalability**: Adding new services or subdomains under the same certificate
  is straightforward, as long as the wildcard or SAN (Subject Alternative Name)
  covers them.

### **Drawbacks**

- **Security Risk**: If a single certificate is compromised, all services using
  that certificate are at risk. This increases the blast radius of a security
  breach.
- **Less Granular Control**: You can’t revoke or update a certificate for a
  single service without affecting others.
- **Potential Compatibility Issues**: Some legacy systems or strict security
  policies may not support wildcard certificates or SANs.
- **Complexity in Rotation**: Rotating a single certificate requires
  coordinating downtime or seamless updates across all services using it.

---

## **Certificate per Connection (One per Service)**

### **Benefits**

- **Enhanced Security**: Each service has its own certificate, limiting the
  impact of a compromise to just that service. This aligns with the principle of
  least privilege.
- **Granular Control**: You can revoke, update, or rotate certificates
  independently for each service, reducing downtime and risk.
- **Better Compliance**: Some regulatory frameworks (e.g., PCI DSS, HIPAA) may
  require or recommend separate certificates for sensitive services.
- **Flexibility**: Different services can use certificates with varying
  lifetimes, algorithms, or security levels based on their specific needs.

### **Drawbacks**

- **Increased Management Overhead**: More certificates mean more to track,
  renew, and deploy. This can become cumbersome without automation (e.g., using
  tools like Let’s Encrypt, HashiCorp Vault, or Cert-Manager).
- **Higher Cost**: Purchasing individual certificates for each service can be
  more expensive than using a wildcard or multi-domain certificate.
- **Complexity in Deployment**: Each service must be configured with its own
  certificate, increasing the risk of misconfiguration.
- **Scalability Challenges**: Adding new services requires generating and
  deploying new certificates, which can slow down scaling efforts.

---

## **Key Considerations for Your Use Case**

1. **Security Sensitivity**: If your services handle highly sensitive data,
   individual certificates are preferable for isolation.
2. **Automation**: If you can automate certificate management (e.g., using ACME
   protocols like Let’s Encrypt), the overhead of per-connection certificates is
   significantly reduced.
3. **Compliance Requirements**: Check if your industry or organization mandates
   specific certificate practices.
4. **Performance Impact**: While HTTPS overhead is generally minimal, having
   many certificates may slightly increase TLS handshake times if not managed
   properly (e.g., using session resumption or OCSP stapling).

---

## **Hybrid Approach**

You might also consider a hybrid approach:

- Use a **wildcard certificate** for non-sensitive internal services.
- Use **individual certificates** for public-facing or highly sensitive
  services.

Would you like recommendations on tools or best practices for managing
certificates in your specific environment?

Me:

> I think you misunderstand. The services are not copies, and they communicate
> via IP-addresses, so none of the certificates will be wildcard certificates.
> The 'per service' strategy would be like nodes in a graph each having a
> certificate, whereas the 'per connection' would instead be that each
> (direction of) edge in the graph has its own certificate. Drawbacks and
> benefits of that?

Mistral:

> Ah, thanks for clarifying, Jakob! You’re describing a **per-service
> certificate** (each node in your service graph has its own certificate) versus
> a **per-connection certificate** (each directed edge or connection between
> services has its own certificate). This is a more nuanced and interesting
> scenario, especially for internal service-to-service communication over HTTPS.

Let’s break down the benefits and drawbacks of each approach in this context:

---

## **Per-Service Certificate (Each Node Has Its Own Certificate)**

### **Benefits**

- **Simpler Certificate Management**: Each service only needs to manage its own
  certificate (and private key). This reduces the number of certificates in play
  and simplifies deployment and renewal.
- **Clear Ownership**: Each service team can own and manage its own certificate,
  making it easier to assign responsibility and audit.
- **Easier Revocation**: If a service is compromised, you only need to revoke
  and replace one certificate.
- **Scalability**: Adding a new service only requires generating and deploying
  one new certificate for that service.

### **Drawbacks**

- **Less Granular Security**: If a service’s certificate is compromised, an
  attacker could impersonate that service to all other services it communicates
  with. The blast radius is all connections involving that service.
- **No Connection-Specific Policies**: You can’t enforce different security
  policies (e.g., cipher suites, certificate lifetimes) for specific connections
  between services.
- **Potential for Misconfiguration**: If a service’s certificate is
  misconfigured, it could break all its incoming and outgoing connections.

---

## **Per-Connection Certificate (Each Edge Has Its Own Certificate)**

### **Benefits**

- **Fine-Grained Security**: Each connection can have its own certificate,
  allowing for connection-specific security policies (e.g., stronger encryption
  for sensitive connections). If one connection’s certificate is compromised,
  only that specific connection is at risk.
- **Isolation**: Compromise of one connection’s certificate doesn’t affect other
  connections, even for the same service.
- **Flexibility**: You can tailor certificates to the specific needs of each
  connection (e.g., shorter lifetimes for high-risk connections).
- **Auditability**: Easier to track and audit which services are communicating
  and under what security parameters.

### **Drawbacks**

- **Complex Management**: The number of certificates scales with the number of
  connections, not just services. This can become unwieldy without robust
  automation (e.g., service mesh, certificate managers).
- **Operational Overhead**: Deploying, renewing, and revoking certificates for
  every connection is more complex and error-prone.
- **Performance Impact**: More certificates mean more TLS handshakes and
  potentially more overhead for certificate validation, though this is usually
  minimal in practice.
- **Tooling Requirements**: You’ll likely need advanced tooling (e.g., service
  mesh like Istio or Linkerd, or a certificate management platform) to handle
  the complexity.

---

## **Key Considerations for Your Architecture**

1. **Security Requirements**: If your services handle highly sensitive data or
   require strict isolation, per-connection certificates may be worth the
   overhead.
2. **Automation**: Without automation, per-connection certificates will be
   impractical. Tools like **service meshes** (Istio, Linkerd), **certificate
   managers** (Cert-Manager, Vault), or **ACME** (Let’s Encrypt) can help.
3. **Performance**: The overhead of per-connection certificates is usually
   negligible, but test in your environment to confirm.
4. **Audit and Compliance**: Per-connection certificates provide finer-grained
   audit trails, which may be useful for compliance.

---

## **Practical Recommendation**

- If your environment is **dynamic** (services frequently added/removed) and you
  lack automation, **per-service certificates** are more practical.
- If you have **high security requirements** and can automate certificate
  management, **per-connection certificates** offer better isolation and
  control.

Would you like to discuss specific tools or architectures (e.g., service meshes)
that could help implement either approach?

Me:

> Yeah and some of the edges need whitelisting in third-party systems. I would
> be able to replace other certificates from the same service without needing to
> re-whitelist.

Mistral:

> Great point, Jakob! Your observation about **whitelisting** is a critical
> factor in this decision. Let’s refine the benefits and drawbacks with this new
> context in mind:

---

## **Per-Service Certificate (Each Node Has Its Own Certificate)**

### **Benefits in Your Context**

- **Whitelisting Stability**: Since the certificate is tied to the service
  (node), not the connection (edge), you can update or replace the certificate
  for a service without affecting whitelisting rules in third-party systems. The
  service’s identity remains consistent, even if its certificate changes.
- **Simpler Whitelisting**: Third-party systems only need to whitelist the
  service’s certificate or its public key fingerprint, not every connection’s
  certificate.
- **Easier Rotation**: You can rotate a service’s certificate without needing to
  update whitelisting rules elsewhere, as long as the service’s identity (e.g.,
  DNS name or IP) remains the same.

### **Drawbacks in Your Context**

- **Blast Radius**: If a service’s certificate is compromised, all connections
  to/from that service are at risk until the certificate is revoked and
  replaced.
- **Less Granular Control**: You can’t enforce different security policies for
  specific connections between services.

---

## **Per-Connection Certificate (Each Edge Has Its Own Certificate)**

### **Benefits in Your Context**

- **Fine-Grained Security**: Each connection can have its own security profile,
  which is useful if some connections are more sensitive than others.
- **Isolation**: Compromise of one connection’s certificate doesn’t affect
  others, even for the same service.

### **Drawbacks in Your Context**

- **Whitelisting Complexity**: If third-party systems whitelist certificates (or
  their fingerprints), you’ll need to update the whitelist every time you
  replace a certificate for any connection involving that service. This is
  operationally cumbersome and error-prone.
- **Operational Overhead**: Managing and rotating certificates for every
  connection is complex and requires robust automation.

---

## **Key Takeaways for Your Use Case**

- **Whitelisting Favors Per-Service Certificates**: Since whitelisting is tied
  to the service’s identity (not the connection), using per-service certificates
  allows you to rotate certificates without updating third-party whitelists.
  This is a major advantage for operational simplicity and security.
- **Security Isolation**: If you need fine-grained security for specific
  connections, consider using **per-connection certificates only for those
  high-risk edges**, while defaulting to per-service certificates for the rest.
  This hybrid approach balances security and manageability.
- **Automation is Critical**: Even with per-service certificates, automate
  rotation and deployment (e.g., using Cert-Manager, Vault, or a service mesh)
  to reduce manual errors and downtime.

---

## **Practical Recommendation**

Given your whitelisting requirement, **per-service certificates** are likely the
best default choice. They simplify whitelisting and certificate rotation while
still providing strong security. For connections requiring extra isolation
(e.g., between highly sensitive services), you could use **per-connection
certificates** as exceptions.

Would you like to explore tools or architectures (e.g., service meshes,
certificate managers) that can help automate and secure this setup?

Me:

> Do you have some links to authorities talking about this?

Mistral:

There isn’t a single authoritative IETF RFC or standard that directly compares
"per-service" vs. "per-connection" certificate strategies in the way you
described. However, the industry best practices and discussions around mutual
TLS (mTLS) and service mesh architectures—especially in microservices
environments—provide strong guidance on the trade-offs and benefits of each
approach.

Here’s what the most relevant and authoritative sources say about certificate
management in service-to-service communication:

---

### **Per-Service Certificates (Each Node Has Its Own Certificate)**

- **Best Practice**: This is the most common approach in service mesh
  architectures (e.g., Istio, Linkerd, Consul). Each service (or workload) is
  issued its own certificate, typically by a central Certificate Authority (CA)
  within the mesh. This is often referred to as "workload identity" or "service
  identity" in the context of mTLS.
- **Benefits**:
  - Simplified management: Each service only needs to manage its own
    certificate.
  - Easier revocation: If a service is compromised, only its certificate needs
    to be revoked.
  - Scalability: Adding a new service only requires issuing one new certificate.
  - Compatibility: Works well with automation tools like cert-manager, HashiCorp
    Vault, or service mesh built-in CAs.
- **Drawbacks**:
  - If a service’s certificate is compromised, all connections involving that
    service are at risk until the certificate is revoked.
  - Less granular control over individual connections.

**Sources**:

- [Istio Certificate Management](https://tetrate.io/blog/how-are-certificates-managed-in-istio)
  explains how Istio issues and manages certificates per workload, with granular
  revocation and rotation capabilities.
- [Consul Service Mesh CA Overview](https://developer.hashicorp.com/consul/docs/connect/ca)
  describes how Consul’s built-in CA issues certificates to each service,
  enabling mTLS for all service-to-service communication.
- [Service Mesh and mTLS Best Practices](https://www.appviewx.com/blogs/why-mutual-tls-mtls-is-critical-for-securing-microservices-communications-in-a-service-mesh)
  highlights the use of per-service certificates for mutual authentication and
  zero-trust networking.

---

### **Per-Connection Certificates (Each Edge Has Its Own Certificate)**

- **Best Practice**: This approach is less common in practice due to its
  complexity, but it is discussed in the context of ultra-high-security
  environments or where connection-specific policies are required.
- **Benefits**:
  - Fine-grained security: Each connection can have its own security policy, and
    compromise of one connection’s certificate doesn’t affect others.
  - Isolation: Ideal for environments where connections must be strictly
    isolated (e.g., multi-tenant or regulated environments).
- **Drawbacks**:
  - Complex management: The number of certificates scales with the number of
    connections, not just services.
  - Operational overhead: Requires advanced automation and tooling to manage the
    lifecycle of each certificate.
  - Performance: More certificates mean more TLS handshakes and potential
    overhead, though this is usually minimal in practice.

**Sources**:

- [Service Mesh Deployment Best Practices](https://tetrate.io/blog/service-mesh-deployment-best-practices-for-security-and-high-availability)
  discusses the use of short-lived certificates and granular revocation, which
  can be extended to per-connection scenarios.
- [Mutual TLS in Microservices](https://www.redhat.com/en/blog/service-mesh-mtls)
  explains how mTLS can be implemented with per-service or per-connection
  certificates, depending on the security requirements.

---

### **Your Specific Use Case: Whitelisting and Certificate Replacement**

- **Per-Connection Certificates** make sense if you need to whitelist specific
  connections in third-party systems and want to replace certificates for one
  connection without affecting others. This aligns with the principle of least
  privilege and minimizes the blast radius of a compromise.
- **Automation is Key**: To manage the complexity, you’ll need a robust
  certificate management system (e.g., service mesh, HashiCorp Vault, or
  cert-manager) to automate issuance, rotation, and revocation.

---

### **Further Reading**

- [Istio Certificate Management](https://tetrate.io/blog/how-are-certificates-managed-in-istio)
- [Consul Service Mesh CA](https://developer.hashicorp.com/consul/docs/connect/ca)
- [Service Mesh and mTLS Best Practices](https://www.appviewx.com/blogs/why-mutual-tls-mtls-is-critical-for-securing-microservices-communications-in-a-service-mesh)

---

Would you like a deeper dive into how to implement either approach with specific
tools like Istio, Linkerd, or HashiCorp Vault?
