# DURF data contracts

Example data contracts for the Dutch Repository Federation (DURF) information chain, written in the
[Open Data Contract Standard (ODCS) v3.1.0](https://bitol-io.github.io/open-data-contract-standard/latest/).
The blog post at <https://durf-project.github.io/durf-data-mesh-architecture/> explains why DURF uses them.

Every contract sits on a **boundary between two parties**: one party's data team delivers a data
product, another party depends on it. Handoffs inside a single party (such as OpenAIRE's PROVIDE, Graph
and Broker) do not get a contract. The arrows are the ones in the
[DURF information chain diagram](https://durf-project.github.io/position-paper/#the-durf-information-chain).

All contracts are **drafts** (`status: draft`, version `0.1.0`). They are best-guess examples, not agreed
contracts. Placeholder URLs end in `.example`.

| Contract | Producer (data product) | Consumer | DURF theme | Arrow in the diagram |
| --- | --- | --- | --- | --- |
| [`durf-cris-publications-v1`](institutions/durf-cris-publications-v1.odcs.yaml) | Each university library (`cris_publications`) | OpenAIRE PROVIDE, and through it everyone downstream | 2 | Repositories → Harvester & validator |
| [`durf-repository-discovery-v1`](institutions/durf-repository-discovery-v1.odcs.yaml) | Each university library (`cris_publications`, second output port) | Search engines and indexes (crawlers) | 5 | Repositories → Other indexes |
| [`durf-publisher-fulltext-v1`](publishing-platforms/durf-publisher-fulltext-v1.odcs.yaml) | Publishing platforms (`publisher_deliveries`) | Each university library | 3 | Publishing platforms → Repositories |
| [`durf-broker-enrichment-events-v1`](openaire/durf-broker-enrichment-events-v1.odcs.yaml) | OpenAIRE (`openaire_graph`) | Each university library | 2 | Enrichment (Broker) → Repositories |
| [`durf-openaire-graph-nl-publications-v1`](openaire/durf-openaire-graph-nl-publications-v1.odcs.yaml) | OpenAIRE (`openaire_graph`) | Netherlands Research Portal, other indexes, EOSC nodes | 5, 6 | Graph → Portal; Graph → Other indexes |
| [`durf-openaire-graph-edepot-feed-v1`](openaire/durf-openaire-graph-edepot-feed-v1.odcs.yaml) | OpenAIRE (`openaire_graph`) | KB e-Depot | 4 | Graph → e-Depot |
| [`durf-edepot-preservation-status-v1`](kb/durf-edepot-preservation-status-v1.odcs.yaml) | KB (`edepot_preservation`) | Each university library, URN:NBN resolver, DURF | 4 | e-Depot → Repositories; e-Depot → Resolver |

## The Dutch application profile

[`durf-cris-publications-v1`](institutions/durf-cris-publications-v1.odcs.yaml) is the contract the rest
depend on. It follows the
[OpenAIRE Guidelines for CRIS Managers 1.2](https://openaire-guidelines-for-cris-managers.readthedocs.io/en/v1.2.0/)
(CERIF-XML over OAI-PMH, `metadataPrefix=oai_cerif_openaire`, set `openaire_cris_publications`). It then
tightens the fields that Dutch consumers downstream rely on. Each such field's description marks the change
with **NL**:

- DOI: mandatory when the publication has one.
- ORCID and affiliation ROR ID for authors: mandatory if applicable.
- COAR access rights: mandatory.
- Embargo end date: an NL addition, so Taverne versions open up on time.
- Licence: mandatory if applicable.
- Full-text location with MIME type: required for every file, whatever its access rights.
- File checksum: recommended, so the e-Depot can verify what it receives.

This contract is meant to become the Dutch application profile, stewarded by the NL Research Information
Content Board through EduStandaard. It would replace the NL-DIDL/MODS requirements.

## Metadata used across all contracts

Every contract carries the same `customProperties`:

| Property | Meaning |
| --- | --- |
| `durfTheme` | The DURF theme(s) the contract serves (1–6, as numbered in the position paper) |
| `diagramArrow` | The arrow(s) in the information chain diagram the contract governs |
| `producerParty` | The party that commits to the contract, as opposed to the system it runs |
| `consumers` / `inputPorts` / `dependsOn` | How the contract links to the others |

## Checking the contracts

```bash
pip install 'datacontract-cli[all]'
for f in contracts/*/*.odcs.yaml; do datacontract lint "$f"; done
```

All seven pass `datacontract lint` against ODCS v3.1.0. Once an endpoint is real, replace its placeholder
server with the actual one. `datacontract test` can then check the structural rules, and the OpenAIRE
PROVIDE validator serves as the test for `durf-cris-publications-v1`.

## Licence

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/), Maurice Vanderfeesten.
