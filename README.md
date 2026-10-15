# SautiShamba

SautiShamba is a concept for helping agricultural extension officers in Eastern Uganda turn local weather, market and transport information into practical, accessible guidance for farmer groups. The officer—not an AI system—reviews, edits and approves advice before it can be shared.

> **Prototype status:** This repository currently contains a static, browser-based product demo with illustrative sample data. It is not a deployed AI service, and it does not connect to live data providers, messaging channels, an agent framework or an MCP server. The approval button changes only the local demo state; it does not contact farmers.

## Problem statement

Smallholder farmer groups need timely, locally relevant advice about planting, selling crops and arranging transport. Extension officers often have to piece together this information and communicate it in formats that do not work for every farmer. Unverified data, language and accessibility barriers can make a recommendation difficult to act on. Automating delivery without accountable review could compound those risks.

## Solution overview

The proposed officer desk brings group priorities and source-attributed local conditions together with a draft recommendation. An officer can inspect and edit the draft, reject it, or approve it. Only after an explicit human approval could a future delivery system prepare accessible versions for voice, SMS, large print and sign-language interpretation.

The demo illustrates this workflow with fictional groups and sample conditions. Search/filter, group selection, draft editing, rejection, approval state and format previews run in the browser; no message is sent.

See [the architecture and workflow explainer](architecture.html) and the [short demo walkthrough](demo-video.webm).

## Target users

- **Primary:** agricultural extension officers serving farmer groups in Eastern Uganda.
- **Recipients:** smallholder farmers who need understandable, accessible and locally relevant guidance. The interface is designed around officers as the accountable users; it is not a farmer self-service portal.
- **Potential administrators:** cooperatives and agricultural programmes that would configure groups, trusted information sources, languages and approval policies in a production system.

## Architecture

### Current demo

```text
Browser
└── index.html (markup, responsive styles and demo JavaScript)
    ├── Illustrative farmer-group records
    ├── Local search, filters, selection and draft state
    ├── Officer edit / reject / approve interactions
    └── Voice / SMS / large-print / interpreter-brief previews
```

There is no application server, database, external API, authentication service, autonomous agent or messaging integration in this prototype. The demo's local sample records are not persistent or authoritative.

### Intended production boundary

```text
Officer UI → authenticated application API → verified source adapters
                                      ↓
                               advisory agent
                                      ↓ draft + evidence
Officer review / edit / reject / explicit approval
                                      ↓ approved content only
                         accessible format preparation
                                      ↓
                         officer-controlled delivery
```

This is a product direction, not implemented infrastructure. A future server-side design should preserve source provenance and timestamps, distinguish observations from model-generated advice, validate recommendations, and enforce authorization and approval before any delivery connector can run.

## Agent architecture

**No AI agent is implemented or invoked by the current demo.** The “AI draft” label represents the intended product workflow, not a claim that generated advice is being produced.

A future advisory agent should be a bounded drafting component, not an autonomous actor with permission to contact farmers. It could receive an officer-selected group and verified context, return a draft with cited evidence, uncertainty and freshness indicators, and ask for officer review. Tool permissions and server-side authorization should prevent it from approving its own output or invoking delivery. Model output should be treated as untrusted until reviewed.

## MCP implementation

**MCP is not implemented in this repository.** There is no MCP client, server, tool registration, MCP configuration or MCP-backed data in the browser demo. The UI's sample values are static records.

MCP could be evaluated later as a standard boundary between a backend advisory agent and approved data or service providers. It should not be described as a live integration until a server is configured, authenticated, permission-scoped and exercised against real data with explicit error handling.

## MCP tools/servers

**Currently configured: none.** No MCP tools or servers are included or connected. Weather, market prices and transport costs displayed in the demo are labeled illustrative samples; their source labels are explanatory mock attribution, not links to operational providers.

Possible future, separately reviewed integrations include read-only weather forecasts, timestamped market-price observations, and officer-confirmed transport estimates. A delivery service would be a distinct, tightly permissioned integration and must require an approved recommendation. Provider selection, access, data licensing, freshness, reliability and safeguarding need to be decided before implementation.

## Human-in-the-loop workflow

1. The officer selects a farmer group and reviews its priorities and attributed conditions.
2. A recommendation is presented as a draft, with the underlying values identified as sample data in this prototype.
3. The officer can edit or reject the draft. Rejection does not send anything.
4. The officer can explicitly approve the displayed draft. In this prototype, approval only changes its local visual status.
5. The four format tabs preview voice, SMS, large print and a sign-language interpreter brief. They are previews only, not deliveries.

In a future live service, the application backend—not the model or client alone—must enforce that only officer-approved, current content can reach a delivery channel. Approval should bind to the exact content and evidence reviewed, and any later edit should require a new approval.

## Technology stack

- **Current prototype:** semantic HTML, CSS and vanilla JavaScript in a single `index.html`; no package manager, framework, build step or server-side runtime is required.
- **Voice preview:** browser Web Speech API when available. Playback uses the current recommendation text and the browser's selected voice; it is not a recorded or validated Lumasaba translation.
- **Demo recording:** `demo-video.webm`, a short silent screen-style walkthrough.
- **Future production stack:** not selected. Authentication, persistence, model provider, server framework, data providers, MCP client/server and messaging vendors remain design decisions.

## Setup / installation

There are no dependencies to install for the static demo.

1. Clone or download this repository.
2. Open `index.html` in a modern browser.
3. To serve it locally instead, run `python -m http.server 8000` from the repository root and visit `http://localhost:8000`. Any simple static-file server also works.

For a shareable static deployment, publish the repository root using GitHub Pages or another static host. Do not treat the sample data as live information.

## Usage

1. Open the dashboard and select a farmer group; search or filter the group list.
2. Review the displayed weather, market and transport cards together with their sample-data labels.
3. Use **Edit draft** to modify the suggested text, **Reject** to reject it, or **Approve draft** to mark it approved in the demo.
4. Select Voice, SMS, Large print or Sign brief to inspect each illustrative format preview. Voice playback, if supported, is a browser-generated English speech sample.
5. Observe the “not sent” and demo-data notices. None of these actions sends information outside the page.

## Limitations

- All farmer groups, priorities, forecasts, crop prices and transport costs are fictional illustrative sample data, not live or verified advice.
- There is no backend, persistence, login, role-based access control, audit log, agent, MCP server/tool, live source integration or messaging channel.
- Approval state is local UI state and is not durable or legally/accountably recorded.
- Format views are mock previews, not complete accessible communication assets. In particular, there is no Lumasaba audio recording or translation, and the sign-language item is an interpreter outline rather than a translation or video.
- Voice playback depends on browser support and available system voices; pronunciation and translation quality are not guaranteed.
- The interface is a demo, not a substitute for local agronomic expertise, current forecasts, market verification or officer judgment.

## Future improvements

- Validate the workflow and languages with extension officers, farmer groups and accessibility practitioners in Eastern Uganda.
- Integrate reliable, permissioned local weather and market feeds with timestamps, provenance, freshness checks and visible failure states; establish a responsible process for transport estimates.
- Build an authenticated backend with group management, durable drafts, versioned approvals and an auditable record of the evidence and exact content approved.
- Evaluate a grounded advisory agent that returns cited sources and uncertainty, and cannot approve or deliver its own recommendations.
- If useful, add a least-privilege MCP client and explicitly reviewed provider servers/tools. Document server ownership, scopes, credentials, availability and data policies before calling them integrations.
- Develop and validate local-language voice, genuinely accessible SMS and large-print materials, and sign-language content with qualified local partners.
- Add delivery connectors only with explicit officer confirmation, recipient safeguards, consent, delivery reporting and a reliable way to stop or correct messages.
- Add automated tests, security review, operational monitoring and a production deployment pipeline before any real farmer data or advice is used.
