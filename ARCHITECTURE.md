# Cloud Architecture Diagram

## Global CDN Layer
```
┌─────────────────────────────────────────────────────────────────┐
│                    GLOBAL CDN (CloudFront)                       │
│                  Video Streaming, Static Content                 │
└─────────────────────────────────────────────────────────────────┘
```

## Regional Distribution
- **EU Ireland (Primary)**
- **EU North Europe (K8s Primary)**
- **Backup Region (K8s Secondary)**

## Network Layer
```
┌──────────────────────────────────────────────────────────────┐
│               VPC Peering (Low-latency)                       │
└──────────────────────────────────────────────────────────────┘
```

## Kubernetes Multi-Cluster (EKS)

### Microservices
- **Auth Service** - Pod x3
- **Catalog Service** - Pod x5
- **Video Service** - Pod x2
- **Assignment Service** - Pod x2
- **Notification Service** - Pod x2

### Auto-scaling
- **HPA/KEDA** enabled
- Autoscale 2→10x during exam periods

## Ingress Layer
```
┌──────────────────────────────────────────────────────────────┐
│   NGINX Ingress + Cert-Manager (TLS via ACM/Let's Encrypt)  │
│   Rate Limiting | Request Routing | Global Load Balancer    │
└──────────────────────────────────────────────────────────────┘
```

## Data Layer (Regional)

### Databases
- **RDS PostgreSQL** (Multi-AZ)
- **ElastiCache Redis** (Sessions)
- **AWS DocumentDB** (MongoDB - Analytics/Logs)

### Storage
- **Amazon S3** - Video Files, Submissions, Backups

---

## Architecture Flow
```
CDN (CloudFront)
    ↓
Regional Distribution (EU Ireland, EU N.Europe, Backup)
    ↓
VPC Peering
    ↓
K8s Multi-Cluster (EKS)
    ↓
NGINX Ingress
    ↓
Data Layer (RDS, Redis, DocumentDB, S3)
```