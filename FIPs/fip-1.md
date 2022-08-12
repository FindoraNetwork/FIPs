---
FIP: 1
title: FIP Purpose and Guidelines
status: Living
type: Meta
author: Akhil Peddireddy
created: 2022-08-11
---

## What is an FIP?

FIP stands for Findora Improvement Proposal. An FIP is a design document providing information to the Findora community, or describing a new feature for Findora or its processes or environment. The FIP should provide a concise technical specification of the feature and a rationale for the feature. The FIP author is responsible for building consensus within the community and documenting dissenting opinions.

## FIP Rationale

We intend FIPs to be the primary mechanisms for proposing new features, for collecting community technical input on an issue, and for documenting the design decisions that have gone into Findora. Because the FIPs are maintained as text files in a versioned repository, their revision history is the historical record of the feature proposal.

For Findora implementers, FIPs are a convenient way to track the progress of their implementation. Ideally each implementation maintainer would list the FIPs that they have implemented. This will give end users a convenient way to know the current status of a given implementation or library.

## FIP Types

There are three types of FIP:

- A **Standards Track FIP** describes any change that affects most or all Findora implementations, such as—a change to the network protocol, a change in block or transaction validity rules, proposed application standards/conventions, or any change or addition that affects the interoperability of applications using Findora. Standards Track FIPs consist of three parts—a design document, an implementation, and (if warranted) an update to the [lite paper](https://findora.org/wp-content/uploads/2020/12/Findora_Litepaper_3.2_Final_Clean.pdf). Furthermore, Standards Track FIPs can be broken down into the following categories:
  - **Core**: improvements requiring a consensus fork, as well as changes that are not necessarily consensus critical but may be relevant to [“core dev” discussions]
  - **Interface**: includes improvements around client API/RPC specifications and standards, and also certain language-level standards like method names. The label “interface” aligns with the [interfaces repo] and discussion should primarily occur in that repository before an FIP is submitted to the FIPs repository.
  - **FRC**: application-level standards and conventions, including contract standards such as token standards, name registries, URI schemes, library/package formats, and wallet formats.

- A **Meta FIP** describes a process surrounding Findora or proposes a change to (or an event in) a process. Process FIPs are like Standards Track FIPs but apply to areas other than the Findora protocol itself. They may propose an implementation, but not to Findora's codebase; they often require community consensus; unlike Informational FIPs, they are more than recommendations, and users are typically not free to ignore them. Examples include procedures, guidelines, changes to the decision-making process, and changes to the tools or environment used in Findora development.

- An **Informational FIP** describes an Findora design issue, or provides general guidelines or information to the Findora community, but does not propose a new feature. Informational FIPs do not necessarily represent Findora community consensus or a recommendation, so users and implementers are free to ignore Informational FIPs or follow their advice.

It is highly recommended that a single FIP contain a single key proposal or new idea. The more focused the FIP, the more successful it tends to be. A change to one client doesn't require an FIP; a change that affects multiple clients, or defines a standard for multiple apps to use, does.

An FIP must meet certain minimum criteria. It must be a clear and complete description of the proposed enhancement. The enhancement must represent a net improvement. The proposed implementation, if applicable, must be solid and must not complicate the protocol unduly.

## FIP Work Flow

### Shepherding an FIP

Parties involved in the process are you, the champion or *FIP author*, the [*FIP editors*](#FIP-editors), and the [*Findora Core Developers*](https://github.com/FindoraNetwork/).

Before you begin writing a formal FIP, you should vet your idea. Ask the Findora community first if an idea is original to avoid wasting time on something that will be rejected based on prior research. It is thus recommended to open a GitHub Issue for discussion to do this. 

Once the idea has been vetted, your next responsibility will be to present (by means of an FIP) the idea to the reviewers and all interested parties, invite editors, developers, and the community to give feedback on the aforementioned channels. You should try and gauge whether the interest in your FIP is commensurate with both the work involved in implementing it and how many parties will have to conform to it. For example, the work required for implementing a Core FIP will be much greater than for an FRC and the FIP will need sufficient interest from the Findora client teams. Negative community feedback will be taken into consideration and may prevent your FIP from moving past the Draft stage.

### Core FIPs

For Core FIPs, given that they require client implementations to be considered **Final** (see "FIPs Process" below), you will need to either provide an implementation for clients or convince clients to implement your FIP.  

*In short, your role as the champion is to write the FIP using the style and format described below, shepherd the discussions in the GitHub Issues, and build community consensus around the idea.* 

### FIP Process 

The following is the standardization process for all FIPs in all tracks:

<!-- ![FIP Status Diagram](../assets/FIP-1/FIP-process-update.jpg) -->

**Idea** - An idea that is pre-draft. This is not tracked within the FIP Repository.

**Draft** - The first formally tracked stage of an FIP in development. An FIP is merged by an FIP Editor into the FIP repository when properly formatted.

**Review** - An FIP Author marks an FIP as ready for and requesting Peer Review.

**Last Call** - This is the final review window for an FIP before moving to `Final`. An FIP editor will assign `Last Call` status and set a review end date (`last-call-deadline`), typically 14 days later.

If this period results in necessary normative changes it will revert the FIP to `Review`.

**Final** - This FIP represents the final standard. A Final FIP exists in a state of finality and should only be updated to correct errata and add non-normative clarifications.

**Stagnant** - Any FIP in `Draft` or `Review` or `Last Call` if inactive for a period of 6 months or greater is moved to `Stagnant`. An FIP may be resurrected from this state by Authors or FIP Editors through moving it back to `Draft` or it's earlier status. If not resurrected, a proposal may stay forever in this status. 

>*FIP Authors are notified of any algorithmic change to the status of their FIP*

**Withdrawn** - The FIP Author(s) have withdrawn the proposed FIP. This state has finality and can no longer be resurrected using this FIP number. If the idea is pursued at later date it is considered a new proposal.

**Living** - A special status for FIPs that are designed to be continually updated and not reach a state of finality. This includes most notably FIP-1.

## What belongs in a successful FIP?

Each FIP should have the following parts:

- Preamble - RFC 822 style headers containing metadata about the FIP, including the FIP number, a short descriptive title (limited to a maximum of 44 characters), a description (limited to a maximum of 140 characters), and the author details. Irrespective of the category, the title and description should not include FIP number. See [below](./FIP-1.md#FIP-header-preamble) for details.
- Abstract - Abstract is a multi-sentence (short paragraph) technical summary. This should be a very terse and human-readable version of the specification section. Someone should be able to read only the abstract to get the gist of what this specification does.
- Motivation _(optional)_ - A motivation section is critical for FIPs that want to change the Findora protocol. It should clearly explain why the existing protocol specification is inadequate to address the problem that the FIP solves. This section may be omitted if the motivation is evident.
- Specification - The technical specification should describe the syntax and semantics of any new feature. The specification should be detailed enough to allow competing, interoperable implementations for any of the current Findora platforms.
- Rationale - The rationale fleshes out the specification by describing what motivated the design and why particular design decisions were made. It should describe alternate designs that were considered and related work, e.g. how the feature is supported in other languages. The rationale should discuss important objections or concerns raised during discussion around the FIP.
- Backwards Compatibility _(optional)_ - All FIPs that introduce backwards incompatibilities must include a section describing these incompatibilities and their consequences. The FIP must explain how the author proposes to deal with these incompatibilities. This section may be omitted if the proposal does not introduce any backwards incompatibilities, but this section must be included if backward incompatibilities exist.
- Test Cases _(optional)_ - Test cases for an implementation are mandatory for FIPs that are affecting consensus changes. Tests should either be inlined in the FIP as data (such as input/expected output pairs, or included in `../assets/FIP-###/<filename>`. This section may be omitted for non-Core proposals.
- Reference Implementation _(optional)_ - An optional section that contains a reference/example implementation that people can use to assist in understanding or implementing this specification. This section may be omitted for all FIPs.
- Security Considerations _(optional)_ - The FIPs might have a section that discusses the security implications/considerations relevant to the proposed change. Include information that might be important for security discussions, surfaces risks and can be used throughout the life-cycle of the proposal. E.g. include security-relevant design decisions, concerns, important discussions, implementation-specific guidance and pitfalls, an outline of threats and risks and how they are being addressed.
- Copyright Waiver - All FIPs must be in the public domain. The copyright waiver MUST link to the license file and use the following wording: `Copyright and related rights waived via [CC0](../LICENSE).`

## FIP Formats and Templates

FIPs should be written in [markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) format. There is a [template](https://github.com/Findora/FIPs/blob/master/FIP-template.md) to follow.

## FIP Header Preamble

Each FIP must begin with an [RFC 822](https://www.ietf.org/rfc/rfc822.txt) style header preamble, preceded and followed by three hyphens (`---`). This header is also termed ["front matter" by Jekyll](https://jekyllrb.com/docs/front-matter/). The headers must appear in the following order.

`FIP`: *FIP number* (this is determined by the FIP editor)

`title`: *The FIP title is a few words, not a complete sentence*

`description`: *Description is one full (short) sentence*

`author`: *The list of the author's or authors' name(s) and/or username(s), or name(s) and email(s). Details are below.*

`discussions-to`: *The url pointing to the official discussion thread*

`status`: *Draft, Review, Last Call, Final, Stagnant, Withdrawn, Living*

`last-call-deadline`: *The date last call period ends on* (Optional field, only needed when status is `Last Call`)

`type`: *One of `Standards Track`, `Meta`, or `Informational`*

`category`: *One of `Core`, `Networking`, `Interface`, or `ERC`* (Optional field, only needed for `Standards Track` FIPs)

`created`: *Date the FIP was created on*

`requires`: *FIP number(s)* (Optional field)

`withdrawal-reason`: *A sentence explaining why the FIP was withdrawn.* (Optional field, only needed when status is `Withdrawn`)

Headers that permit lists must separate elements with commas.

Headers requiring dates will always do so in the format of ISO 8601 (yyyy-mm-dd).

#### `author` header

The `author` header lists the names, email addresses or usernames of the authors/owners of the FIP. Those who prefer anonymity may use a username only, or a first name and a username. The format of the `author` header value must be:

> Random J. User &lt;address@dom.ain&gt;

or

> Random J. User (@username)

if the email address or GitHub username is included, and

> Random J. User

if the email address is not given.

It is not possible to use both an email and a GitHub username at the same time. If important to include both, one could include their name twice, once with the GitHub username, and once with the email.

At least one author must use a GitHub username, in order to get notified on change requests and have the capability to approve or reject them.

#### `discussions-to` header

While an FIP is a draft, a `discussions-to` header will indicate the URL where the FIP is being discussed.

#### `type` header

The `type` header specifies the type of FIP: Standards Track, Meta, or Informational. If the track is Standards please include the subcategory (core, networking, interface, or ERC).

#### `category` header

The `category` header specifies the FIP's category. This is required for standards-track FIPs only.

#### `created` header

The `created` header records the date that the FIP was assigned a number. Both headers should be in yyyy-mm-dd format, e.g. 2001-08-14.

#### `requires` header

FIPs may have a `requires` header, indicating the FIP numbers that this FIP depends on.

## Linking to External Resources

Links to external resources **SHOULD NOT** be included. External resources may disappear, move, or change unexpectedly.

## Linking to other FIPs

References to other FIPs should follow the format `FIP-N` where `N` is the FIP number you are referring to.  Each FIP that is referenced in an FIP **MUST** be accompanied by a relative markdown link the first time it is referenced, and **MAY** be accompanied by a link on subsequent references.  The link **MUST** always be done via relative paths so that the links work in this GitHub repository, forks of this repository, the main FIPs site, mirrors of the main FIP site, etc.  For example, you would link to this FIP with `[FIP-1](./FIP-1.md)`.

## Auxiliary Files

Images, diagrams and auxiliary files should be included in a subdirectory of the `assets` folder for that FIP as follows: `assets/FIP-N` (where **N** is to be replaced with the FIP number). When linking to an image in the FIP, use relative links such as `../assets/FIP-1/image.png`.

## Transferring FIP Ownership

It occasionally becomes necessary to transfer ownership of FIPs to a new champion. In general, we'd like to retain the original author as a co-author of the transferred FIP, but that's really up to the original author. A good reason to transfer ownership is because the original author no longer has the time or interest in updating it or following through with the FIP process, or has fallen off the face of the 'net (i.e. is unreachable or isn't responding to email). A bad reason to transfer ownership is because you don't agree with the direction of the FIP. We try to build consensus around an FIP, but if that's not possible, you can always submit a competing FIP.

If you are interested in assuming ownership of an FIP, send a message asking to take over, addressed to both the original author and the FIP editor. If the original author doesn't respond to the email in a timely manner, the FIP editor will make a unilateral decision (it's not like such decisions can't be reversed :)).

## FIP Editor Responsibilities

For each new FIP that comes in, an editor does the following:

- Read the FIP to check if it is ready: sound and complete. The ideas must make technical sense, even if they don't seem likely to get to final status.
- The title should accurately describe the content.
- Check the FIP for language (spelling, grammar, sentence structure, etc.), markup (GitHub flavored Markdown), code style

If the FIP isn't ready, the editor will send it back to the author for revision, with specific instructions.

Once the FIP is ready for the repository, the FIP editor will:

- Assign an FIP number (generally the PR number, but the decision is with the editors)
- Merge the corresponding [pull request](https://github.com/Findora/FIPs/pulls)
- Send a message back to the FIP author with the next step.

Many FIPs are written and maintained by developers with write access to the Findora codebase. The FIP editors monitor FIP changes, and correct any structure, grammar, spelling, or markup mistakes we see.

The editors don't pass judgment on FIPs. We merely do the administrative & editorial part.

## Style Guide

### Titles

The `title` field in the preamble:

 - Should not include the word "standard" or any variation thereof; and
 - Should not include the FIP's number.

### Descriptions

The `description` field in the preamble:

 - Should not include the word "standard" or any variation thereof; and
 - Should not include the FIP's number.

### FIP numbers

When referring to an FIP by number, it should be written in the hyphenated form `FIP-X` where `X` is the FIP's assigned number.

### RFC 2119

FIPs are encouraged to follow [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt) for terminology and to insert the following at the beginning of the Specification section:

> The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119.

## History

This document was derived heavily from [Ethereum's EIP-1](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1.md), which was derived from [Bitcoin's BIP-0001](https://github.com/bitcoin/bips) written by Amir Taaki which in turn was derived from [Python's PEP-0001](https://www.python.org/dev/peps/). In many places text was simply copied and modified. Although the PEP-0001 text was written by Barry Warsaw, Jeremy Hylton, and David Goodger, they are not responsible for its use in the Findora Improvement Process, and should not be bothered with technical questions specific to Findora or the FIP. Please direct all comments to the FIP editors.

## Copyright

Copyright and related rights waived via [CC0](../LICENSE).