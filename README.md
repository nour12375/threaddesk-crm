# ThreadDesk CRM – Inbox-to-Revenue MVP

**Turn email chaos into structured revenue — without forcing your team into another CRM.**

Modern lightweight **B2B CRM** that treats the **inbox as primary source of truth**.  
Automatically extracts contacts, deals, action items and summaries from email threads using AI — then drives structured sales workflows (deal → quote → invoice).

<br>

<p align="center">
  <img src="https://nethunt.com/blog/content/images/2026/01/Screenshot-2026-01-16-at-20.03.20-1.png" width="720" alt="Example of inbox-integrated CRM pipeline view">
  <br><em>ThreadDesk aims for this kind of inbox-native experience — but fully self-hosted & AI-first</em>
</p>

<br>

## One-liner Value Proposition

Reduce lost leads, shorten quote cycles and gain visibility into email-driven sales — without double data entry or forcing salespeople to live in yet another SaaS tool.

## Target Audience

Small & medium B2B teams that still live in email:  
marketing agencies • consultants • technical service providers • energy & industrial suppliers • B2B wholesalers

## Core Features (MVP Scope)

1. **Email Intelligence Pipeline**
   - IMAP polling & ingestion
   - Thread grouping + plain-text normalization
   - Auto-create Contact / Lead from sender
   - AI-generated thread summary + strict-JSON action items

2. **Revenue Workflow**
   - One-click deal creation from thread
   - Quote generation (line items from product catalog)
   - Basic invoice generation
   - Customer 360° timeline (emails + tasks + sales events)

3. **Product Catalog**
   - CSV / XLSX bulk import
   - Quote line-item support

4. **Architecture Pillars**
   - Strict multi-tenant isolation
   - Event-driven microservices
   - Everything runs via Docker Compose (one command local demo)

## Why ThreadDesk vs HubSpot / Pipedrive / Zoho / Salesforce?

| Aspect                        | ThreadDesk (MVP)                  | Traditional CRMs                     |
|-------------------------------|------------------------------------|--------------------------------------|
| Primary data entry            | Email thread (automatic)          | Manual form filling                  |
| Email integration depth       | Native thread → CRM entity        | Side panel / sync / add-on           |
| Self-hostable                 | Yes – Docker Compose              | No (SaaS only)                       |
| AI summaries & actions        | Built-in (strict schema)          | Add-on / expensive                   |
| Deployment footprint          | Very lightweight                  | Heavy / enterprise                   |
| MVP sell-ability              | Designed to be packaged & sold    | —                                    |

## Current MVP Limitations (be transparent)

- Only IMAP support (no Gmail/Outlook native API yet)  
- Manual invoice payment status tracking  
- Basic functional UI (Next.js – not enterprise polish)  
- No forecasting / advanced analytics  
- No outgoing email / mail merge yet

## Technical Stack

- **Backend**     : Microservices (Node.js / Go / Python – pick per service)  
- **Database**    : PostgreSQL (per-service ownership)  
- **Messaging**   : NATS (event bus)  
- **Cache**       : Redis  
- **Storage**     : MinIO (S3-compatible files)  
- **Frontend**    : Next.js (App Router)  
- **Local/Dev**   : Docker Compose (full stack in one file)  

### High-level Architecture

```mermaid
graph TD
    A[IMAP Mailbox] -->|polling| B[Email Ingestion Service]
    B -->|raw thread| C[Thread Normalizer + Grouper]
    C -->|normalized thread| D[AI Summary & Action Extractor]
    D -->|JSON: summary + actions| E[NATS Event Bus]
    
    E --> F[Contact / Lead Service]
    E --> G[Deal Service]
    E --> H[Quote & Invoice Service]
    E --> I[Timeline / Customer 360 Service]
    
    subgraph Product Catalog
        J[(PostgreSQL – Catalog DB)]
        K[CSV/XLSX Import Service] --> J
    end
    
    G -->|quote request| H
    H -->|line items| J
    
    L[Next.js UI] <-->|REST / Server Actions| M[API Gateway / BFF]
    M <--> E
    
    subgraph Persistence
        N[(PostgreSQL – per service)]
        O[(Redis)]
        P[(MinIO – attachments)]
    end
    
    style E fill:#f9f,stroke:#333,stroke-width:2px