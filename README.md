# GCP Architecture Security Reviewer

A single-page, fully offline tool for sketching a GCP architecture and getting instant, rule-based security findings — no live GCP connection, no LLM calls, no build step.

Build a diagram out of common GCP resources (Compute VMs, GKE clusters, Cloud SQL, Storage buckets, Load Balancers, Cloud Functions), define firewall rules and IAM bindings, and the scanner flags misconfigurations against checks modeled on the CIS GCP benchmark — things like public IPs, open SSH/RDP, primitive Owner/Editor roles, public buckets, disabled SSL enforcement, and non-private GKE clusters.

## Setup

There's nothing to install. The whole app is one HTML file with inline CSS/JS.

1. Clone the repo:
   ```
   git clone https://github.com/Mattmc49/gcp-arch-security.git
   cd gcp-arch-security
   ```
2. Open `index.html` in any modern browser (double-click it, or `open index.html` on macOS).

That's it — no server, no dependencies, no API keys.

## Usage

- **Load Sample** (top right) loads a pre-built architecture with intentional misconfigurations so you can see the scanner in action.
- **Add resources**: click a resource type in the left palette (Compute VM, GKE Cluster, Cloud Function, Storage Bucket, Cloud SQL, Load Balancer) to drop it onto the canvas.
- **Edit a resource**: click a node in the diagram to select it, then edit its name and security-relevant properties in the **Properties** tab on the right. The security scan re-runs automatically on every change.
- **Drag nodes** to reposition them on the canvas.
- **Connect nodes**: click **🔗 Connect Mode**, then click a source node followed by a target node to draw a directional edge (traffic flow). Press `Esc` to cancel.
- **Firewall Rules tab**: add/edit/delete firewall rules (direction, action, source ranges, ports). Rules with broad ingress (e.g. `0.0.0.0/0` on port 22/3389, or all ports) are flagged.
- **IAM tab**: add/edit/delete IAM bindings (member, role, scope, condition). Primitive roles (`roles/owner`, `roles/editor`) and public principals (`allUsers`, `allAuthenticatedUsers`) are flagged.
- **Findings tab**: shows every finding, grouped by severity (High/Medium/Low), with a description and a remediation suggestion. Click a finding's resource link to jump to and highlight that node on the diagram.
- **Export SVG**: downloads the current diagram as an `.svg` file.
- **Delete a resource**: select it, then click **Delete Resource** in the Properties tab. Select a node and press `Delete`/`Backspace` as a shortcut.
- **Clear All**: wipes the entire diagram, firewall rules, and IAM bindings.

Your work is auto-saved to the browser's `localStorage`, so refreshing the page won't lose it.

## Notes

- This is a design-time/offline review tool — it does not connect to a real GCP project or call any external API. All checks run against the architecture you define in the UI.
- Findings are illustrative, not a substitute for a full security audit (e.g. Security Command Center, a CIS benchmark scan, or a manual review).
