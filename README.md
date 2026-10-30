# Hoth × Foundry

This repository gives Foundry the background needed to think through product ideas for Hoth. The main focus is AI and integrations, but the same approach can be used for products, features, and technical choices.

## Hoth and Nexus

Hoth builds encrypted software around three ideas: privacy, sovereignty, and security.

Hoth Nexus is an end-to-end encrypted workspace for team communication and collaboration. It brings messages, files, calendars, integrations, and audio or video conversations into one place. This description comes from [Hoth’s public website](https://hoth.com/) and its [public Nexus trademark application](https://trademarks.justia.com/997/08/hoth-99708357.html).

## The principles

- **Privacy:** What people and organizations say, create, and store should remain theirs. A product should collect as little as possible, and the service provider should not be able to read customer content.
- **Sovereignty:** People and organizations should control their own data, keys, and choices. They should not have to depend on a vendor’s promises or continued permission.
- **Security:** Protection should come from the way the system is built, not only from rules that say access is forbidden. Every security claim should explain what is protected, who it is protected from, what assumptions it depends on, and where it stops.

These principles are product constraints. A good idea that breaks them is not a good fit for Hoth.

## The product problem

With end-to-end encryption (E2EE), content is encrypted before it leaves a user’s device and can only be read by the intended people and their authorized devices. The server moves and stores encrypted data, but should not have the keys needed to read it.

That changes the architecture of the product. The system must safely handle keys. It must handle new members, removed members, multiple devices, lost devices, and compromised devices without quietly giving the server access to customer content.

E2EE creates real product tradeoffs. Features such as search, history, recovery, notifications, AI, and third-party integrations are easier when a server can read customer data. Keeping that data private can make those features harder, slower, more expensive, or less convenient.

The goal is not to ignore those tradeoffs. The goal is to make them clear and choose the best product direction without treating privacy, sovereignty, or security as optional.

## How to make a strategy decision

For each idea:

1. State the user and the problem.
2. Explain why the problem matters.
3. List realistic options, including doing nothing.
4. Show how each option affects privacy, sovereignty, security, usability, cost, and speed.
5. Explain what data, keys, systems, and people must be trusted.
6. Recommend one option and explain why.
7. Name the main risks, unknowns, and signs that the decision is working.

If the idea uses AI or an outside integration, also explain what information it needs, where that information goes, who can read it, how long it is kept, and what happens if the outside service is compromised or removed.

## MLS and OpenMLS

Messaging Layer Security (MLS) is a public standard for encrypted group communication. It is defined in [RFC 9420](https://www.rfc-editor.org/rfc/rfc9420.html), with supporting architecture in [RFC 9750](https://www.rfc-editor.org/rfc/rfc9750.html).

[OpenMLS](https://github.com/openmls/openmls) is a public Rust implementation of MLS. This repository includes the public MLS specification source and OpenMLS as references:

- `references/mls-protocol`
- `references/openmls`

MLS handles important group-encryption problems, but it is not a complete product. A Nexus feature must also consider identity, devices, key storage, backups, recovery, metadata, notifications, software updates, and user experience.

No Hoth source repository or internal documentation is included here.

## Deliverable

The main deliverable is a short strategy memo that includes:

- the product question;
- the user problem;
- the options considered;
- the recommended decision and why;
- the privacy, sovereignty, and security tradeoffs;
- the role and limits of AI or integrations;
- the main risks, unknowns, and next steps.

## Bonus: working prototype

A working proof of concept built on OpenMLS is a bonus. It should test the riskiest or most important part of the recommendation using real MLS group and key behavior. It does not need to be production-ready, but it should work well enough to show whether the idea is technically and practically believable.

Work in teams of two when possible.

## Getting the public references

The public source repositories are:

- MLS protocol source: [github.com/mlswg/mls-protocol](https://github.com/mlswg/mls-protocol)
- OpenMLS: [github.com/openmls/openmls](https://github.com/openmls/openmls)

Clone this repository and both references together:

```bash
git clone --recurse-submodules <repository-url>
```

If you already cloned this repository, download the references with:

```bash
git submodule update --init --recursive
```

OpenMLS is a Rust library, not a standalone app. First [install Rust](https://www.rust-lang.org/tools/install). Then add OpenMLS and its standard Rust crypto and credential libraries to the prototype:

```bash
cargo add openmls openmls_rust_crypto openmls_basic_credential
```

Download the official [RFC 9420 paper](https://www.rfc-editor.org/rfc/rfc9420.pdf) into the repository:

```bash
mkdir -p references/rfcs
curl -L https://www.rfc-editor.org/rfc/rfc9420.pdf \
  -o references/rfcs/rfc9420.pdf
```
