# Six Thinking Hats — Applied Engineering Examples

## Example: Custom Auth vs Auth0

**White**: Auth0 costs $X/month at scale. Custom auth: 3 weeks senior dev. Need SSO, MFA, social login.
**Red**: Team prefers building — feels more "professional." Auth0 docs frustrate lead dev. Customer success worries about vendor lock-in.
**Black**: Custom auth is a security liability. Token rotation bugs. Password storage mistakes. Compliance gaps.
**Yellow**: Auth0 gives SOC2 out of the box. Frees 3 weeks for product features. Battle-tested against attacks we haven't imagined.
**Green**: Auth0 now, migration path later. Open-source alternative (Keycloak, Supertokens). Hybrid: Auth0 for social, custom for internal SSO.
**Blue**: Red Hat revealed the real tension — partly an ego decision. Black strongly favors managed auth. Green hybrid merits deeper analysis.

## Example: Kubernetes Adoption

**White**: Current: 3 EC2 instances, manual deploys. 5 services, team of 4. Uptime requirement: 99.9%.
**Red**: DevOps engineer excited — career growth. Backend devs anxious about complexity. CTO heard "everyone uses K8s."
**Black**: K8s learning curve: 3-6 months. Team of 4 cannot sustain on-call for K8s cluster + apps. YAML sprawl.
**Yellow**: Auto-scaling. Self-healing. Standardized deployments. Easier to onboard future devs.
**Green**: ECS/Fargate (managed, simpler). Docker Compose + CI/CD improvements. K8s only for the one service that needs scaling.
**Blue**: Red Hat exposed that this is career-driven, not problem-driven. White Hat shows current setup handles load fine. Green's "improve what we have" deserves serious consideration.
