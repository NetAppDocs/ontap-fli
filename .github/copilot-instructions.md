## Copilot instructions for ONTAP Foreign LUN Import documentation

### Repository overview
Product: ONTAP Foreign LUN Import (FLI)

ONTAP Foreign LUN Import (*FLI*) documents SAN block migration workflows that move data from third-party array LUNs to ONTAP LUNs. The content covers migration planning, phased execution, FC and iSCSI backend setup, import operations, and post-migration remediation.

### Repository structure
- `get-started/` – Core background on SAN migration, *FLI* concepts, prerequisites, supported configurations, and migration options.
- `phases/` – End-to-end migration methodology organized by *Discovery*, *Analyze*, *Plan and Prepare*, *Execute*, and *Verify* phases.
- `configure/` – Backend connectivity and transport setup for FC and iSCSI, including initiator-mode and zoning guidance.
- `migrate/` – Task flow for foreign LUN discovery, import relationship creation, import execution/monitoring, and relationship cleanup.
- `post-migration-performance/` – Post-cutover cleanup, performance validation, and host-specific remediation topics.
- `sample-site/` – Sample site survey and planning worksheet reference pages by worksheet tab.
- `release-notes/` – What changed in SAN migration and FLI documentation scope.
- `redirects/` – Legacy URL redirect stubs that map older SAN migration paths to current pages.
- `media/` – Shared diagrams and images referenced by AsciiDoc topics.

### Product-specific context
**Architecture and components:**
- *FLI* is an ONTAP capability where ONTAP acts as a *SCSI initiator* to read from a foreign array LUN and write to a destination ONTAP LUN.
- Migration is *LUN-level only* (SAN/block); file-protocol migrations are out of scope.
- Backend connectivity can be *FC* (initiator ports, zoning, masking) or *iSCSI* (software initiator sessions bound to *intercluster* connectivity), while host-facing frontend access can remain FC or iSCSI.
- Core operational components are foreign array LUNs, ONTAP destination LUNs, SVM context, igroups/mapping, and persistent `lun import` relationships.

**Key concepts:**
- *Foreign LUN discovery* requires deprovisioning/unmapping from hosts first, then presenting the source LUN only to ONTAP initiators to avoid dual presentation.
- A discovered source must be explicitly marked as *foreign* in ONTAP before import setup.
- *Offline FLI* keeps host I/O stopped for the import window; *online FLI* resumes host I/O after cutover while copy continues in the background.
- Import lifecycle includes create/start/show/pause/resume/stop/verify/delete operations on the `lun import` relationship.

**Naming conventions and terminology:**
- *FLI* = *Foreign LUN Import*.
- *Foreign disk* identifiers and serial numbers are used to create correctly sized destination ONTAP LUNs.
- Common ONTAP CLI objects in this repo include `lun`, `lun import`, `storage disk`, `igroup`, and *SVM* parameters.
- *IC LIF* refers to intercluster LIF connectivity used by iSCSI backend sessions.

### Typical user workflows
**Initial FLI setup and planning:** Environment discovery → Supportability and prerequisite validation → Backend transport design (FC or iSCSI) → Host remediation planning → Test migration preparation

**FLI migration execution (offline or online):** Deprovision source LUN from host → Present source to ONTAP initiators and discover foreign disk → Mark foreign LUN and capture serial → Create destination LUN and import relationship → Start and monitor import → Optional verify → Delete import relationship and finalize cutover

**Post-migration operational cleanup:** Validate host access and multipathing → Perform host-specific post-remediation → Remove obsolete zoning/mappings and source presentation → Validate performance and operational state → Complete migration documentation artifacts
