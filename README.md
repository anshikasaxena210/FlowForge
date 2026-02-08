FlowForge
Config-driven workflow and rule engine with versioned configurations, state transitions, runtime rule evaluation, and audit events. Built using Java & Spring Boot.

Why FlowForge?

Many enterprise systems hardcode workflows and business rules, making changes risky and expensive.
FlowForge externalizes workflows and rules into versioned configuration, allowing systems to evolve
without code changes while maintaining safety, traceability, and backward compatibility.

Key Features

- Config-driven workflow definitions (no hardcoded states or transitions)
- Versioned workflow configurations with immutable history
- Action-based state transitions
- Runtime rule evaluation against workflow context
- Audit-grade event logging for all transitions and rule outcomes
- REST APIs with OpenAPI/Swagger documentation

High-Level Flow

1. Workflow configuration is uploaded via API as JSON.
2. FlowForge validates the configuration (states, transitions, rules).
3. A new workflow version is created and stored immutably.
4. Workflow instances are created using the latest configuration version.
5. State transitions are applied using action-based rules.
6. Business rules are evaluated at runtime during transitions.
7. All transitions and rule evaluations are recorded as audit events.

Architecture Overview

Client
  ↓
REST API (Controllers)
  ↓
Service Layer
  - Validation
  - Versioning
  - Rule Evaluation
  ↓
Domain Models
  ↓
Persistence Layer (JPA / Database)
  ↓
Audit Event Store

Sample Workflow Configuration

```json
{
  "workflowKey": "fee-approval",
  "initialState": "DRAFT",
  "states": ["DRAFT", "REVIEW", "APPROVED", "REJECTED"],
  "transitions": [
    { "from": "DRAFT", "to": "REVIEW", "action": "SUBMIT" },
    { "from": "REVIEW", "to": "APPROVED", "action": "APPROVE" },
    { "from": "REVIEW", "to": "REJECTED", "action": "REJECT" }
  ],
  "rules": [
    {
      "name": "HighAmountNewUserNeedsManualReview",
      "condition": "amount > 10000 AND userType == \"NEW\"",
      "effect": "REQUIRE_MANUAL_REVIEW"
    }
  ]
}

API Endpoints

| Method | Endpoint | Description |
|------|--------|------------|
| POST | /configs | Upload and version workflow configuration |
| POST | /instances | Create a workflow instance |
| POST | /instances/{id}/transition | Apply a workflow transition |
| GET | /instances/{id} | Fetch workflow instance |
| GET | /instances/{id}/events | Fetch audit events |

Execution Flow

Configuration Upload
- Validates states and transitions
- Prevents duplicate actions
- Creates a new immutable version

Instance Creation
- Uses latest configuration version
- Starts from initial state
- Stores contextual data

Transition Execution
- Validates allowed transition
- Updates state atomically
- Evaluates configured rules

Audit & Events
- Logs all transitions
- Logs rule evaluation outcomes

Design Principles

- Config over code
- Immutable versioned configurations
- Deterministic state transitions
- Backward compatibility for running instances
- Auditability by default

Tech Stack

- Java 17
- Spring Boot
- Spring Data JPA / Hibernate
- H2 / PostgreSQL
- REST APIs
- OpenAPI / Swagger








