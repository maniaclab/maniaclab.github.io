---
layout: project_page
title: AF MCP Platform
group_title: Agentic Systems
tagline: The USB-C port of the facility
lead: A credential-brokered Model Context Protocol gateway that lets a physicist's AI assistant use the Analysis Facility's data, metadata, batch and notebook services, with the facility holding the credentials and auditing every call. Designed and built by Giordon Stark, running in production at UChicago and deployable by any analysis facility.
facts:
  - value: mcp.af.uchicago.edu
    label: One endpoint for every backend
  - value: About 800 users
    label: UChicago ATLAS Analysis Facility, the reference deployment
  - value: Rucio, AMI, HTCondor, JupyterLab, files
    label: Backends today; ServiceX and PanDA next
  - value: Zero raw credentials
    label: Held by the assistant, ever
links:
  - title: Documentation
    url: https://maniaclab.uchicago.edu/af-mcp-platform/
    note: Architecture, authentication, connecting a client, adding a service, observability
  - title: Connecting a client
    url: https://maniaclab.uchicago.edu/af-mcp-platform/connecting-a-client/
    note: Claude Desktop, Claude Code and other MCP clients, step by step
  - title: Self-service portal
    url: https://mcp-portal.af.uchicago.edu
    note: Link identities, mint tokens, see which tools your account can reach (AF login)
  - title: maniaclab/af-mcp-platform
    url: https://github.com/maniaclab/af-mcp-platform
    note: Broker, portal and Helm chart. MIT licence.
  - title: Ecosystem repositories
    url: https://maniaclab.uchicago.edu/af-mcp-platform/ecosystem/
    note: Backend MCP servers and credential-minting services
people:
  - name: Giordon Stark
    url: /team/
    role: Architect and lead developer, MANIAC Lab
  - name: Henry Schreiner
    url: https://iscinumpy.dev
    role: Contributor (Princeton, IRIS-HEP)
  - name: Brian Bockelman
    url: https://github.com/bbockelm/golang-htcondor
    role: HTCondor MCP server (Morgridge Institute)
  - name: MANIAC Lab AF team
    url: /team/
    role: Operates the reference deployment on the Analysis Facility
figure:
  src: /assets/img/af-mcp-architecture.svg
  alt: "Diagram: assistants and the portal authenticate with the facility's Keycloak and present their own bearer token to the broker, whose identity, authorization, credential and audit subsystems sit between every tool call and the backend MCP servers; credential services mint HTCondor tokens, Kerberos tickets and VOMS proxies on the user's behalf."
  caption: One endpoint, four checks on every call, and credentials that stay on the facility side. Solid arrows carry tool calls; dashed arrows carry credentials minted for the user.
status: Version 0.3.5, October 2026. In production on the UChicago Analysis Facility since mid-2026. Phase 1 acceptance, "one AF login, Rucio tools in Claude", met; ServiceX and PanDA backends in progress.
---

## Why a gateway

An AI assistant is only as useful to a physicist as the systems it can reach. On an analysis facility those systems are Rucio for data, AMI for metadata, HTCondor for batch, JupyterLab for interactive work, and behind them the grid: x509 proxies, ATLAS IAM tokens, Kerberos tickets. The obvious shortcut, handing those credentials to the assistant so it can call each service directly, multiplies the places a user has to trust with their identity and leaves no single record of what the assistant did.

The AF MCP Platform takes the other path. Every backend sits behind one Model Context Protocol endpoint. The assistant logs in once, as the user, to the facility's own identity service. From then on the broker decides what the caller may do, mints the right short-lived credential for each backend on the user's behalf, forwards the call, and writes an audit record. The assistant never sees a proxy, a token for CERN, or a Kerberos ticket. In Giordon's phrase, MCP is the USB-C port to a service; this platform is the facility's single, trusted port.

## How it works

The broker is four subsystems behind one HTTP contract.

- **Identity.** Every caller, whether a browser, Claude Desktop or a script, presents its own bearer token. The broker validates it directly against the facility's Keycloak. A token says who is calling and nothing more.
- **Authorization.** Permissions are an attribute of the person, not the token. On every request the broker re-reads the caller's groups and POSIX identity from the Keycloak directory and maps them to permissions through a declarative policy file. A tool the caller is not entitled to does not appear in their tool list at all.
- **Credential.** For each backend the broker resolves the credential that backend needs: an OAuth token for Rucio, an ATLAS IAM token brokered through a linked CERN account for AMI and PanDA, an HTCondor IDTOKEN, a CERN Kerberos ticket, or an x509/VOMS proxy. Credentials are short-lived, cached per user and per backend, and minted by small single-purpose services. The VOMS service is the only pod that touches a user's grid certificate; the passphrase is used once and never stored, and the proxy is redeemed by the backend itself so it never transits the aggregator.
- **Audit.** Each tool invocation produces one structured record with the identity attached and an outcome of success, denied or error. Prometheus metrics stay aggregate and never per user; the audit log is the per-user source of truth, and a usage store serves each user their own history, including wall time, bytes returned and an estimate of the tokens the result injected into their assistant's context.

Adding the next backend is a configuration change, not a code change: one entry registers the server and the permission it requires, and if it needs its own credential flow, one more entry declares the identity provider. The reference deployment has grown this way from Rucio alone to the catalog below.

## What a physicist does

Point the assistant at the endpoint and nothing else. The first request is refused, the client discovers the broker's OAuth endpoints, a browser window opens on the facility's login page, and the client stores the resulting token itself. Claude Desktop and Claude Code both work this way today.

```json
{
  "mcpServers": {
    "atlas-af": { "url": "https://mcp.af.uchicago.edu/mcp" }
  }
}
```

From there the assistant can look up datasets and replicas, query metadata, submit and monitor batch jobs, start or stop the user's JupyterLab server and read the user's own files on the facility, all as that user and only within what their group membership allows. A client that cannot open a browser, such as a CI job, uses a token minted on the portal's Tokens page instead.

The portal at mcp-portal.af.uchicago.edu is for setup, not daily work. Its four screens show the connection snippet and a dashboard, the catalog of backends and tools the signed-in account can reach, the Identities page for linking a CERN account and the x509 grid certificate, and the Tokens page.

## The reference deployment

The platform runs on the UChicago ATLAS Analysis Facility for its roughly 800 users, with the broker at mcp.af.uchicago.edu and the portal at mcp-portal.af.uchicago.edu, both on the facility's Kubernetes cluster and deployed from the project's Helm chart. The hostnames, realm names and group mappings on this page are that deployment's configuration; nothing about them is built into the software.

<table class="uk-table uk-table-small uk-table-divider">
  <thead><tr><th>Component</th><th>What it does</th><th>Repository</th></tr></thead>
  <tbody>
    <tr><td>af-mcp-platform</td><td>The broker, the portal and the Helm chart</td><td><a href="https://github.com/maniaclab/af-mcp-platform" target="_blank" rel="noopener">maniaclab/af-mcp-platform</a></td></tr>
    <tr><td>rucio-mcp</td><td>ATLAS and ESCAPE distributed data management: datasets, files, replicas</td><td><a href="https://github.com/kratsg/rucio-mcp" target="_blank" rel="noopener">kratsg/rucio-mcp</a></td></tr>
    <tr><td>ami-mcp</td><td>ATLAS Metadata Interface: provenance and physics metadata</td><td><a href="https://github.com/kratsg/ami-mcp" target="_blank" rel="noopener">kratsg/ami-mcp</a></td></tr>
    <tr><td>condor-mcp</td><td>Submit and monitor HTCondor jobs on the facility</td><td><a href="https://github.com/bbockelm/golang-htcondor" target="_blank" rel="noopener">bbockelm/golang-htcondor</a></td></tr>
    <tr><td>af-jupyterlab-mcp</td><td>Create, inspect and delete the user's own JupyterLab servers</td><td><a href="https://github.com/maniaclab/af-jupyterlab-mcp" target="_blank" rel="noopener">maniaclab/af-jupyterlab-mcp</a></td></tr>
    <tr><td>af-filesystem-mcp</td><td>Browse and read the user's own files on the facility's home and data storage</td><td><a href="https://github.com/maniaclab/af-filesystem-mcp" target="_blank" rel="noopener">maniaclab/af-filesystem-mcp</a></td></tr>
    <tr><td>servicex-mcp</td><td>ServiceX columnar data delivery as MCP tools (in development)</td><td><a href="https://github.com/maniaclab/servicex-mcp" target="_blank" rel="noopener">maniaclab/servicex-mcp</a></td></tr>
    <tr><td>voms-token-service</td><td>Mints x509/VOMS proxies; the only pod that touches grid certificates</td><td><a href="https://github.com/maniaclab/voms-token-service" target="_blank" rel="noopener">maniaclab/voms-token-service</a></td></tr>
    <tr><td>condor-token-service</td><td>Issues HTCondor IDTOKENs; the pool signing key never leaves HTCondor</td><td><a href="https://github.com/maniaclab/condor-token-service" target="_blank" rel="noopener">maniaclab/condor-token-service</a></td></tr>
    <tr><td>krb5-token-service</td><td>Mints CERN Kerberos tickets for CERN-authenticated identities</td><td><a href="https://github.com/maniaclab/krb5-token-service" target="_blank" rel="noopener">maniaclab/krb5-token-service</a></td></tr>
    <tr><td>af-credentials</td><td>Library a backend uses to redeem a proxy from the broker at call time</td><td><a href="https://github.com/maniaclab/af-credentials" target="_blank" rel="noopener">maniaclab/af-credentials</a></td></tr>
    <tr><td>servicex-token-service, atlas-search-mcp-bridge</td><td>ServiceX refresh-token redemption; auth translation for ATLAS OpenSearch</td><td><a href="https://github.com/maniaclab" target="_blank" rel="noopener">maniaclab on GitHub</a></td></tr>
  </tbody>
</table>

## For other facilities

The platform is written to be deployed, not copied. A facility installs the Helm chart against its own Keycloak, registers whichever backend MCP servers it runs, and declares the identity providers those backends need. The credential-minting services follow one small, auditable pattern: one endpoint, one credential type, one binary, so a facility can add its own without touching the broker. The broker is in Python on FastAPI and FastMCP; the portal is a Vue application linted for accessibility with WCAG 2.1 AA as the target. Everything is MIT licensed.

## Where it fits

The gateway is the facility-side half of the Lab's agentic work. The <a href="https://github.com/usatlas/marketplace" target="_blank" rel="noopener">USATLAS Marketplace</a> carries the portable know-how, skills and plugins, that tell an assistant how to use a facility; the gateway is where that assistant's calls actually land, under the user's identity and the facility's policy. Elwood, the Lab's agentic analysis framework, reaches the Analysis Facility through it. In the team metaphor Giordon uses for Elwood, the gateway is the stadium entrance: everyone comes in through the same door, shows the same badge, and is on the record.

The work was presented at the Throughput Computing 2026 meeting in June 2026 and is part of the IRIS-HEP agentic-analysis roadmap and the CLARIPHY community effort on agentic analysis systems.
