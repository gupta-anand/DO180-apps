Purpose:
To challenge the PODS design decision to segregate payment event statuses by Line of Business (LOB) topics (e.g., RBC Clear, RBC Canada) and propose alternative designs that preserve the unified single payment router’s abstraction while addressing geographical data restrictions.Context and Critique:
Sean, we need your input to evaluate the PODS decision to publish payment event statuses to LOB-specific topics, which requires systems like LMS to be aware of the LOB (e.g., RBC Clear or RBC Canada) processing a transaction. This design undermines the core purpose of the unified single payment router, which is to abstract payment engine and LOB details from originating systems. Key concerns include:  Undermines Single Payment Router Abstraction:
The unified router is designed to allow systems like LMS to initiate a transfer (e.g., A to B) without needing to know the underlying engine (e.g., Fedwire, GBT, SPS) or LOB. Requiring LMS to subscribe to LOB-specific topics (e.g., RBC Clear or RBC Canada) for status updates forces it to understand LOB-specific processing, defeating the router’s abstraction and adding integration complexity.
Cross-Border Transaction Complexity:
For global cross-border book transfers (e.g., RBC Clear to RBC Canada), LMS must listen to both RBC Clear and RBC Canada topics to track a single transaction’s status. This is inefficient, requiring LMS to correlate events across multiple topics, increasing the risk of errors and complicating development, especially for systems like LMS that simply want to execute a transfer without LOB awareness.
Geographical Restrictions Can Be Addressed Differently:
I understand the need to segregate data due to geographical restrictions (e.g., Singapore’s GDPR-like requirements to prevent unauthorized access). However, other authorization techniques, such as role-based access control (RBAC) or attribute-based access control (ABAC), could enforce data segregation within a unified topic, preserving the router’s abstraction and simplifying consumer logic.
Inconsistency with Channel-Agnostic Design:
Channels like LMS and RBC Edge should remain agnostic to LOB details. Forcing them to subscribe to LOB-specific topics shifts the burden of understanding payment flows to the consumer, contradicting the router’s goal of simplifying interactions. For instance, RBC Edge should not need to know whether a payment involves RBC Clear to enforce data restrictions.
Legacy Constraints Are Not Insurmountable:
The reliance on LOB-specific topics stems from legacy system constraints, such as missing channel IDs for some RBC Canada wires. However, with the Global Payments Platform (GPP) introducing channel identification, we can explore designs that leverage channel-based segregation or unified schemas to reduce LOB dependency.

Proposed Alternatives:
To maintain the single payment router’s abstraction and address geographical restrictions, we should consider:  Unified Topic with Access Controls:  Publish all payment events to a single PODS topic, using RBAC or ABAC to restrict access based on consumer credentials or attributes (e.g., BIC, region). This allows LMS to subscribe to one topic while ensuring compliance with geographical restrictions (e.g., Singapore wires hidden from unauthorized consumers).  
Example: Include BIC or LOB metadata in event headers for consumer-level filtering without requiring separate topics.

Channel-Based Event Segregation:  Segregate events by originating channel (e.g., LMS, RBC Edge) rather than LOB. Each channel subscribes to its own topic, receiving only events for its initiated transactions. This preserves abstraction, as LMS would not need to know LOB details.  
Challenge: Legacy wires (e.g., inbound Swift) lack channel IDs. Solutions include assigning default channel IDs in GPP or using UETR for transaction correlation.

Enhanced Business View for Status Tracking:  Promote the Business View schema in PODS, which consolidates payment events into a single document per UETR, abstracting raw event details (e.g., Paxo 2, Pan 01). This simplifies status tracking for consumers like LMS, reducing the need to filter LOB-specific events.  
Add optional fields for geographical metadata to the Business View, enabling consumers to filter events without LOB-specific topics.

Kafka Bridging with Protocol-Level Filtering:  For Azure-to-on-prem Kafka bridging (e.g., RBC Clear on Azure to RBC Canada on-prem), implement protocol-level filtering to enforce geographical restrictions within a single topic. This supports a unified topic model while ensuring compliance with regional regulations.

Agenda:  Critique of LOB-Segregated Topic Design (15 mins)  How LOB-specific topics break the single payment router’s abstraction.  
Challenges for cross-border transfers requiring multiple topic subscriptions.

Alternative Design Proposals (20 mins)  Unified topic with RBAC/ABAC for geographical restrictions.  
Channel-based segregation vs. LOB-based segregation.  
Enhancing Business View for simplified status tracking.

Addressing Legacy Constraints (15 mins)  Handling missing channel IDs in legacy RBC Canada wires.  
Leveraging GPP for improved event metadata.

Kafka Bridging and Geo-Restrictions (10 mins)  Azure-to-on-prem Kafka bridging with protocol-level filtering.  
Collaboration with the Enterprise Kafka team.

Next Steps and Action Items (5 mins)  Assign owners to evaluate RBAC/ABAC feasibility.  
Plan discussions with the Enterprise Kafka team on bridging solutions.

Preparation:
Please review the attached PODS status notes and the prior SPS integration notes. Come prepared to discuss:  Feasibility of unified topic with RBAC/ABAC vs. LOB-specific topics.  
Strategies to address missing channel IDs in legacy wires.  
Resource implications for implementing channel-based segregation or enhanced Business View.

Attachments:  PODS Status Notes (October 14, 2025)  
Payment Integration Notes (October 14, 2025)

Looking forward to your insights to refine this design and align with the unified single payment router’s vision!  

