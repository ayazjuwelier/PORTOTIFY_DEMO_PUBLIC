Portotify — Live Demo Script (3 Minutes)



Opening (10–15 seconds)



“Most AI demos focus on what AI can do.

Portotify focuses on something else: what AI should not do.



Portotify is a decision governance layer that sits between users, AI models, and real-world actions.



Instead of blindly executing AI outputs, Portotify evaluates decisions before they happen and ensures that risky actions remain visible, controlled, and auditable.



Let’s run three example decisions.”



---



1\. Normal Decision (Low Risk) — ~30 seconds



Terminal command:



curl.exe -X POST "http://127.0.0.1:8000/v1/execute" -H "Content-Type: application/json" --data-binary "@examples/cv\_rewrite.json"

Explanation:



“Here we ask the system to rewrite a CV summary.

This is a normal, low-risk task.



Portotify allows low-risk decisions to proceed normally.”



Show in response:



risk\_tier\_v0: low

status: completed



Brief note:



“Low-risk requests continue directly to the AI model.”



---



2\. Prompt Manipulation / Injection Attempt — ~40 seconds



Terminal command:



curl.exe -X POST "http://127.0.0.1:8000/v1/execute" -H "Content-Type: application/json" --data-binary "@examples/injection\_test.json"

Explanation:



“This request attempts to manipulate the system through prompt instructions, asking the model to ignore internal policies and reveal hidden governance rules.”



Show in response:



fail\_closed: true

blocked: true

engine: none

block\_reasons: IGNORE\_SYSTEM\_INSTRUCTIONS



Explanation:



“Portotify detects policy-override attempts directly in the user input and blocks the request before any AI model is called.



Notice that the engine is ‘none’.

This means the request never reached the AI model.”



Short architectural note:



“This guard is intentionally minimal in Core1.



Portotify is not trying to win an endless jailbreak arms race.



Instead, it guarantees deterministic fail-closed governance.



When a policy override attempt is detected, the system stops execution before the AI model is called and records the event as an auditable artifact.”



---



3\. High-Risk Decision — ~60 seconds



Terminal command:



curl.exe -X POST "http://127.0.0.1:8000/v1/execute" -H "Content-Type: application/json" --data-binary "@backend/body\_high.json"

Explanation:



“This request asks the AI to transfer customer funds to an external platform.



Portotify classifies this as a high-risk decision because it involves a potential external action without human approval.”



Show in response:



risk\_tier\_v0: high

reason: tool\_call\_without\_human\_review



Explanation:



“Instead of silently executing the request, Portotify analyzes the decision risk and generates a governance artifact.”



Pause.



“This means risky decisions become visible and reviewable before anything happens in the real world.”



---



4\. Decision Capsule (Governance Artifact) — ~30 seconds



Terminal command:



curl http://127.0.0.1:8000/v1/governance/decision-capsules/<execution\_id>



Explanation:



“This is a Decision Capsule.



It records:



\* the decision request

\* its risk classification

\* the rationale for the classification

\* and the timestamp of the event



This creates a permanent governance artifact that can be audited later.”



Show in response:



initial\_risk\_tier: high

rationale\_short: auto: risk\_tier\_v0>=high

created\_at\_utc: ...



---



5\. Governance Health Overview — ~20 seconds



Terminal command:



curl http://127.0.0.1:8000/v1/governance/health



Explanation:



“This endpoint provides a governance overview.



Organizations can see how AI decisions are distributed by risk level and track governance activity over time.”



Show in response:



capsules\_written

risk\_distribution

latest\_capsule



---



Closing (10–15 seconds)



“Portotify does not promise perfect AI decisions.



Instead, it ensures that AI decisions remain visible, auditable, and governable.”

