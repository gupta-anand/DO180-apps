# DO180-apps
DO180 Repository for Payment Router Discussion – Next Steps

Hi all,

Thanks for the engaging and thoughtful discussion earlier today. Below is a quick summary of the key points and the next steps we agreed on.

⸻

Key Points:
	•	Clear Book Transfer should be decoupled from Fedwire logic and treated as a standalone rail.
	•	Global Payment Router will determine the optimal rail (Clear Book, ACH, Fedwire) in a single call and return a synchronous or technical ack based on rail capability.
	•	Fraud and Sanctions should not be on the critical path for predictable flows like LMS or CAD-to-CAD transfers. Risk controls should adapt to context.
	•	Sweeps and Multi-Entity Transfers may still require sanctions, especially across legal entities or currencies.
	•	Sanctions Effectiveness must be ensured at the time of transaction. The idea of pre-scrubbing accounts (vs. scanning every transaction) was raised as a potential efficiency improvement.
	•	We need to avoid a uniform design that burdens all transfers with unnecessary checks and delays.

⸻

Next Steps:
	•	Suresh to coordinate a working session with:
	•	Harish (Compliance)
	•	Sean Lonergan (Fraud)
	•	Liquidity business leads
	•	Architecture/design stakeholders (please include me)
	•	Objective: Align on when fraud and sanction checks are required vs. optional, and confirm business rules for routing and risk evaluation.
	•	Clarify compliance expectations around “sanctions effective at time of transaction” and feasibility of account-level pre-scrubbing.
	•	Confirm where vendor product logic ends and internal service responsibility begins.

Please include me in the upcoming meetings, even as an optional attendee, so I can share input as we finalize the design.




