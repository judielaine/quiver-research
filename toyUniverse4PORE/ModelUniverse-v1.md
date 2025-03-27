# OAuth/OIDC Demonstration Model

## 1. Organizational Structure

### 1.1 Educational Institutions

#### 1.1.1 Eastern University (Self-managed approach)

- Runs its own IdP/OP for authentication
- Runs its own authorization server
- Issues its own verifiable credentials (diplomas/transcripts)
- Operates its own portal and mobile applications
- Manages physical resources (lathe)
- *External dependencies*: Uses Learning R Us LMS and accepts their class badge VCs

#### 1.1.2 Central College (Outsourced approach)

- Contracts with Big Box for IdP services
- Contracts with Learnings R Us for:
  - LMS platform
  - Authorization server
  - Credential issuance (diplomas/transcripts as VCs)
  - Portal services
- No self-hosted technical resources

### 1.2 Service Providers

#### 1.2.1 Learnings R Us

- Provides LMS to both Eastern and Central
- Provides authorization server to Central College
- Issues credentials on behalf of Central College
- Issues class badges (used by Eastern)
- Provides portal service to Central College
- Issues CAN_ACCESS tokens for journal resources

#### 1.2.2 Big Box Cloud Services

- Provides IdP services to Central College
- Provides consumer IdP (IdP of last resort)
- Provides file sharing resources

#### 1.2.3 Craft News and Research

- Publishes multiple journal resources:
  - Journal publishing platform (for editors, reviewers)
  - Woodworking guild journal
  - Basketweaving journal
  - Intro to crafts journal
- Relies on external authorization servers for access control

### 1.3 Virtual Organizations (VOs)

#### 1.3.1 High Energy Basketweaving Research Consortia

- "Bring your own IdP" model:
  - Accepts authentication from any IdP
  - Also accepts Big Box IdP
  - Email-initiated account creation
  - Internal attribute/privilege assignment
- Runs authorization server issuing various scopes (LATHE, BEND, etc.)
- Manages steam bender resource
- Manages wiki resource
- Members may need access to Woodworking Guild's wood depot

#### 1.3.2 Woodworkers Guild

- Mixed account creation model:
  - Bulk membership for Eastern University students
  - Individual paid memberships
  - VC-based privileged memberships
- Runs authorization server
- Issues guild membership credentials
- Verifies educational credentials
- Manages portal and mobile applications
- Operates wood depot resource
- Publishes woodworking journal (via Craft News and Research)

## 2. Organizational Interactions

### 2.1 Eastern University Interactions

Eastern University represents a self-managed approach to identity and access management:

1. **Identity Provider Role**:
   - Authenticates its own students, faculty, and staff
   - Issues ID tokens and access tokens for its own services
   - May accept trusted external identities in specific contexts

2. **Resource Provider Role**:
   - Controls access to its lathe resource
   - Accepts access tokens with LATHE scope
   - Validates tokens issued by its own authorization server

3. **Consumer Role**:
   - Integrates with Craft News journals for academic resources
   - Issues tokens through its authorization server for authorized users
   - Provides bulk registration data to Woodworkers Guild for student memberships
   - Accepts Learning R Us class badge VCs for internal purposes

4. **Credential Issuer Role**:
   - Issues verifiable credentials (diplomas/transcripts)
   - Supports standard credential formats
   - Enables credential verification by third parties

### 2.2 Central College Interactions

Central College demonstrates an outsourced identity and access management approach:

1. **Delegated Identity Management**:
   - Authentication happens via Big Box IdP
   - Authorization decisions delegated to Learning R Us
   - Clear separation between authentication and authorization

2. **Service Consumption**:
   - Central College users access resources through delegated services
   - Learning R Us acts as Central's representative for most services
   - Resource access governed by contractual relationships

3. **Resource Access**:
   - Central's library can access Woodworking Guild journal
   - Subscription validation through proper authorization tokens
   - Tokens issued by Learning R Us's authorization server

### 2.3 Virtual Organization Interactions

#### 2.3.1 High Energy Basketweaving Research Consortia

The HEBRC demonstrates flexible authentication while maintaining control over authorization:

1. **Login Integration**:
   - Accepts logins from any IdP
   - Also accepts Big Box IdP
   - Maps external identities to internal accounts
   - Focuses on authorization rather than authentication

2. **Authorization Control**:
   - Maintains its own group and role management
   - Issues tokens with research-specific scopes
   - Controls access to specialized research equipment (steam bender)
   - Defines authorization policies independently

3. **Cross-VO Collaboration**:
   - Establishes trust with Woodworkers Guild through agreements
   - Enables resource sharing between virtual organizations
   - Facilitates complex research collaborations across organizational boundaries

#### 2.3.2 Woodworkers Guild

The Woodworkers Guild demonstrates both institutional and individual membership models:

1. **Multi-faceted Membership Model**:
   - Institutional memberships for Eastern University students
   - Individual memberships with direct authentication
   - Membership levels based on verified credentials
   - Multiple access patterns based on membership type

2. **Resource Provider Role**:
   - Wood depot access controlled through tokens
   - Different access levels based on membership type
   - Cross-VO authorization for HEBRC researchers
   - Clear authorization requirements

3. **Credential Verification**:
   - Accepts Eastern University diplomas/transcripts as VCs
   - Verifies credentials through standard protocols
   - Issues its own membership credentials
   - Uses credentials as part of access decisions

## 3. Authorization Flows

The demonstration model showcases several authorization patterns:

1. **Institutional Authorization**:
   - Eastern University authorizing its students for Woodworkers Guild access
   - Central College library accessing subscription resources

2. **Cross-Organization Authorization**:
   - HEBRC members accessing Woodworkers Guild resources
   - Researchers collaborating across institutional boundaries

3. **Credential-Based Authorization**:
   - Using verified diplomas for enhanced membership levels
   - Using class badges for specific resource access

## 4. Integration Patterns

The model demonstrates several integration patterns:

1. **Authentication Integration**:
   - Big Box authenticates Central College users
   - HEBRC accepts authentications from multiple sources
   - Trust is established at the authorization layer rather than authentication layer

2. **Authorization Integration**:
   - Eastern University authorization server issues tokens for external resources
   - Learning R Us issues tokens on behalf of Central College
   - Resource providers validate tokens from multiple issuers

3. **Credential Integration**:
   - Verifiable credentials flow between organizations
   - Credentials enhance access rights
   - Standard formats enable interoperability
