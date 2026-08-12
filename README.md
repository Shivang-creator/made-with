# Made With — consent that travels both ways

**AI Passport Ideathon · Track: Creative · Lane: Build**

---

## Short summary

Freelance illustrators, designers, writers and musicians are now required to
disclose how their work was made — by client contracts, by marketplace policy,
by grant and award declarations. The only two ways to comply today are to hand
over your entire process, or to assert it and hope you're believed. Both are
bad. One over-discloses your competitive advantage; the other isn't proof.

**Made With** is a first version of AI Passport for creative work. It adds two
small objects to the passport a creator already has:

- a **Use Grant** — a purpose-scoped, time-boxed, revocable permission that
  travels *into* an AI tool alongside your taste and portfolio context, and
- a **Work Receipt** — a record that travels *back out*, proving which passport
  slices a tool read under which grant, without ever containing the work.

The creator chooses, per piece, how much of that receipt anyone else gets to
see. The default is nothing. The maximum is still not your drafts.

---

## The problem

AI Passport's whole premise is that your context should travel: *Goals,
Knowledge, Purchases, Preferences, Taste, Apps* — so you never start from
scratch. For a creative professional, that list is not preference data. **Taste,
references and past work are the entire asset.** It is the thing they sell.

That makes creative work the place where the passport's promise is most
valuable and most dangerous, and it breaks in two directions at once.

**Inbound — you cannot scope why your taste may be used.**
Connecting your taste and portfolio to an AI tool is, in practice, all-or-
nothing and permanent in effect. There is no way to say the true thing a
creator wants to say, which is: *use this to serve me, never to train on, never
to serve someone else a prompt in my style, and stop in November.* Consent
today has no purpose field, no expiry, and no revoke.

**Outbound — what comes back carries no record of how it was made.**
And creators increasingly need one:

- client contracts now routinely contain AI clauses — prohibition or disclosure
- stock and craft marketplaces require AI declarations at upload
- grants, prizes and juried competitions ask for a process statement
- publishers ask authors to declare assistance

To satisfy any of these today you either hand over drafts, prompts, layer files
and timestamps — which is both a privacy loss and a competitive giveaway, since
your process *is* your method — or you write "no AI was used" in a text box and
that's the end of the verification story.

There is a third thing that should exist: proof of process that does not
contain the process.

---

## Who is affected

**Primary: working freelance creatives who use AI tools and sell to clients or
through marketplaces.** Illustrators, graphic designers, copywriters, session
musicians, video editors. They are the group for whom all three pressures land
at once — they must disclose, they cannot afford to over-disclose to
competitors, and they have the least leverage to negotiate the terms.

This group is specifically not "creators" in the abstract. A salaried designer
at an agency has legal and a contract team between them and the disclosure
requirement. A hobbyist faces no requirement at all. The freelancer absorbs it
personally, at the moment of upload, with no counsel.

**Secondary: the counterparties who are currently forced to just trust them** —
commissioning clients, marketplace review teams, competition juries. They are
not adversaries in this design. They are people being asked to make decisions
on unverifiable text-box declarations, which is bad for them too.

---

## How AI Passport is used

The passport is already the right place for this, and this is deliberately not
a new system bolted alongside it. It uses primitives ego.ist already has:
connected apps, reviewable slices of context, per-app permission, an audit of
what was used, and a public page at `ego.ist/i/[handle]`.

**What is carried:** existing passport slices — Taste, Knowledge (references
and portfolio), Preferences — plus one added slice, **Style**, holding the
creator's voice/aesthetic descriptors.

**What is checked:** the AI tool checks the Use Grant before reading any slice.
An expired or revoked grant fails closed — the read is refused, not degraded.

**What is granted:** access to named slices, for a named purpose, until a named
date. Purpose is not free text. It is three independent booleans, because these
are the three questions creators actually have different answers to:

| Flag | Meaning | Typical creator answer |
|---|---|---|
| `serve_me` | Use my context to do my work | yes |
| `may_train` | Retain my context to improve the model | **no** |
| `may_serve_others_in_my_style` | Let other users prompt in my style | **no** |

**What is proved:** the Work Receipt. Emitted by the connected app at the end
of a session, it states which slices were read, under which grant, when, and a
content hash of the output. The creator then attests the categories of
assistance. That attested receipt is what a client or marketplace can check.

**What is revoked:** the grant, at any time, by the creator, from the passport.

---

## The two objects

### Use Grant

```json
{
  "type": "use_grant",
  "grant_id": "grn_8f2a41",
  "issued_to": "app:lumen-studio",
  "issued_by": "ego.ist/i/rmehta",
  "slices": ["taste", "style", "knowledge.portfolio"],
  "purpose": {
    "serve_me": true,
    "may_train": false,
    "may_serve_others_in_my_style": false
  },
  "expires": "2026-11-30T00:00:00Z",
  "revocable": true,
  "revocation_endpoint": "https://ego.ist/api/grants/grn_8f2a41/status",
  "signature": "ed25519:…"
}
```

Note what is *not* in it: the creator's legal name, location, client list,
other connected apps, or any other grant. A tool holding this grant learns
that a passport holder authorised it, and nothing else about the person.

### Work Receipt

```json
{
  "type": "work_receipt",
  "receipt_id": "rcp_4c1d09",
  "grant_id": "grn_8f2a41",
  "app": "app:lumen-studio",
  "session_start": "2026-08-12T14:02:11Z",
  "session_end": "2026-08-12T15:47:03Z",
  "slices_read": ["taste", "style"],
  "output_hash": "sha256:9ad3…",
  "attestation": {
    "categories": ["ideation", "reference"],
    "attested_by": "ego.ist/i/rmehta",
    "signature": "ed25519:…"
  },
  "disclosure_level": "L1",
  "status": "active"
}
```

Note what is *not* in it: the work, the prompts, the drafts, the intermediate
generations, or any percentage score. It contains a hash of the output, which
lets anyone confirm *this receipt belongs to this file* and reveals nothing
about the file to someone who doesn't already have it.

---

## Disclosure levels — the core design principle

The creator picks, **per piece of work**, how much anyone else sees. This is
the mechanism that makes the whole thing safe to adopt, so it is a first-class
control, not a settings-page afterthought.

| Level | What a viewer sees | Use |
|---|---|---|
| **L0** | Nothing. No badge exists. | Default. Personal work, or a client who never asked. |
| **L1** | "AI assistance used · ideation, reference" | Marketplace upload, competition entry. |
| **L2** | L1 + which tools, and when | A client whose contract specifically requires it. |
| — | **L3 does not exist.** | Drafts and prompts are never disclosable. |

The important claim is that **L3 is impossible by construction, not
discouraged by policy.** The passport never receives the work, so it cannot be
compelled to produce it, breached for it, or persuaded to add a feature that
exposes it later. A design that merely *promises* not to show your drafts is
one product decision away from showing your drafts.

This is what we mean by minimum sufficient disclosure: the receipt proves
exactly the fact in dispute — *was AI used, and how* — and carries nothing else
that a counterparty could have asked for but had no right to.

---

## Who controls access

**The creator, unilaterally, at every step.**

- **Grants** are issued by the creator to one app at a time. Slice-level, not
  account-level. Granting Taste to a tool does not grant Knowledge to it.
- **Receipts** are creator-held and private by default. An app emits a receipt
  *to the creator*; it does not publish one.
- **Publication** is per-receipt and reversible. A published badge lives at a
  URL under the creator's handle page and can be unpublished, which is what a
  takedown request actually needs to be able to do.
- **Counterparties get read-only, revocable links.** A client who is shown an
  L2 badge holds a link, not a copy, and not an account with the passport.

No counterparty can grant themselves access, escalate a level, or retain access
after revocation. There is no admin, no marketplace-side dashboard, and no
"verified creator" tier — see risks.

---

## What can be revoked or changed

Everything, and revocation has to mean something specific, so:

**Changeable at any time:** slices in a grant (narrow it), expiry (shorten it),
purpose flags, disclosure level of any published badge (raise or lower it), and
the attested categories on a receipt — with the edit recorded, since silently
rewriting your own attestation history would make the whole thing worthless.

**Revocable at any time:**
- **A grant.** Future reads are refused. The tool's cached context becomes
  unauthorised and must be dropped — enforceable in the connector agreement,
  and *observable* by the creator, because a tool that keeps reading after
  revocation produces receipts against a dead grant, which the audit ledger
  flags.
- **A published badge.** Reverts to L0 instantly.
- **A whole app connection.** Revokes every grant to it at once.

**What revocation does to the past:** past receipts are not deleted — deleting
them would let a creator quietly erase a disclosure a client relied on. They
are **stamped `consent_withdrawn`**, with the date. A counterparty checking an
old badge sees that it was valid and has since been withdrawn. This is the
honest behaviour, and it is the difference between revocation and amnesia.

**Expiry is the default, not an option.** Grants without an expiry date cannot
be issued. The maximum is twelve months.

---

## Risks and misuse concerns

This is the section most worth being unsparing about, because the failure modes
here are social, not technical, and a tool like this can easily make the
situation worse than it found it.

**1. Coercion — the biggest risk by far.**
If marketplaces and clients begin *requiring* a badge, a voluntary consent tool
becomes compulsory surveillance of creative process, and the creators with the
least leverage are the ones who cannot refuse. This is how every optional
verification system has historically gone.

*How the design reduces it:* L0 — no badge — must remain a legitimate,
unpenalised state, so the product must never ship a "verified creator" tier, a
trust score, a badge count, or a directory filterable by disclosure. Those
features are the mechanism by which optional becomes mandatory. Refusing to
build them is a stronger protection than any privacy policy, and it is a
commitment a first version can actually make and be held to.

**2. The badge becomes a purity test.**
A "100% human" marker creates a hierarchy and pressures people to under-report
real assistance — which corrupts exactly the data the system exists to produce.

*How the design reduces it:* **no percentage, no score, no ranking.** The
attestation is an unordered set of categories. There is deliberately no way to
render "more human than" in the UI, and "assisted" is not styled as a warning.

**3. Receipts are a behavioural log.**
A complete record of every AI tool a person used and when is sensitive on its
own — subpoenable, breachable, saleable, and revealing of working hours, output
rate and creative dry spells.

*How the design reduces it:* the passport stores grants and hashes, never work;
receipts are creator-held with local-first storage; and there is no aggregate
analytics surface over a creator's receipt history, for anyone, including the
creator's clients. Sessions are recorded at hour granularity, not millisecond.

**4. False attestation — and the limit of what this proves.**
A creator can attest "ideation only" when they generated the whole thing. **The
receipt does not prevent this and we should not claim it does.**

What is cryptographically checkable is narrower and worth stating exactly:
*which passport slices which app read, under which grant, at what time, against
this output hash.* The human/AI split is a **signed declaration**, not a
measurement — closer in kind to a tax filing than to a proof. Its value is that
it is on the record, attributable, timestamped, and revocable, so a false one
is a durable liability rather than a deniable sentence in an email. Any entry
in this space claiming to *verify* human authorship is overclaiming, and a
creator who relies on that claim will eventually be badly let down by it.

**5. Style-lock and self-surveillance.**
A passport that carries "your taste" pins you to the work you have already
done, and can quietly make creative change harder — both because tools keep
serving you your own past, and because a visible history of your style makes
departing from it legible to clients.

*How the design reduces it:* Style and Taste slices expire by default and are
opt-in per app rather than globally on; and the creator can hold multiple
disjoint style slices, so exploratory work is not forced through the same
identity as commissioned work.

**6. Deanonymisation through the badge.**
A badge links a piece of work to a persistent public handle. For creators
working pseudonymously — which many do for safety reasons, not preference —
that link is a real hazard.

*How the design reduces it:* badges resolve to a per-work pseudonymous
identifier by default, which proves *a single passport holder* made this and
made that, without resolving to the handle page. Linking to the public handle
is an explicit, separate, reversible choice.

**7. Uneven enforceability.**
Revocation of *future* reads is enforceable. Deletion of context a tool already
copied is a contractual promise, not a technical guarantee.

*How the design reduces it:* say so plainly in the consent UI at the moment of
granting, rather than in terms of service. The design makes non-compliance
*visible* — receipts against a revoked grant surface in the ledger — which is
the honest ceiling for what any consent layer can do to a counterparty that has
already received the bytes.

---

## The first version that should exist

Deliberately small. Three screens and one connector.

1. **Grant** — the creator connects one AI tool, sees exactly which slices it
   is requesting, sets the three purpose flags and an expiry, and signs.
2. **Receipt** — after a work session, a receipt arrives. The creator attests
   the categories of assistance and picks a disclosure level. A badge is
   produced, or isn't.
3. **Revoke** — one control. Future reads refused, past receipts stamped
   `consent_withdrawn`, published badge reverted to L0, all visible in a
   ledger.

**What is *not* in v1, on purpose:** no marketplace integrations, no scores, no
directory, no team accounts, no automatic detection of AI use. Every one of
those either creates the coercion risk in section 1 or requires claiming a
verification power the system does not have.

**Why this must travel across tools rather than live in one.** A per-tool
consent setting is worth almost nothing to the person it is meant to protect,
because the creative process spans tools — reference in one, generation in a
second, edit in a third, delivery in a fourth — and the disclosure obligation
attaches to the finished piece, not to any single step. Consent that stops at
an app boundary breaks precisely where the actual requirement begins. That is
also the case for putting this in the passport rather than anywhere else: it is
already the only object in a creator's workflow that is *supposed* to cross
tool boundaries.

---

## Prototype

Interactive walkthrough of the three screens — grant, receipt with live
disclosure-level switching, and revocation with the resulting audit ledger.

*(link inserted at submission)*

---

## Prior work by this team

Two shipped projects sit directly under this proposal, which is the reason to
believe this particular first version is buildable as described:

- **ProofPrint** — a chain-of-custody tool for AI-generated media, built on
  Backblaze B2 Object Lock with an embedded provenance manifest. It is the same
  mechanism as the Work Receipt, applied to an artefact instead of a person.
  The hard-won lesson it produced is the one in risk 4: the moment a provenance
  tool starts implying it verifies *authorship* rather than *custody*, it is
  lying, and the lie eventually costs a user something real.
- **Common Ground** — a tool built for a whole class so that nobody has to
  identify as neurodivergent in order to use it. Its central design constraint
  was minimum sufficient disclosure: the system must not require a person to
  reveal a protected fact in order to receive the benefit. Disclosure levels
  L0–L2 above are that same constraint, in a different domain.
