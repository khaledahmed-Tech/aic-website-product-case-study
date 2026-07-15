# Active Impact Club Website — Product Decisions

## Purpose

This document records the key product and delivery decisions made while defining, building, and launching the Active Impact Club website.

The goal is not only to describe what was delivered, but to show the reasoning, trade-offs, and constraints behind the product. These decisions reflect the needs of a newly launched nonprofit organization, the importance of trust and privacy, the available operating capacity, and the need to avoid unnecessary technical complexity.

---

## Decision Summary

| Decision | Chosen Approach | Primary Reason |
|---|---|---|
| Product scope | Public informational and participation website | Establish a credible digital presence without overbuilding |
| Architecture | Static-first website | Lower complexity, faster delivery, simpler operations |
| Hosting | Cloudflare Pages | Integrated deployment, serverless functions, environment variables, and Turnstile |
| Source control | GitHub organization repository | Keep production ownership with AIC |
| Personal portfolio | Separate case-study repository | Present product leadership without duplicating production code |
| AI-assisted delivery | Codex-supported implementation with human review | Accelerate execution while retaining accountability |
| Form integration | Same-origin Cloudflare Pages Function | Protect API credentials and validate requests server-side |
| Project configuration | Project metadata sourced from the Projects page | Reduce duplicated hard-coded project lists |
| Participation UX | Conditional first-time and returning-participant paths | Reduce unnecessary data entry and improve relevance |
| Bot protection | Cloudflare Turnstile | Reduce automated abuse without exposing secret keys |
| Initial launch | No authentication, CMS, database, or admin portal | Avoid premature platform complexity |
| Content strategy | English-first public website | Launch with consistent, maintainable content |
| Governance | Privacy, dignity, transparency, and accurate expectations as product requirements | Build trust and reduce reputational risk |
| Release approach | Incremental branches, pull requests, validation, and production verification | Reduce change risk and preserve traceability |

---

## 1. Launch a Focused Public Product Before Building an Internal Platform

### Context

AIC needed a public digital presence that could explain the organization, present its committees and projects, communicate guiding principles, and provide a structured way for people to participate.

There were also many possible future capabilities, including member accounts, automation, internal administration, project reporting, contribution management, and communications.

### Decision

Launch a focused public website first, rather than attempting to build a full nonprofit operations platform.

### Why

The immediate product need was public credibility and clear participation—not internal software complexity.

A smaller first release:

- Reduced time to public launch
- Allowed AIC to validate its content and operating model
- Avoided building workflows before the organization had fully stabilized them
- Reduced cost, maintenance, security, and privacy obligations
- Created a foundation that could evolve later

### Trade-off

Some internal processes remain manual and future operational capabilities are not yet available through the website.

### Outcome

AIC launched a usable public product without being delayed by lower-priority platform features.

---

## 2. Use a Static-First Architecture

### Context

The website primarily consists of public content, navigation, project information, participation forms, and contact workflows.

### Decision

Use a static-first frontend built with HTML, CSS, and JavaScript.

### Why

A static architecture was sufficient for the launch scope and provided:

- Fast page delivery
- Low hosting complexity
- Simple deployment
- Reduced operational overhead
- A smaller security surface
- Easy integration with Cloudflare Pages
- Direct control over content and responsive behavior

### Alternatives Considered

- A full web application framework
- A database-backed content platform
- A traditional content-management system
- A hosted website builder

### Why They Were Not Chosen Initially

These options would have introduced additional dependencies, administration, costs, and maintenance responsibilities that were not necessary for the first release.

### Trade-off

Content updates require source-control changes rather than being managed through an editorial interface.

### Revisit When

AIC has multiple regular content editors, higher publishing frequency, or a need for structured approval workflows.

---

## 3. Keep Production Source Code Under the AIC GitHub Organization

### Context

The website was led and delivered through my product work, but it is an organizational product belonging to Active Impact Club.

### Decision

Maintain the official production repository within the Active Impact Club GitHub organization.

### Why

This creates a clear separation between:

- Organizational ownership
- Production source of truth
- Personal portfolio documentation
- Future maintenance responsibilities

It also reduces the risk that AIC becomes dependent on one individual’s personal account for production continuity.

### Trade-off

My personal GitHub does not contain the production source code.

### Mitigation

A separate personal case-study repository documents my product leadership, architecture decisions, screenshots, delivery approach, and outcomes while linking to the official organization.

---

## 4. Use a Separate Personal Product Case Study Instead of Copying the Code

### Context

The project is valuable evidence of product leadership, AI-assisted delivery, technical decision-making, UX ownership, and launch execution.

### Decision

Create a documentation-focused portfolio repository under my personal GitHub rather than duplicating or forking the production website as the primary portfolio artifact.

### Why

A case study communicates the aspects most relevant to product leadership:

- Product challenge
- Role and responsibilities
- User journeys
- Architecture
- Key decisions
- Governance
- Delivery process
- Lessons learned
- Product impact

It also avoids ownership confusion and keeps the official repository authoritative.

### Trade-off

Visitors cannot inspect the full production code from the personal repository unless they follow the official organization link.

### Outcome

The portfolio presents the work as a real product launch rather than only a coding exercise.

---

## 5. Use AI as a Delivery Multiplier, Not an Autonomous Decision-Maker

### Context

The project was delivered using Codex-supported workflows across implementation, debugging, content refinement, responsive improvements, and release preparation.

### Decision

Use AI to accelerate execution while retaining human ownership over product requirements, validation, acceptance, and release decisions.

### Why

AI-assisted development improved delivery speed, but generated output still required:

- Clear requirements
- Focused prompts
- Scope control
- Business validation
- UX review
- Security review
- Accessibility review
- Testing
- Iteration and correction

### Operating Principle

AI-generated output was treated as a proposal to review, not as automatically production-ready work.

### Trade-off

Human review remains necessary and can require multiple iterations when the generated result does not meet the intended standard.

### Outcome

The project demonstrated an AI-enabled product management model that accelerated delivery without removing accountability.

---

## 6. Protect External API Credentials Behind Cloudflare Pages Functions

### Context

The Participation Form needed to send data to an external service that required an API key.

Placing the API key in browser JavaScript would expose it publicly.

### Decision

Submit the form to a same-origin Cloudflare Pages Function that acts as a secure server-side proxy.

### Why

The function can:

- Read credentials from protected environment variables
- Validate requests
- Normalize payloads
- Enforce method and content-type rules
- Limit request size
- Verify Turnstile tokens
- Add the API key server-side
- Return safe error messages
- Prevent upstream details from being exposed to users

### Alternatives Considered

- Direct browser-to-API requests
- A separate hosted backend server
- Embedding credentials in frontend configuration

### Why They Were Not Chosen

Direct browser requests would expose credentials. A separate backend would add unnecessary operational complexity for the initial scope.

### Trade-off

The serverless function introduces an additional integration layer that must be configured and monitored.

### Outcome

The website could integrate with the external participation service without exposing protected credentials.

---

## 7. Use Same-Origin API Endpoints

### Context

The browser needed to submit Contact and Participation forms securely.

### Decision

Use same-origin endpoints such as `/api/participate`.

### Why

This approach:

- Keeps browser integration simple
- Reduces cross-origin configuration complexity
- Creates a controlled security boundary
- Keeps external service details out of the frontend
- Allows Cloudflare Pages and Functions to operate as one product surface

### Trade-off

The frontend depends on the Cloudflare-hosted function layer being correctly deployed and configured.

---

## 8. Make the Projects Page the Source of Project Metadata

### Context

The Participation Form needed to display only active projects and show project-specific beneficiary locations.

Maintaining independent hard-coded lists in both the Projects page and the Participation Form could create inconsistencies.

### Decision

Store approved project metadata on the current project cards and allow the Participation Form to read it.

The metadata includes:

- Project title
- Project summary
- Beneficiary locations

### Why

This lightweight source-of-truth pattern:

- Reduces duplication
- Improves maintainability
- Keeps the participation options aligned with public project content
- Allows new projects to be added with fewer coordinated changes
- Supports project-specific form behavior

### Trade-off

The approach depends on stable HTML metadata conventions and is less robust than a formal content API or structured data store.

### Revisit When

The number of projects grows, project data becomes more complex, or multiple systems need to consume the same project information.

---

## 9. Use Conditional Participation Paths

### Context

First-time participants need to provide more information than returning participants.

Showing every field to every user would increase effort and create unnecessary data collection.

### Decision

Create separate first-time and returning-participant experiences within the same form.

### Why

This improves:

- Form relevance
- Completion speed
- Data minimization
- User clarity
- Validation behavior
- Overall participation experience

Hidden or inactive fields are disabled so they do not block validation or appear in the submitted payload.

### Trade-off

Conditional forms require more careful testing than a single fixed form.

### Outcome

The workflow supports a detailed onboarding path for new participants and a shorter path for returning participants.

---

## 10. Populate Beneficiary Locations Based on the Selected Project

### Context

A generic location list could allow users to select a beneficiary location that is not supported by the chosen project.

### Decision

Show only beneficiary locations associated with the selected project.

### Why

This:

- Improves data quality
- Reduces user confusion
- Prevents invalid project-location combinations
- Makes the form easier to maintain
- Creates a clearer relationship between public project information and participation data

### Trade-off

Project metadata must remain accurate whenever project geography changes.

---

## 11. Add Cloudflare Turnstile to Public Forms

### Context

Public forms can be targeted by automated submissions and spam.

### Decision

Use Cloudflare Turnstile for bot verification and enforce the result server-side.

### Why

Turnstile aligned with the existing Cloudflare environment and provided:

- Automated abuse protection
- A user-facing verification step
- Server-side token validation
- No exposure of the secret key
- A consistent pattern across public forms

### Trade-off

Bot verification adds a small amount of friction and depends on a third-party service.

### Risk Control

The secret is stored only in the deployment environment, and the server does not trust the browser token without verifying it.

---

## 12. Avoid Authentication in the Initial Public Release

### Context

The website is public and does not currently require protected member-only content or self-service accounts.

### Decision

Do not add user registration, sign-in, role management, or access control to the initial release.

### Why

Authentication would introduce:

- Identity-management complexity
- Password or account-security responsibilities
- Additional privacy obligations
- User-support needs
- Role and permission design
- Higher implementation and testing effort

None of these were required to meet the initial product goals.

### Trade-off

The website cannot yet support personalized dashboards or protected internal workflows.

### Revisit When

AIC has validated member-only use cases, ownership roles, privacy requirements, and administrative capacity.

---

## 13. Avoid a Public Database in the Initial Website Architecture

### Context

The public website could operate without directly exposing or managing a general-purpose application database.

### Decision

Do not introduce a new website-managed public database for the launch.

### Why

This reduced:

- Security exposure
- Data-retention obligations
- Backup and recovery responsibilities
- Administrative complexity
- Privacy risk
- Operational dependencies

### Trade-off

The website does not provide public account history, contribution history, or self-service participant records.

---

## 14. Avoid a CMS Until the Editorial Need Is Proven

### Context

A content-management system would allow nontechnical editors to change pages, but it would also introduce another platform to configure, secure, train, and maintain.

### Decision

Manage content through GitHub for the initial release.

### Why

At launch:

- The number of editors was small
- Content changes required careful governance
- GitHub provided traceability and review
- Publishing frequency did not justify a separate CMS
- Technical simplicity was more valuable than editorial convenience

### Trade-off

Content editors need GitHub support or a structured change request process.

### Revisit When

Publishing frequency and editorial participation increase enough to justify the added platform.

---

## 15. Treat Trust, Privacy, and Dignity as Product Requirements

### Context

AIC works with contributions, humanitarian needs, youth, families, and beneficiaries in difficult circumstances.

### Decision

Embed trust-related principles directly into product requirements and content decisions.

### Why

The website needed to clearly communicate:

- Volunteer-led operations
- How contributions are directed
- The absence of charitable tax receipts
- Respect for beneficiary privacy and dignity
- Non-political and non-factional positioning
- Accurate public expectations
- Responsible use of proof-of-delivery information

### Product Impact

These principles affected:

- Page content
- Form wording
- Confirmation statements
- Contribution expectations
- Image and story selection
- Data collection
- Public claims
- Future feature boundaries

### Trade-off

Some content requires more careful review and cannot be published as quickly as ordinary marketing material.

---

## 16. Use English as the Initial Public Website Language

### Context

The source material included both Arabic and English, and the audience may include speakers of both languages.

### Decision

Launch the public website with a consistent English experience while preserving the possibility of future multilingual support.

### Why

A single launch language reduced:

- Content duplication
- Translation inconsistency
- Navigation complexity
- Maintenance overhead
- Risk of one language becoming outdated
- Layout and responsive testing complexity

Arabic source information was translated and rewritten into clear English rather than copied literally.

### Trade-off

Arabic-speaking visitors do not yet have a fully localized interface.

### Revisit When

AIC can support a complete translation workflow, content ownership, review, and ongoing synchronization across languages.

---

## 17. Use Incremental Delivery Through Branches and Pull Requests

### Context

The website required many small refinements across content, forms, navigation, responsive behavior, API integration, accessibility, and launch hardening.

### Decision

Break changes into focused branches and pull requests rather than making large unreviewed updates directly in production.

### Why

This approach provided:

- Smaller review scope
- Clear change history
- Easier rollback
- Better validation
- Reduced risk
- Traceable product decisions
- Safer use of AI-assisted implementation

### Trade-off

Incremental delivery produces more pull requests and requires disciplined review.

### Outcome

The product evolved through controlled changes rather than a single high-risk release.

---

## 18. Separate Launch-Critical Work from Future Enhancements

### Context

There were many desirable improvements, but delaying launch until every idea was delivered would have created an open-ended project.

### Decision

Define launch readiness around essential product quality and move noncritical enhancements to future iterations.

### Launch-Critical Areas

- Accurate public content
- Working navigation
- Responsive layouts
- Functional participation and contact workflows
- Secure API handling
- Bot verification
- Clear confirmation behavior
- Accessibility hardening
- Production deployment
- Domain configuration
- Privacy and trust messaging

### Deferred Areas

- Member accounts
- Internal dashboards
- Automated reporting
- Advanced analytics
- CMS capabilities
- Contribution reconciliation
- Operational workflow automation

### Why

The website needed to become useful and public before AIC invested in secondary capabilities.

---

## 19. Use Screenshots and Documentation as the Personal Portfolio Artifact

### Context

A portfolio needs to make the product understandable to hiring managers, product leaders, and technical reviewers without exposing sensitive data or confusing the official source of truth.

### Decision

Use approved screenshots, product documentation, architecture diagrams, decision records, and links to the live site.

### Why

This demonstrates:

- End-to-end ownership
- Product judgment
- Technical fluency
- AI-assisted delivery
- UX leadership
- Governance awareness
- Release discipline
- Real production impact

It also avoids including participant information, credentials, or duplicated production code.

---

## Decision Principles

Across the project, the main principles were:

1. **Launch the smallest credible product that solves the immediate need.**
2. **Keep protected credentials and validation outside the browser.**
3. **Do not introduce operational complexity before the organization can support it.**
4. **Use AI to accelerate work, but retain human accountability.**
5. **Treat trust, privacy, accessibility, and dignity as product requirements.**
6. **Create one clear source of truth wherever practical.**
7. **Separate organizational production ownership from personal portfolio storytelling.**
8. **Prefer incremental, reviewable changes over large unvalidated releases.**
9. **Document both the outcome and the reasoning behind it.**
10. **Design the first release so it can evolve without requiring premature platform investment.**

---

## Future Decision Areas

As AIC grows, future decisions may include:

- Whether to introduce a CMS
- Whether member authentication is justified
- How to structure internal roles and permissions
- Where participant and contribution data should be governed
- How long data should be retained
- How multilingual content should be managed
- Whether to add project reporting and impact dashboards
- How communication preferences and consent should be maintained
- Which workflows should be automated
- What monitoring, analytics, and service-level expectations are appropriate
- Whether a dedicated internal operations platform is needed

These decisions should be made based on validated organizational needs rather than technical possibility alone.

---

## Product Leadership Perspective

The most important product decision was to avoid treating the initiative as only a website build.

The work required aligning the organization’s mission, programs, contribution model, governance principles, public messaging, user journeys, technical architecture, delivery process, and future operating needs.

The resulting product is intentionally simple at the infrastructure level, but it reflects deliberate choices about security, trust, maintainability, ownership, and organizational readiness.
