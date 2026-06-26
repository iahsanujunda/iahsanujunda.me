+++
title = 'Testing is where Spring Boot earns it'
date = 2026-06-26T21:00:00+09:00
draft = true
+++

<!--
THESIS: Bootstrapping a FastAPI *app* is easier — concede that up front.
But bootstrapping *tests* is where Spring Boot quietly does the work for you.
Built around one example: refunding an invoice across three layers.
Keep it honest: "defaults gap," not "FastAPI can't do it."
-->

## The concession first

<!--
Lead with the honest part so the rest is credible.
- Starting a FastAPI app is genuinely easier: pip install, ~5 lines, uvicorn, done.
- Spring Boot has more ceremony to get hello-world running.
- The turn: but writing tests is where I kept fighting FastAPI, and Spring Boot
  just did it for me. This post is about that.
-->

## The example: refunding an invoice

<!--
Three layers everyone has, same convention in both apps:
- Controller — routing, auth, role check (the full contract)
- Service — business logic of the refund
- Repository / ORM — reads and writes the DB
Sketch the layers so the rest of the post has something concrete to point at.
-->

## Slicing the layer I actually want to test

<!--
The core asymmetry: Spring DEFINES the slices; FastAPI makes you rebuild them.
- Spring: @WebMvcTest(RefundController) + @MockBean RefundService + @WithMockUser
  -> loads only the web layer, auth/roles testable in isolation.
- FastAPI: TestClient(app) + app.dependency_overrides -> no "web layer only"
  loader, you boot the app and wire overrides by hand every time.
Side-by-side snippets here.
-->

## The arrange step: "a refundable invoice must exist first"

<!--
- Spring: invoiceRepository.save(refundableInvoice()) or @Sql(...). Runs inside
  the test transaction, request sees it, rolled back after. Order-independent.
- FastAPI: build a fixture, and now you own fixture SCOPE + cleanup.
- Name the footgun: the app's Depends(get_db) session vs the fixture's session
  are often DIFFERENT -> arranged record not visible / not rolled back unless you
  override get_db to share the transaction-bound session.
-->

## The part that bit me: passes alone, fails in the full run

<!--
Precise framing — NOT "race condition". It's state leaking between tests because
FastAPI has no default rollback.
- Spring: @Transactional test methods roll back automatically -> free per-test
  isolation, nothing leaks, order doesn't matter.
- FastAPI: no default isolation; the fast/obvious fixture choices silently leak.
-->

## Being fair: it's a defaults gap, not impossibility

<!--
- Show the FastAPI fix: session-scoped engine + function-scoped nested
  transaction (SAVEPOINT) rolled back per test -> Spring-like isolation.
- The perf claim (shared-testcontainer suite runs longer in FastAPI): keep it
  soft, framed as defaults-vs-defaults (rollback + cached context vs
  recreate/truncate + fresh engine). Note it can be sped up.
-->

## Why it feels more delightful

<!--
Tie back to the recurring theme from the Kotlin post: the framework bootstraps
correctness for me, so I spend energy on the interesting parts instead of
re-deriving fundamentals. "More delightful" stated plainly as my experience.
-->
