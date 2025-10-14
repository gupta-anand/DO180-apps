

Key Takeaways for SPS Integration with Global Payment RouterPurpose of Integration:The integration aims to connect RBC Clear's Liquidity Management System (LMS) with the global single payment router to streamline payment processing across various rails (Clear, Fedwire, book transfers).
SPS acts as a proxy to the DDA system, handling account posting (intraday and EOD) without payment orchestration.

Global Single Payment Router:Routes payment instructions from various channels (e.g., LMS for high-value, low-value, ACH, or book transfers) to the appropriate payment engine.
Primarily a routing capability, not responsible for decision-making or orchestration.
Does not currently route Interac or Canadian RTR transactions.

SPS Functionality:SPS focuses on account posting (debit/credit entries for Canadian client DDA accounts) and acts as an adapter to core banking.
Handles soft postings intraday and hard postings at EOD, providing finality responses but not orchestrating payments.
Does not emit events to PODS; relies on IMM for interactions with PODS for RBC Canada clients.

Payment Processing Design:A generic interface allows dynamic routing based on cost or speed, but payment type-specific endpoints are preferred to handle unique behaviors (e.g., E-transfer registration in Canada).
Channels must be aware of payment type differences to avoid a single point of failure in the router.

PODS Integration:PODS is not part of the initial transaction flow to avoid high availability requirements.
Payment status updates are handled asynchronously via PODS, with corrective actions taken later.

Liquidity Management and BFS:BFS supports sweeping and account posting but assumes orchestration, compliance, and validation are handled externally.
Proper payment orchestration is required before payments hit SPS to ensure compliance and fraud risk management.

Payment Certainty and Finality:SPS provides finality responses (completion, errors, or timeouts), requiring channels to interpret results (e.g., insufficient balance).
Options for handling failures include voiding, reversing, or retrying, with server-side settings to prevent duplication.

Onboarding and Configuration:Duplication checks are part of onboarding, with consumers providing an FPS application ID.
New business cases or clients require config changes for DDA transaction codes and detailed onboarding discussions.

Action ItemsSchedule Follow-Up Discussion:Arrange a drill-down session to further clarify the global router’s role and SPS integration requirements.
Owner: Meeting organizer/speaker.
Timeline: Within 1-2 weeks (by October 28, 2025).

Share SPS Architecture Documentation:Distribute the SPS architecture diagram and relevant current-state documentation to stakeholders for review.
Owner: Speaker.
Timeline: By October 21, 2025.

Conduct RAM Review:Initiate a RAM review to discuss the global router’s capabilities and ensure alignment with SPS and LMS requirements.
Owner: RAM team lead.
Timeline: Schedule by November 4, 2025.

Define LMS Requirements:Gather concrete requirements from the LMS Canada team to clarify whether they need payment orchestration or only account posting.
Owner: LMS team.
Timeline: By November 11, 2025.

Discuss Low-Value and RTR RTP Payments:Engage Livio and the RAM team to finalize plans for routing low-value and RTR RTP payments through the global router.
Owner: Project lead.
Timeline: By November 18, 2025.

Onboarding Process for New Clients:Develop a detailed onboarding process for new consumers or business cases, covering reconciliation, settlement, and DDA transaction code configurations.
Owner: Task experience team/DBA.
Timeline: By December 2, 2025.

Enhance PODS Integration Strategy:Define how PODS will handle asynchronous payment status updates and ensure it is not part of the critical transaction flow.
Owner: PODS integration team.
Timeline: By December 9, 2025.

Review Duplication Check Process:Validate the duplication check process during onboarding, ensuring the FPS application ID is correctly implemented.
Owner: Onboarding team.
Timeline: By November 25, 2025.

Assess Business and Client Experience:Conduct a review of the business and client experience to evaluate reconciliation impacts and ensure alignment with integration goals.
Owner: Business analyst team.
Timeline: By December 16, 2025.

Test Failure Handling Mechanisms:Test voiding, reversing, and retry mechanisms to ensure payment certainty and proper error handling (e.g., closed accounts, timeouts).
Owner: QA team.
Timeline: By December 23, 2025.

