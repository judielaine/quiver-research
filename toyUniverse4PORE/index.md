# Toy universe for exploring OpenID Federation

Last modified: 2025/03/27 18:31:40

## Before federation

The toy universe has been populated with an extensive set of organizations to establish a variety of interrelationships to simulate different types of participants in research and scholarship (including virtual organizations), plus different types of plausible future access and attribute relationships. [The initial outline](./ModelUniverse-v1.md) goes into extensive detail.

**For the initial exploration in the universe, the important participants are Eastern University, which has substantial internal technical services, and Central College, which uses cloud providers for the technical services relevant to federation.** These diagrams are for reference and are not necessary for understanding the following.

![Deployment diagram for Central College](../out/toyUniverse4PORE/technical/universe01-deploy-CentralCollege/universe01-deploy-CentralCollege.png)

![Deployment diagram for Eastern University](../out/toyUniverse4PORE/technical/universe01-deploy-EasternU/universe01-deploy-EasternU.png)

## Blue Sky Federation

A toy infrastructure for the Blue Sky Federation is described in the following diagram, in order to depict a plausible infrastructure for managing the federation. It is a **toy** for exploring possibilities.

![Deployment diagram for Blue Sky](../out/toyUniverse4PORE/technical/universe02-deploy-BlueSkyFed/universe02-deploy-BlueSkyFed.png)

## Joining a federation and registering an intermediary

[This text heavy sequence diagram (png)](../out/toyUniverse4PORE/technical/universe02-seq-BootstrapEUfed4rs/) walks through Eastern University joining Blue Sky and registering an intermediary.

In the toy universe, there is an open source application, fed4rs, that acts as an intermediary and fills gaps for applications that do not yet support OpenID Federation. For example, it might host `.well-known/openid-configuration` endpoints for any entities not otherwise prepared to do so, act as a keystore for entities, and so on.

![Deployment diagram for Eastern University and the fed4rs](../out/toyUniverse4PORE/technical/universe02-deploy-EasternU/universe02-deploy-EasternU.png)

The sequence diagram  considers what information is needed in the initial registration and led to the section below discussing [Federation requirements for members](#federation-requirements-for-members)

Then a trivial examination of initial application configuration documents 

* configuring the intermediary to have a URL that will be allowed by the configuration entity name constraints registered with the federation
* and configuring the intermediary to use un-federated authentication against the campus OP.

**Monitoring the trust anchor for key rollover.** Next is the data needed to establish the trust chain. At the time of joining the federation, the University receives a jwks file documenting the federation -- the trust anchor's -- key. The organization could simply trust the TLS connection to the federation's `/.well-known/openid-federation` endpoint, but instead, this participant wants to depend completely on the federation practices. The fed4rs service provides a monitoring service that checks the Entity Configuration for the trust anchor watching for key rollover. To complete monitor and to have the correct `authority hints` in its own Entity Configuration, the application needs the federation entity ID.

In return, the application has its own entity ID and its jwks file to be submitted to the federation.

**Registration of the intermediary** then is simply providing the entity ID and jwks.

In this toy universe it seems that it might be useful for the intermediary to have a copy of the metadata policy that Blue Sky will apply to it, so the federation provides it, and the fed4rs service consumes it, and then generate's its first entity statement.

**Monitoring the subordinate for key rollover.** While the federation may have manual ways for member organizations to provide new keys, the federation should be able to simply monitor the subordinate entity's `/.well-known/openid-federation` endpoint to watch for key rollover.  


## Federation requirements for members

> **Warning** How an immediate superior entity enforces metadata quality appears to allow both assertion of metadata and use of metadata policy. The toy universe might have made poor choices. [More discussion](./metadatapolicy-options.md) is sketched out at the link.

Initial exploration of the toy universe is to support something similar to our SAML federations but with intermediaries allowed between the federation and leaf entities. However, federation asserted responsibility for entities is important and so this initial exploration looks at how the metadata can meet standards currently enforced by federation operators.

In this model, the Blue Sky Federation will use metadata policy to assure that all trust chain evaluations involving Blue Sky end up with metadata that lists includes the legal name of the organization that registered the entity directly or via an intermediary using the attribute  `organization_name#x-legal`. (See RFC5646.) Metadata policy will use the `value` operator to ensure the attribute is present and has the federation specified value.

As intermediaries might want their leaf entities to be able to provide other organization names for display purposes, so the federation metadata policy is silent regarding all names of the form  `organization_name#[language tags]`. However, to ensure interoperability, the metadata policy for OPs and RPs has uses the `default` operator for the untagged `organization_name` so that all OPs and RPs will have a display name.

[More detailed discussion of member level data elements and their use](./metadatapolicy-members.md) is available: the short of it is that Blue Sky Federation can ensure that the metadata of entities registered by intermediaries have certain values by use of the metadata policy, because the metadata policy is expressed at the *entity* level.

The metadata policy that Blue Sky issues as part of its Subordinate Statements about its allowed intermediaries can include:

* constraints on the entity ID names that that subordinate is allowed to issue
* constraints on the entity types that a subordinate is allowed to issue
* restrictions on values for specific metadata parameters per entity type

#### OpenID4RS is silent regarding member data attribute and intermediary capability enforcement with metadata policy

The policy controls allow a great deal of flexibility. OpenID4RS could specify answers to the following questions.

**What member organizational metadata must be federation validated values, for all or specific entity types?**

The toy universe is playing with legal names vs display names and can enforce all entities having the legal name as part of the metadata. What attributes are required for specific entity types? What attribute values should be asserted by the federation (using the `value` operator)?

Note that with intermediaries the enforcement is ... blunt. When the intermediary is registered it can provide "common" values and those can be defaults or overrides. Or perhaps it is sufficient that the intermediary's entity statement has those federation validated values.  

**What entity types must be registered by the federation and what may be registered by intermediaries?**

A federation can control what entity types its intermediaries can issue using the `allowed_entity_types` attribute in the `constraints` claim for intermediaries it registers.  A federation could choose to only allow intermediaries to register RPs and insist that all OIDC OPs are registered directly with the fed.

An interfederation trust anchor would currently have less technical control over enforcing what federations allow intermediaries to do via metadata policy. Presumably, the interfederation's Subordinate Statements about member federations allow OIDC OPs.

If the OIDF spec were changed, the control of `max_path_length` might be moved from the `constraints` claim into the entity type part of the `metadata_policy` claim and then the depth for different type entities could be enforced in the trust chain.

Legal agreement could be sufficient, as could also be separate entity IDs for the federation as direct registrar and the federation as registrar of intermediaries.
