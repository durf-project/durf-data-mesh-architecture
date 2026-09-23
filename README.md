# DURF data mesh architecture

**Data governance for a federated repository ecosystem**

_Using data products and data contracts for Dutch repositories in a data mesh architecture._

Read it at <https://durf-project.github.io/durf-data-mesh-architecture/>.

This is a blog post (and a small set of example contracts) about how every party in the
[DURF information chain](https://durf-project.github.io/position-paper/#the-durf-information-chain) can
deliver its data as a product, according to a data contract, so that Dutch research output becomes
resilient, open and global. That covers:

- the university libraries;
- OpenAIRE;
- the KB;
- publishers.

It borrows two ideas from data mesh architecture: data as a product, and contracts first. DURF builds no
lake of its own. The OpenAIRE Graph plays that role, with bronze, silver and gold layers. It is a companion to the
[DURF position paper](https://durf-project.github.io/position-paper/). It is also a companion to the
earlier post on
[data mesh for Open Research Information](https://surf-ori.github.io/ori-data-mesh-architecture/).

## What's in here

| Path | What it is |
| --- | --- |
| [`index.html`](index.html) | The blog post, a single static page served by GitHub Pages |
| [`contracts/`](contracts/) | Seven draft data contracts in [ODCS v3.1.0](https://bitol-io.github.io/open-data-contract-standard/latest/), one per boundary between parties. See [`contracts/README.md`](contracts/README.md) |
| [`assets/`](assets/) | DURF logo and favicon |

The central contract is
[`durf-cris-publications-v1`](contracts/institutions/durf-cris-publications-v1.odcs.yaml). It is a proposal
for the Dutch application profile of the
[OpenAIRE Guidelines for CRIS Managers 1.2](https://openaire-guidelines-for-cris-managers.readthedocs.io/en/v1.2.0/).

## Checking the contracts

```bash
pip install 'datacontract-cli[all]'
for f in contracts/*/*.odcs.yaml; do datacontract lint "$f"; done
```

## Contributing

Run a repository, a CRIS, the OpenAIRE Graph or the e-Depot, and think a contract gets your part wrong?
Please open an issue or a pull request. That's how contracts are meant to change.

## Licence

[CC BY 4.0](LICENSE), Maurice Vanderfeesten.
