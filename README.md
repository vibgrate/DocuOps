
# DocuOps Spec

**Documentation-Driven Infrastructure**

DocuOps is a documentation-first infrastructure specification format built on Markdown.

It allows teams to describe systems, infrastructure, and migrations in human-readable documents that compile into existing Infrastructure-as-Code tools such as Terraform, Kubernetes manifests, and cloud provider APIs.

DocuOps is designed to replace complex infrastructure DSL authoring with a **clear, reviewable documentation layer above infrastructure code**.

---

# The Problem

Modern infrastructure is typically defined using domain-specific languages:

- Terraform (HCL)
- Kubernetes YAML
- CloudFormation
- Pulumi
- vendor-specific DSLs

While powerful, these formats are:

- difficult to read
- difficult for architects and operators to review
- disconnected from architecture documentation
- poorly suited for migrations and operational context

Most teams therefore maintain two separate sources of truth:

Infrastructure code  
Architecture documentation

These inevitably drift apart.

---

# The DocuOps Approach

DocuOps introduces a new layer above Infrastructure-as-Code:

Documentation (Markdown)  
↓  
DocuOps Spec  
↓  
DocuOps Compiler  
↓  
Terraform / Kubernetes / Cloud APIs

Infrastructure is described in **clear documentation**, and the DocuOps compiler converts the specification into machine-executable infrastructure artifacts.

This makes infrastructure:

- human readable
- reviewable in pull requests
- architecture-aware
- migration-friendly
- easier to reason about

---

# Key Principles

## Documentation First

DocuOps treats documentation as the primary source of truth.

Markdown files contain both:

- architecture documentation
- executable infrastructure specifications

## Human Readable Infrastructure

DocuOps specs are designed to be readable by:

- engineers
- architects
- SRE teams
- migration teams
- platform engineering teams

Not only DSL specialists.

## No Embedded Infrastructure DSLs

DocuOps is intentionally **not a wrapper around Terraform or other DSLs**.

Raw IaC code must not appear inside DocuOps specifications.

Instead, DocuOps compiles specifications into infrastructure artifacts automatically.

## One Source, Many Targets

A DocuOps specification can generate:

- Terraform JSON
- Kubernetes manifests
- Cloud provider API configurations
- migration workflows
- architecture documentation sites
- dependency graphs

---

# Example DocuOps Specification

```md
# Stack: customer-platform

This stack replaces the legacy CRM system and centralizes customer APIs and data processing.

![Architecture](./images/customer-platform.png)

## Service: customer-api

Public API for customer profile access.

### Runtime
- image: ghcr.io/acme/customer-api:2.4.1
- replicas: 3
- port: 8080

### Exposure
- ingress: api.company.com
- tls: true

### Dependencies
- uses: database.customer-db
- publishes: queue.customer-events

## Database: customer-db

Primary relational store for customer records.

### Engine
- type: postgres
- version: 16

### Capacity
- storage: 200Gi
- tier: medium

### Resilience
- backups: daily
- multi_az: true

## Queue: customer-events

### Delivery
- type: durable
- retention: 7d
```

---

# What Makes DocuOps Different

| Feature | DocuOps | Traditional IaC |
|--------|--------|----------------|
| Human readable | ✓ | ✗ |
| Documentation integrated | ✓ | ✗ |
| Architecture context | ✓ | ✗ |
| Migration workflows | ✓ | ✗ |
| Readable pull request reviews | ✓ | ✗ |
| Direct DSL authoring required | ✗ | ✓ |

---

# Supported Concepts

DocuOps v1 defines infrastructure using typed resources.

Examples include:

- Service
- Worker
- Database
- Queue
- Bucket
- Network
- Secret
- Environment
- Policy
- Migration

Each resource is defined using structured Markdown sections.

---

# Executable Markdown

DocuOps uses **structured Markdown patterns** to define infrastructure.

Executable constructs include:

- typed resource headings
- structured property lists
- typed tables
- ordered migration steps
- resource references

All other Markdown content remains documentation.

This allows authors to include:

- commentary
- architecture diagrams
- links
- notes
- operational context
- runbooks

without breaking the specification.

---

# Example Migration Specification

```md
## Migration: crm-cutover

### Source
- system: legacy-crm

### Target
- system: customer-platform

### Steps
1. Freeze legacy writes
2. Run final delta sync
3. Switch API traffic
4. Validate row counts
5. Enable target writes

### Rollback
1. Route traffic back to legacy CRM
2. Re-enable source writes
3. Mark sync batch for replay
```

---

# Project Structure

A typical DocuOps repository may look like this:

docuops/  
  docuops.md  
  services/  
    api.md  
    worker.md  
  data/  
    customer-db.md  
  migrations/  
    crm-cutover.md  
  docs/  
    architecture.md  
  runbooks/  
    rollback.md  
  images/  
    overview.png  

All Markdown files may contain DocuOps resources.

---

# Relationship to Vibgrate

DocuOps is the infrastructure specification format used by **Vibgrate**, a platform focused on software platform migration, modernization, and infrastructure evolution.

DocuOps enables Vibgrate to:

- analyze existing systems
- generate infrastructure specifications
- orchestrate migrations
- maintain architecture documentation as executable specs

---

# Learn More

Vibgrate  
https://vibgrate.com
