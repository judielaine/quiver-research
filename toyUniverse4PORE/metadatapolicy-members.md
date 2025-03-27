# Blue Sky Federation member requirements

Blue Sky collects the following information from all federation members, that is, the organizational representatives:

* organization_name#x-legal - the legal name
* organization_name - a default display name
* [tech|admin|support|security] contacts for default use
* homepage_uri, logo_uri, and policy_uri

In the metadata policies it issues for all entities registered by a member, Blue sky uses the `value` operator to insert the registered value for  `organization_name#x-legal` and to append the registered security contact to the `contacts` array using the `name-addr` format with the `display-name` "Registered security contact"

## Entity specific policies for OPs

Blue Sky wants to align with SAML2Int's metadata requirements:

> [SDP-MD09] Metadata MUST include an <mdui:UIInfo> element as defined in [MetaUI] containing at least the child elements <mdui:DisplayName> and <mdui:Logo>. An SP’s metadata MUST include the child element <PrivacyStatementURL>
>
> [SDP-MD10] The content of the <mdui:Logo> element MUST be either an https URL or an in-line image embedded in a data URI element. The size of the data URI used in a <mdui:Logo> element is not limited to 256 characters.
>
> _Specific details around logo formats including image size, encoding and aspect ratio should be coordinated with the common practice of the entity’s community of SAML peers._
>
> [SDP-MD11] Metadata MUST include an <md:ContactPerson> element within the <md:EntityDescriptor> element, with a contactType of technical and an <md:EmailAddress> element.

In this universe, the R&E community registered a new JWT claim with `IANA JSON Web Token Claims Registry`, `shibmd_scope`.

So Blue Sky has policy that for all OpenID Connect OpenID Providers the metadata policies

* use the `default` operator to provide the registered `logo_uri` if none was present,
* use  `superset_of` to list all the registered `contacts` in  `name-addr` format with the `display-name`  string identifying the purpose of the contact (this preserves any other contacts the institution may have added in the Entity Statement),
* use the `value` operator to provide the registered `shibmd_scope`,
* and use the `value` operator to provide the registered `policy_uri`.

 and the federation enforces the value the  federation vetted with the `value` operator

## Entity specific policies for Intermediaries

If the organization is registering an intermediary entity, Blue Sky requires that the intermediary provide the base of the entity names that it will use. This entity name constraint can be included in the `constraints` part of the Subordinate Statement to limit the entity IDs the intermediary can use.
