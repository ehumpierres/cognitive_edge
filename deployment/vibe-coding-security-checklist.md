# Vibe Coding Security Checklist: 20 Ways AI-Generated Apps Sink

**Source:** [Harshil Tomar (@Hartdrawss)](https://x.com/Hartdrawss)  
**Date:** 2026-04-12  
**Tags:** #security #vibe-coding #deployment #checklist #anti-patterns #production

---

## TL;DR

AI-generated code is confident, compiles, and passes basic tests — but often ships with critical security and operational gaps. This is the checklist of what vibe-coded apps get wrong. These aren't edge cases; they're the defaults when you don't explicitly ask for them.

---

## The 20 Failure Modes

### Authentication & Authorization

| # | Problem | Consequence |
|---|---------|-------------|
| 2 | Auth tokens in localStorage | One XSS attack = all user accounts compromised |
| 8 | Sessions that never expire | Stolen token = permanent access forever |
| 10 | Password reset links that don't expire | Old email in someone's inbox = instant account takeover |
| 16 | Admin routes with no role checks | Any logged-in user can access admin panel |

### Input & API Security

| # | Problem | Consequence |
|---|---------|-------------|
| 1 | No rate limiting on API routes | Anyone can spam backend into $500 bill overnight |
| 3 | No input sanitization on forms | SQL injection still works in 2026 |
| 4 | Hardcoded API keys in frontend | Someone WILL find them within 48 hours |
| 5 | Stripe webhooks with no signature verification | Anyone can fake a successful payment |
| 13 | No CORS policy | Any website can make requests to your API |

### Database & Performance

| # | Problem | Consequence |
|---|---------|-------------|
| 6 | No database indexing on queried fields | Works at 100 users, dies at 1,000 |
| 9 | No pagination on database queries | One fetch loads entire database into memory |
| 15 | No database connection pooling | First traffic spike = database crashes |
| 19 | No backup strategy | One bad migration = all user data gone |

### Infrastructure & Operations

| # | Problem | Consequence |
|---|---------|-------------|
| 11 | No environment variable validation at startup | App silently breaks in production |
| 12 | Images uploaded directly to server | No CDN = 8 second load times + massive bill |
| 14 | Emails sent synchronously in handlers | Slow SMTP = entire app hangs |
| 17 | No health check endpoint | App goes down silently, find out from client |
| 18 | No logging in production | When it breaks, zero idea where or why |

### Code Quality

| # | Problem | Consequence |
|---|---------|-------------|
| 7 | No error boundaries in UI | One crash = white screen = user never returns |
| 20 | No TypeScript on AI-generated code | AI writes confident, wrong, untyped code |

---

## The Pattern

These aren't exotic vulnerabilities. They're **defaults that AI doesn't include unless you ask**:

1. **Security features are opt-in** — rate limiting, CORS, signature verification, role checks
2. **Scale considerations are invisible** — indexing, pooling, pagination, CDN
3. **Operational basics are forgotten** — logging, health checks, backups, env validation
4. **AI optimizes for "works" not "works safely"** — confident code that compiles ≠ production-ready

---

## The Checklist

Before shipping any vibe-coded app:

### Auth & Sessions
- [ ] Tokens in httpOnly cookies, not localStorage
- [ ] Sessions expire (reasonable TTL)
- [ ] Password reset links expire (1 hour max)
- [ ] Role checks on all admin/privileged routes

### API Security
- [ ] Rate limiting on all public endpoints
- [ ] Input sanitization (SQL injection, XSS)
- [ ] API keys in environment variables, not code
- [ ] Webhook signature verification (Stripe, etc.)
- [ ] CORS policy configured

### Database
- [ ] Indexes on all queried/filtered fields
- [ ] Pagination on list endpoints
- [ ] Connection pooling configured
- [ ] Backup strategy (and tested restore)

### Infrastructure
- [ ] Environment variables validated at startup
- [ ] Images/assets on CDN
- [ ] Async email sending (queue, not handler)
- [ ] Health check endpoint
- [ ] Logging configured for production

### Code Quality
- [ ] Error boundaries in UI
- [ ] TypeScript strict mode on AI-generated code

---

## Connections

- **To Vercel "Agent Responsibly":** This is the concrete version. Vercel says "green CI ≠ safety" — this is WHY. These 20 items pass tests but fail production.
- **To enterprise readiness gaps:** Output validation problem in action. AI code "works" but doesn't meet security/operational requirements.
- **To "leverage don't rely":** If you can't explain whether your app has rate limiting, you're relying, not leveraging.

---

## Notable Quote

> "SQL injection still works in 2026. Your AI didn't tell you that."

> "AI writes confident, wrong, untyped code and you ship it anyway."

---

## Actionable Takeaway

**Add this to your CLAUDE.md or system prompt:**

```markdown
## Production Requirements
Every endpoint must have:
- Rate limiting
- Input validation (zod)
- Proper error handling

Authentication must use:
- httpOnly cookies (not localStorage)
- Session expiry
- Role-based access control

Database queries must have:
- Indexes on filtered fields
- Pagination for lists
- Connection pooling
```

Make the requirements explicit. AI won't assume them.
