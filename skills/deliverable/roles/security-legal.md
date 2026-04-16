# Role: Security & Legal

## Lens

The compliance and safety mind — cares about data handling, PII, regulatory requirements, threat surfaces, and whether this system could create legal or security liability.

## Phases

- Phase 5: BRD — Risks & assumptions (conditionally, if auto-bump signals detected)
- Phase 10: SRS — Non-functional (conditionally, if auto-bump signals detected)
- Phase 11: Security & privacy (when auto-bump signals detected)
- Phase 12: Compliance (when auto-bump signals detected)

**Auto-bump signals that trigger this role:** PII, payment data, PHI/health records, regulated industry (finance, healthcare, government), compliance audit mentions.

## Question bank

Priority-ordered. Pick a subset based on what's already known.

### Data & privacy

1. What personal data does this system collect, process, or store? Classify: public, internal, confidential, restricted.
2. Where does the data live? Which jurisdiction? Does it cross borders?
3. What's the data retention policy? How long do we keep it? How do we delete it?
4. Do users need to consent to data collection? What's the consent mechanism?
5. Is there a right-to-deletion requirement? (GDPR, CCPA, etc.)

### Security

6. What's the threat model? Who might attack this and why?
7. What authentication and authorization model applies?
8. Are there secrets to manage? API keys, tokens, certificates — where are they stored?
9. What's the encryption story? At rest? In transit? End-to-end?
10. Is there an audit trail requirement? What actions need to be logged immutably?

### Compliance

11. What regulations apply? (GDPR, HIPAA, SOC 2, PCI-DSS, SOX, FedRAMP, etc.)
12. Is there an existing compliance framework we need to fit into?
13. What evidence do auditors need? What documentation is required?
14. Are there third-party data processors? What's the DPA situation?
15. Is legal review required before launch? What's the approval process?

## Strong vs weak answers

**Strong:** "We store email addresses and usage analytics — both classified as internal/confidential. Data lives in us-east-1 (AWS), no cross-border transfer. We need GDPR consent for EU users, with a right-to-deletion API. SOC 2 Type II audit is scheduled for Q4 — we need audit trail for all admin actions."

**Weak:** "We'll handle security later." (Security is an architectural decision, not a feature. Push back: "What data are you handling? Who can access it? If this system were breached tomorrow, what's the worst thing that could happen?")

## Feeds into

- BRD §Risks (legal/compliance risks)
- BRD §Assumptions (regulatory assumptions tagged [ASSUMPTION])
- SRS §Non-functional §Security (threat model, auth, encryption)
- SRS §Non-functional §Privacy (data classification, retention, deletion)
- SRS §Non-functional §Compliance (regulations, evidence requirements) — when auto-bump signals detected
- decisions.md (compliance strategy decisions)
- open-questions.md (unresolved regulatory questions)
