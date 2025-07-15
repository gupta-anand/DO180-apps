# DO180-apps
DO180 Repository for Sample Applications

Here’s a clean table format you can drop into a slide, doc, or wiki page. It lays out each validation category, what it checks for, and an example to ground it.

⸻

Validation Categories in FED Wire and ACH Payment Processing

#	Validation Category	What it Checks	Example
1	Field Validation	Format, length, and data type of individual fields	BIC is 8 or 11 alphanumeric characters
2	Semantic Validation	Logical correctness of values	BIC exists and is SWIFT-registered
3	Rules Validation	Business-specific rules	Debit account is active and has sufficient balance
4	Schema Validation	Conformance to XML/flat file schemas	pain.001 structure is followed
5	Authorization Validation	Sender’s permission to initiate transactions	Only authorized users can initiate wire over $500K
6	Sanctions & Compliance Validation	Screening against regulatory lists	Name doesn’t appear on OFAC list
7	Limit Validation	Transaction amount vs. configured thresholds	ACH batch under $100K daily cap
8	Cutoff Time Validation	Timing rules for processing windows	Wires submitted after 5 PM get next-day processing
9	Data Enrichment Validation	Accuracy of system-derived data	Routing number maps to correct bank name
10	Channel-Specific Validation	Constraints based on origination method	Real-time API doesn’t allow future-dated wires


⸻

Let me know if you want a PowerPoint slide or an Excel export of this.
