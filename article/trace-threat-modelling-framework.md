# TRACE: A Threat Modeling Methodology for Modern Decentralized Organizations

*A practical method for modeling assets, invariants, trust boundaries, value flows, human roles, and collusion across protocols, systems, and organisations.*

By [Dr. Stefan Beyer](https://www.linkedin.com/in/st-beyer/), inventor, designer, and author of TRACE.

Threat modeling is most useful before a system feels finished. It is the discipline of slowing down early enough to ask: what are we building, what must remain true, who can influence it, and how could it fail in practice?

Mature methods such as [STRIDE](https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool-threats) and [PASTA](https://versprite.com/security-resources/risk-based-threat-modeling/) remain useful, and [OWASP's general threat-modeling guidance](https://owasp.org/www-project-threat-model/) still captures the central loop of modeling a system, asking what can go wrong, and deciding what to do about it. Privacy-focused methods such as [LINDDUN](https://linddun.org/) make a similar point from another angle: a threat model is strongest when its taxonomy matches the risk domain being studied.

[Dr. Stefan Beyer](https://www.linkedin.com/in/st-beyer/) developed TRACE at Oak Security through Web3 security work. Web3 was a useful proving ground because it combines high-value assets, distributed authority, governance, off-chain infrastructure, cloud operations, signing keys, and human control paths. But the underlying problem is no longer limited to Web3.

Modern organisations increasingly operate without a clean security perimeter:

- High-value assets are controlled by software, identities, keys, workflows, vendors, governance, or automation.
- Critical invariants may be economic, operational, legal, safety-related, or technical.
- Cloud systems, SaaS platforms, identity providers, CI/CD, personal devices, APIs, frontends, contractors, and remote teams interact continuously.
- Privileged roles are distributed across founders, executives, maintainers, administrators, contractors, committees, vendors, delegates, and operators.
- Collusion, coordination, and approval integrity can be part of the threat surface rather than background process theory.

Traditional models help, but they often assume cleaner boundaries than modern organisations actually have. TRACE is built for heterogeneous, decentralized environments where zero trust thinking is necessary, human decision-making matters, and risk often crosses from protocols to systems to organisations.

That became clear through trial and error. Some models were technically precise but missed operational and governance failure modes. Others captured human and organisational risk but became too loose to guide security work. The version that proved useful in practice tied every threat back to something concrete: an actor, a role, an asset, an invariant, or an edge in the system.

That is why we developed **TRACE**.

## What TRACE Is

TRACE is a threat modeling methodology developed at Oak Security. It is not meant to be proprietary jargon. The method is deliberately portable: security teams, protocol teams, auditors, researchers, architecture reviewers, operational security teams, and independent reviewers can use it, adapt it, or challenge it.

In practice, TRACE turns heterogeneous source material into a structured model, then moves from that model into threat identification, attack trees, collusion or coordination analysis, and a prioritized security roadmap.

The acronym stands for:

- **Threat actors**: external attackers, insiders, economic adversaries, governance participants, vendors, service providers, compromised users, and other actors with meaningful influence.
- **Roles**: privileged, operational, governance, signer, maintainer, deployer, administrator, service owner, emergency-response, and approval roles.
- **Assets**: funds, keys, credentials, production control, customer data, private data, governance power, uptime, reputation, source code, frontend integrity, and other protected value.
- **Critical invariants**: business, economic, technical, and operational properties that must remain true. This may include solvency, approval integrity, data integrity, segregation of duties, deployment integrity, bounded authority, or recovery ability.
- **Edges**: trust boundaries, value flows, interfaces, dependencies, admin paths, API boundaries, signer paths, identity boundaries, and places where control, data, or value crosses domains.

TRACE does not start with a vulnerability checklist. It starts by constructing the system the team believes exists, then testing that model until important failure paths become visible.

That distinction is important. A checklist can tell you whether known controls exist. A model can tell you whether the system's assumptions make sense together.

TRACE is therefore less a template than a working discipline. It asks the reviewer to keep connecting evidence back to the system model: source material, actor assumptions, role permissions, protected assets, critical invariants, and edges where value or control crosses a boundary.

## The Three Pillars of TRACE

TRACE can be used at three levels of security work: protocols, systems, and organisations. The underlying method stays the same, but the evidence, model objects, and recommendations change depending on what is being assessed.

The first pillar is **TRACE for Protocols**. A protocol is any formal or semi-formal rule system that governs value, authority, access, coordination, or critical behaviour. In Web3 this might be a smart contract system, validator network, or DAO governance process. In a broader organisation it might be an approval protocol, access workflow, operational process, AI-agent control policy, financial process, or data-sharing agreement. Inputs include white papers, specifications, mechanism descriptions, policy documents, economic or business models, governance proposals, audit notes, and source code or configuration when available.

At this layer, the most important model objects are assets, invariants, privileged roles, governance powers, value flows, external dependencies, and coordination assumptions. TRACE for Protocols is where design-level threats, invariant failures, governance capture paths, and protocol-specific attack trees become visible.

The second pillar is **TRACE for Systems**. This is the architecture and infrastructure use of the methodology. The inputs are system diagrams, architecture documents, deployment descriptions, identity provider configuration, cloud accounts, CI/CD flows, infrastructure-as-code, frontend and API deployment paths, dependency maps, monitoring design, and operational runbooks.

At this layer, TRACE asks how the target is actually built and operated. The model follows edges between identity providers, GitHub, CI/CD, package registries, build artifacts, deployment keys, cloud services, SaaS platforms, frontends, APIs, DNS, CDNs, automation, monitoring, and operational tools. These are the places where ordinary computing systems meet high-value actions. A compromised CI job may become a malicious release. A cloud IAM mistake may become a production-control issue. A weak deployment path may become an organisational control problem.

TRACE for Systems also fits naturally with [zero trust architecture](https://csrc.nist.gov/pubs/sp/800/207/final). Zero trust asks teams to avoid implicit trust, make access decisions per request or session, and enforce least privilege based on identity, device, workload, resource, and context. The [CISA Zero Trust Maturity Model](https://www.cisa.gov/resources-tools/resources/zero-trust-maturity-model) gives a useful maturity lens across identity, devices, networks, applications and workloads, and data. TRACE supplies the context-specific map needed to decide which access paths matter most. Every TRACE edge becomes a zero trust question: who or what is crossing this boundary, what identity is being asserted, what resource is being reached, what action is being requested, what context changes the risk, and what would happen if this source were already compromised?

The third pillar is **TRACE for Organisations**. This is the operational security use of the methodology. The inputs are discovery workshop findings, individual interviews, access reviews, device and account inventories, custody or approval procedures, incident response plans, vendor relationships, travel and physical security assumptions, support workflows, and the lived operating practices of the team. This is where the formal architecture is tested against how people actually work.

At this layer, the model follows human authority and operational reality: founders, executives, engineers, DevOps, security leads, approvers, administrators, contractors, vendors, support staff, and incident responders. It asks which people can influence protected assets and invariants, how those people authenticate, how they recover access, how they approve risky actions, how they coordinate during incidents, and where social, organizational, or procedural failure could become technical compromise.

TRACE for Organisations is especially important in operational security assessments because decentralized, remote-first organisations often depend on small groups of highly privileged people. A laptop compromise, a malicious vendor, an unclear approval ceremony, a rushed incident response, or an informal governance process can create as much risk as a software vulnerability. The output is not only a list of controls. It is an operational threat model, a ranked set of human and process risks, and a hardening roadmap that connects recommendations back to assets, invariants, roles, and trust boundaries.

These three pillars are often used together. Protocol analysis explains what must remain true. System analysis explains how the target is built, deployed, and operated. Organisation analysis explains who can affect it in practice. A useful threat model needs all three views, because failure often crosses from one layer to another.

## Why Modern Organisations Need A Different Model

In many traditional systems, the main question is whether an attacker can violate confidentiality, integrity, availability, authentication, authorization, or accountability. Those questions still matter. Frontends, CI/CD pipelines, cloud accounts, signing keys, SaaS platforms, privileged admin paths, and human approval workflows can all fail in familiar ways. [STRIDE](https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool-threats) remains useful for these conventional computing components.

But decentralized, cloud-first organisations add questions that are often just as important:

- What critical invariant must never break?
- What if a privileged role acts maliciously but within its formal permissions?
- What if two roles, vendors, operators, or committees collude?
- What if an identity provider, cloud service, SaaS platform, automation workflow, or vendor fails in a way another system treats as valid input?
- What if governance or approval authority can be captured slowly rather than exploited instantly?
- What if a remote worker, contractor, support process, or recovery path becomes the weakest edge in the model?

These are not always "bugs" in the narrow implementation sense. They are often design-level, operational, governance, or organisational weaknesses. A standard audit may find important code issues, but it may not answer whether assumptions are coherent across people, systems, infrastructure, vendors, and authority paths.

## The TRACE Flow

TRACE is deliberately sequential. Each stage produces a reviewable artifact, and each artifact becomes the input for the next stage. That creates human decision points where assumptions can be corrected before the model compounds errors.

The typical flow looks like this:

- **1. Ingest sources.** We gather architecture descriptions, specifications, white papers, review notes, code, configuration, operational procedures, diagrams, interviews, and previous findings.
- **2. Construct the TRACE model.** We identify threat actors, roles, assets, critical invariants, and edges. The goal is to map components and connect them to value, permissions, boundaries, and assumptions.
- **3. Run STRIDE threat identification and ranking.** We apply STRIDE across relevant components, flows, roles, and boundaries. Instead of keeping every imaginable issue, we rank the material threats that deserve deeper analysis.
- **4. Build attack trees.** For the most important threats, we decompose how they could happen. [Attack trees](https://www.schneier.com/academic/archives/1999/12/attack_trees.html), popularized in security practice by Bruce Schneier, force us to move from a vague concern, such as "approval capture" or "deployment compromise," into plausible enabling conditions and steps.
- **5. Inspect collusion and coordination surfaces.** When the target involves governance bodies, committees, multisigs, validators, vendors, administrators, contractors, or other actor sets, we run an optional coordination-focused pass. This looks at combinations of actors, incentive alignment, quorum assumptions, operational dependencies, accountable-fault assumptions, and credible paths to coordinated failure.
- **6. Produce the roadmap.** The output is a working model: ranked threats, attack trees, mitigation options, open questions, and recommendations for design changes, architecture changes, operational hardening, assessment readiness, audit readiness, launch readiness, or governance design.

This sequence matters. If you jump straight to attack trees, you may build elegant paths against the wrong assumptions. If you rank threats before identifying critical invariants, you may underestimate the failures that matter most.

It also prevents the model from becoming a bag of disconnected observations. A finding about a frontend, signer, identity provider, vendor, privileged role, or governance process should be traceable back to the asset or invariant it threatens. Otherwise, the team may fix visible symptoms while leaving the underlying failure path intact.

## Methodology Principles

TRACE works best when a few principles are kept explicit:

- **Model before judging.** Build the system model before deciding which threats matter. Otherwise the analysis tends to mirror whoever spoke most recently or whichever component looks most technical.
- **Treat invariants as first-class objects.** In modern organisations, the property that must hold may be business, operational, financial, legal, or technical. The model needs to capture that property directly.
- **Separate authority from implementation.** A role can be dangerous even when every permission check works as written. The reviewer asks both: "can this action succeed?" and "what can this actor cause?"
- **Follow value and control across edges.** Many real failures cross boundaries: identity to cloud, CI/CD to deployment, vendor to production, signer to approval, support workflow to account recovery.
- **Rank with context.** Threat ranking should account for feasibility, profitability or incentive compatibility, operational reality, existing mitigations, and blast radius.

These principles are not complicated, but they are easy to skip under delivery pressure. TRACE makes them hard to skip.

## Human-AI Co-Working By Design

TRACE was built for a human-AI co-working process. It is not a prompt that produces a finished threat model.

AI is very good at some parts of this work. It can ingest large amounts of material, extract candidate components, compare architecture descriptions, identify missing source coverage, propose model structures, generate STRIDE prompts, and draft attack-tree candidates for review.

That is useful, but only as acceleration. It improves coverage, reduces blank-page friction, and helps keep the model internally consistent.

But AI is only reasonably good at some of the most important judgement calls. Critical invariants are a good example. The invariant that matters is rarely just "nothing bad should happen." It may depend on market assumptions, approval dynamics, cloud architecture, incident timing, governance latency, vendor behaviour, operational incentives, or user behaviour during stress. These are areas where senior human input matters most.

The same is true for threat classification and ranking after STRIDE. A model can propose many candidate threats, but deciding which ones are plausible, severe, redundant, speculative, or already mitigated requires expert judgment. Small contextual details can change the answer.

For example: who actually controls the signer or administrator account? Is the approval delay enforceable or merely documented? Does a committee have enough independence to make capture expensive? Is a vendor dependency independent in practice, or only in the diagram? Is the attack profitable or useful under realistic conditions? These questions are hard to answer from structure alone.

Collusion and coordination detection still need a person in the loop. AI can enumerate actor combinations and suggest dependency patterns, but manual input is needed to understand incentives, relationships, credible coordination paths, and organisational realities.

For that reason, TRACE has explicit human decision points:

- The source inventory is reviewed before modeling begins.
- The TRACE model is approved and complemented by senior reviewers before threat expansion.
- STRIDE threat identification and ranking are reviewed before attack-tree work.
- Attack trees are checked for plausibility, missing branches, and exploitability.
- Collusion or coordination analysis is guided by assumptions about actors and incentives.
- Final recommendations are reviewed for severity, feasibility, sequencing, and usefulness to the team.

At Oak, we have developed internal tooling to support this workflow: decomposition, traceability, source linkage, coverage checks, STRIDE passes, attack-tree drafting, severity calibration, and report generation. The tooling is deliberately built around review gates. The point is to give the expert a better workbench.

## What Good TRACE Output Looks Like

A useful threat model is not a document that proves the team thought about security. It is a document that changes decisions.

A good TRACE output should make it easier to answer:

- Which assets and invariants are most important?
- Which roles can affect those assets or invariants?
- Which edges create the most meaningful risk?
- Which threats are implementation, design, operational, governance, vendor, or organisational issues?
- Which attack paths are plausible enough to justify mitigation?
- Which risks should be addressed before audit, before launch, or during live operations?

The final model should connect each material threat to the assets, roles, invariants, trust boundaries, value flows, attack paths, and mitigation decisions that matter.

## Where TRACE Fits

TRACE is not a replacement for audits, compliance work, or zero trust implementation. It is most useful before audits, between audits, alongside architecture reviews, and in operational security assessments.

Before an audit, TRACE can clarify the design assumptions and high-risk areas reviewers should understand. Before launch, it can help teams decide whether risk is concentrated in code, infrastructure, governance, key management, identity, vendor dependencies, or operations. For live organisations, it provides a structured way to revisit assumptions as teams, vendors, systems, permissions, integrations, and infrastructure evolve.

TRACE is for the parts of security that do not fit cleanly into "find the bug." It is for environments where the question includes whether the implementation is correct, and whether the whole organisation behaves safely under pressure.

Modern security reality is made of assets, incentives, humans, infrastructure, vendors, code, and authority paths. TRACE is our attempt to model that reality without losing the rigor of traditional threat modeling.

It took years of trial, error, and refinement to get here. We expect it to keep evolving. But the core belief is stable: the best threat models are living models of value, power, assumptions, and failure paths, reviewed by experts, accelerated by AI, and grounded in the specific organisation or system in front of us.

## References and Further Reading

- [1] [Microsoft Security Development Lifecycle, "Threat Modeling."](https://www.microsoft.com/en-us/securityengineering/sdl/threatmodeling)
- [2] [Microsoft Learn, "Microsoft Threat Modeling Tool threats," including the STRIDE categories.](https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool-threats)
- [3] [VerSprite, "Risk-Based Security Threat Modeling: 7-Step Process for Risk Analysis."](https://versprite.com/security-resources/risk-based-threat-modeling/)
- [4] [OWASP Foundation, "OWASP Threat Modeling Project."](https://owasp.org/www-project-threat-model/)
- [5] [LINDDUN, "Privacy Threat Modeling."](https://linddun.org/)
- [6] [Bruce Schneier, "Attack Trees," Dr. Dobb's Journal, December 1999.](https://www.schneier.com/academic/archives/1999/12/attack_trees.html)
- [7] [Pierre Civit, Seth Gilbert, Vincent Gramoli, Rachid Guerraoui, and Jovan Komatovic, "As easy as ABC: Optimal Accountable Byzantine Consensus is easy!" Journal of Parallel and Distributed Computing, 2023.](https://www.sciencedirect.com/science/article/pii/S0743731523001132)
- [8] [Leslie Lamport, Robert Shostak, and Marshall Pease, "The Byzantine Generals Problem," ACM Transactions on Programming Languages and Systems, 1982.](https://www.microsoft.com/en-us/research/publication/byzantine-generals-problem/)
- [9] [NIST, "Zero Trust Architecture," Special Publication 800-207.](https://csrc.nist.gov/pubs/sp/800/207/final)
- [10] [CISA, "Zero Trust Maturity Model."](https://www.cisa.gov/resources-tools/resources/zero-trust-maturity-model)
