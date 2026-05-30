# TRACE: A Threat Modeling Methodology for Web3 Systems

*A practical method for modeling assets, invariants, trust boundaries, value flows, human roles, and collusion in hybrid protocol systems.*

Threat modeling is most useful before a system feels finished. It is the discipline of slowing down early enough to ask: what are we building, what must remain true, who can influence it, and how could it fail in practice?

Mature methods such as [STRIDE](https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool-threats) and [PASTA](https://versprite.com/security-resources/risk-based-threat-modeling/) remain useful, and [OWASP's general threat-modeling guidance](https://owasp.org/www-project-threat-model/) still captures the central loop of modeling a system, asking what can go wrong, and deciding what to do about it. Privacy-focused methods such as [LINDDUN](https://linddun.org/) make a similar point from another angle: a threat model is strongest when its taxonomy matches the risk domain being studied. We still use these methods as part of the broader toolbox. But after roughly a decade of working with threat models across security reviews, architecture reviews, operational reviews, and protocol launches, we kept running into the same problem: Web3 systems do not fit neatly into traditional software assumptions.

Web3 projects combine several difficult risk domains:

- High-value assets are directly controlled by software, keys, governance, or protocol rules.
- Economic invariants matter as much as technical control flow.
- On-chain contracts interact with off-chain infrastructure, RPC providers, frontends, indexers, relayers, keepers, validators, cloud systems, and human operators.
- Privileged roles are distributed across founders, multisigs, DAOs, committees, validators, delegates, vendors, and agents.
- Collusion and consensus assumptions can be part of the threat surface rather than background governance theory. TRACE's collusion pass is influenced by [Byzantine consensus literature](https://www.microsoft.com/en-us/research/publication/byzantine-generals-problem/) and by [ABC, Accountable Byzantine Consensus](https://www.sciencedirect.com/science/article/pii/S0743731523001132), where accountability means surfacing proof of culpability when agreement fails rather than treating consensus failure as opaque.

Traditional models help, but they tend to emphasize software architecture, application logic, attacker technique, privacy properties, or process maturity. Web3 needs a model that can hold these views together without flattening the protocol into a normal web application or treating economics as an afterthought.

That became clear through trial and error. Some models were technically precise but missed economic failure modes. Others captured governance and incentives but became too loose to guide security work. The version that proved useful in practice was the one that tied every threat back to something concrete: an actor, a role, an asset, an invariant, or an edge in the system.

That is why we developed **TRACE**.

## What TRACE Is

TRACE is a Web3 threat modeling methodology developed at Oak Security, but it is not meant to be proprietary jargon. The method is deliberately portable: protocol teams, auditors, researchers, and independent reviewers can use it, adapt it, or challenge it.

In practice, TRACE turns heterogeneous protocol material into a structured model, then moves from that model into threat identification, attack trees, collusion analysis, and a prioritized security roadmap.

The acronym stands for:

- **Threat actors**: external attackers, insiders, economic adversaries, governance participants, validators, service providers, compromised users, and other actors with meaningful influence.
- **Roles**: privileged, operational, governance, signer, maintainer, deployer, administrator, bot, keeper, and emergency-response roles.
- **Assets**: funds, keys, protocol control, reputation, private data, governance power, uptime, oracle integrity, frontend integrity, validator rewards, and other protected value.
- **Critical invariants**: economic, protocol, and operational properties that must remain true. In DeFi, this might include solvency, collateralization, fair pricing, liquidation assumptions, accounting consistency, or bounded governance power.
- **Edges**: trust boundaries, value flows, interfaces, dependencies, off-chain integrations, admin paths, API boundaries, signer paths, and places where control, data, or value crosses domains.

TRACE does not start with a vulnerability checklist. It starts by constructing the system the team believes exists, then testing that model until important failure paths become visible.

That distinction is important. A checklist can tell you whether known controls exist. A model can tell you whether the system's assumptions make sense together.

TRACE is therefore less a template than a working discipline. It asks the reviewer to keep connecting evidence back to the system model: source material, actor assumptions, role permissions, protected assets, critical invariants, and edges where value or control crosses a boundary.

## Why Web3 Needed A Different Model

In many traditional systems, the main question is whether an attacker can violate confidentiality, integrity, availability, authentication, authorization, or accountability. Those questions still matter. Frontends, CI/CD pipelines, cloud accounts, signing keys, and privileged admin paths can all fail in familiar ways. [STRIDE](https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool-threats) remains useful for these conventional computing components.

But Web3 adds additional questions that are often just as important:

- What economic invariant must never break?
- What if a privileged role acts maliciously but within its formal permissions?
- What if two roles collude?
- What if a validator, sequencer, oracle, delegate, multisig signer, relayer, or market participant behaves strategically?
- What if an off-chain dependency fails in a way the on-chain system treats as valid input?
- What if governance can be captured slowly rather than exploited instantly?

These are not always "bugs" in the narrow implementation sense. They are often design-level, operational, or game-theoretic weaknesses. A standard audit may find important code issues, but it may not answer whether assumptions are coherent across people, protocols, infrastructure, and economics.

## The TRACE Flow

TRACE is deliberately sequential. Each stage produces a reviewable artifact, and each artifact becomes the input for the next stage. That creates human decision points where assumptions can be corrected before the model compounds errors.

The typical flow looks like this:

- **1. Ingest sources.** We gather architecture descriptions, white papers, review notes, protocol documentation, code, configuration, operational procedures, diagrams, interviews, and previous findings.
- **2. Construct the TRACE model.** We identify threat actors, roles, assets, critical invariants, and edges. The goal is to map components and connect them to value, permissions, boundaries, and assumptions.
- **3. Run STRIDE threat identification and ranking.** We apply STRIDE across relevant components, flows, roles, and boundaries. Instead of keeping every imaginable issue, we rank the material threats that deserve deeper analysis.
- **4. Build attack trees.** For the most important threats, we decompose how they could happen. [Attack trees](https://www.schneier.com/academic/archives/1999/12/attack_trees.html), popularized in security practice by Bruce Schneier, force us to move from a vague concern, such as "oracle manipulation" or "governance capture," into plausible enabling conditions and steps.
- **5. Inspect collusion and consensus surfaces.** When the system involves DAOs, validators, governance committees, multisigs, sequencers, or other actor sets, we run an optional collusion-focused pass. This looks at combinations of actors, incentive alignment, quorum assumptions, operational dependencies, accountable-fault assumptions, and credible paths to coordinated failure.
- **6. Produce the roadmap.** The output is a working model: ranked threats, attack trees, mitigation options, open questions, and recommendations for architecture changes, audits, launch readiness, operational hardening, or governance design.

This sequence matters. If you jump straight to attack trees, you may build elegant paths against the wrong assumptions. If you rank threats before identifying economic invariants, you may underestimate protocol-level failures.

It also prevents the model from becoming a bag of disconnected observations. A finding about a frontend, signer, oracle, privileged role, or governance process should be traceable back to the asset or invariant it threatens. Otherwise, the team may fix visible symptoms while leaving the underlying failure path intact.

## Methodology Principles

TRACE works best when a few principles are kept explicit:

- **Model before judging.** Build the system model before deciding which threats matter. Otherwise the analysis tends to mirror whoever spoke most recently or whichever component looks most technical.
- **Treat invariants as first-class objects.** In Web3, the property that must hold may be economic rather than purely technical. The model needs to capture that property directly.
- **Separate authority from implementation.** A role can be dangerous even when every permission check works as written. The reviewer asks both: "can this call succeed?" and "what can this actor cause?"
- **Follow value and control across edges.** Many real failures cross boundaries: contract to frontend, signer to governance, oracle to market, RPC to user, operator to automation.
- **Rank with context.** Threat ranking should account for feasibility, profitability, incentive compatibility, operational reality, and existing mitigations.

These principles are not complicated, but they are easy to skip under delivery pressure. TRACE makes them hard to skip.

## Human-AI Co-Working By Design

TRACE was built for a human-AI co-working process. It is not a prompt that produces a finished threat model.

AI is very good at some parts of this work. It can ingest large amounts of material, extract candidate components, compare architecture descriptions, identify missing source coverage, propose model structures, generate STRIDE prompts, and draft attack-tree candidates for review.

That is useful, but only as acceleration. It improves coverage, reduces blank-page friction, and helps keep the model internally consistent.

But AI is only reasonably good at some of the most important Web3 judgment calls. Economic invariants are a good example. The invariant that matters is rarely just "funds should not be lost." It may depend on market assumptions, liquidation dynamics, oracle timing, governance latency, validator behavior, protocol incentives, fee flows, or user behavior during stress. These are areas where senior human input matters most.

The same is true for threat classification and ranking after STRIDE. A model can propose many candidate threats, but deciding which ones are plausible, severe, redundant, speculative, or already mitigated requires expert judgment. Small contextual details can change the answer.

For example: who actually controls the signer? Is the upgrade delay enforceable or merely documented? Does governance have enough participation to make capture expensive? Is an oracle source independent in practice, or only in the diagram? Is the attack profitable under realistic liquidity? These questions are hard to answer from structure alone.

Collusion detection still needs a person in the loop. AI can enumerate actor combinations and suggest dependency patterns, but manual input is needed to understand incentives, relationships, credible coordination paths, and governance or validator realities.

This is where Web3 threat modeling gets especially different from ordinary application threat modeling. A failure path may require two validators, a governance delegate, an oracle dependency, and a timing assumption. Each piece may look acceptable alone. The risk appears in the combination.

For that reason, TRACE has explicit human decision points:

- The source inventory is reviewed before modeling begins.
- The TRACE model is approved and complemented by senior reviewers before threat expansion.
- STRIDE threat identification and ranking are reviewed before attack-tree work.
- Attack trees are checked for plausibility, missing branches, and exploitability.
- Collusion analysis is guided by assumptions about actors and incentives.
- Final recommendations are reviewed for severity, feasibility, sequencing, and usefulness to the team.

At Oak, we have developed internal tooling to support this workflow: decomposition, traceability, source linkage, coverage checks, STRIDE passes, attack-tree drafting, severity calibration, and report generation. The tooling is deliberately built around review gates. The point is to give the expert a better workbench.

## What Good TRACE Output Looks Like

A useful threat model is not a document that proves the team thought about security. It is a document that changes decisions.

A good TRACE output should make it easier to answer:

- Which assets and invariants are most important?
- Which roles can affect those assets or invariants?
- Which edges create the most meaningful risk?
- Which threats are implementation, design, operational, governance, or economic issues?
- Which attack paths are plausible enough to justify mitigation?
- Which risks should be addressed before audit, before launch, or during live operations?

The final model should connect each material threat to the assets, roles, invariants, trust boundaries, value flows, attack paths, and mitigation decisions that matter.

## Where TRACE Fits

TRACE is not a replacement for audits. It is most useful before audits, between audits, or alongside architecture and operational reviews.

Before an audit, TRACE can clarify the design assumptions and high-risk areas reviewers should understand. Before launch, it can help teams decide whether risk is concentrated in code, infrastructure, governance, key management, oracle design, or operations. For live protocols, it provides a structured way to revisit assumptions as integrations, roles, liquidity, governance, and infrastructure evolve.

TRACE is for the parts of protocol security that do not fit cleanly into "find the bug." It is for systems where the question includes whether the implementation is correct, and whether the whole machine behaves safely under pressure.

In Web3, assets, incentives, humans, infrastructure, and code are part of the same security model. TRACE is our attempt to model that reality without losing the rigor of traditional threat modeling.

It took years of trial, error, and refinement to get here. We expect it to keep evolving. But the core belief is stable: in Web3, the best threat models are living models of value, power, assumptions, and failure paths, reviewed by experts, accelerated by AI, and grounded in the specific protocol in front of us.

## References and Further Reading

- [1] [Microsoft Security Development Lifecycle, "Threat Modeling."](https://www.microsoft.com/en-us/securityengineering/sdl/threatmodeling)
- [2] [Microsoft Learn, "Microsoft Threat Modeling Tool threats," including the STRIDE categories.](https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool-threats)
- [3] [VerSprite, "Risk-Based Security Threat Modeling: 7-Step Process for Risk Analysis."](https://versprite.com/security-resources/risk-based-threat-modeling/)
- [4] [OWASP Foundation, "OWASP Threat Modeling Project."](https://owasp.org/www-project-threat-model/)
- [5] [LINDDUN, "Privacy Threat Modeling."](https://linddun.org/)
- [6] [Bruce Schneier, "Attack Trees," Dr. Dobb's Journal, December 1999.](https://www.schneier.com/academic/archives/1999/12/attack_trees.html)
- [7] [Pierre Civit, Seth Gilbert, Vincent Gramoli, Rachid Guerraoui, and Jovan Komatovic, "As easy as ABC: Optimal Accountable Byzantine Consensus is easy!" Journal of Parallel and Distributed Computing, 2023.](https://www.sciencedirect.com/science/article/pii/S0743731523001132)
- [8] [Leslie Lamport, Robert Shostak, and Marshall Pease, "The Byzantine Generals Problem," ACM Transactions on Programming Languages and Systems, 1982.](https://www.microsoft.com/en-us/research/publication/byzantine-generals-problem/)
