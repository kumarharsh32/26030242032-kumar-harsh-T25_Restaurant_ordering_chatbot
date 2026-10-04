Restaurant Ordering Chatbot
T25 — Chatbots & AI Assistants
1. Project Overview and Objective
This project developed a conversational restaurant ordering assistant for Tambda Rassa Kitchen, a fictional restaurant in Gangapur Road, Nashik. The objective was to build a chatbot that could understand natural-language food orders, handle modifications, check item availability and allergens, calculate the bill with GST, and apply restaurant-specific delivery and discount rules. The supplied project workbook contains the restaurant information, menu data and seven sample customer conversations. The assignment additionally requires ten own tests, hand verification of bills, an AI-use log and a 3–5 page report.
The key learning was that a restaurant chatbot should not depend on a large language model alone for business-critical decisions. During early development, the chatbot could approve requests even when the requested item was unavailable. This showed that a fluent AI response is not the same as a rule-compliant transaction. The solution was redesigned as a hybrid system: AI handles natural-language understanding and conversation, while deterministic logic validates availability, allergens and financial calculations.
The restaurant rules used in the prototype were taken from the workbook: 5% GST on the food bill, a 6 km delivery radius, free delivery above ₹499 and ₹40 otherwise, and a 10% weekday discount for orders above ₹999. Payment methods are UPI, cards and cash on delivery.
1.1 Project Objectives
•	Build an end-to-end conversational ordering experience using a free AI development tool.
•	Use the supplied menu and restaurant information instead of allowing the AI to invent business facts.
•	Handle additions, removals and replacements correctly.
•	Validate unavailable items and allergen information before accepting an order.
•	Calculate subtotal, discount, GST and delivery charges deterministically and verify bills manually.
•	Document AI use, iterations, limitations and recommendations.
1.2 Dataset and Scope
The workbook describes the data as synthetic teaching data. Its detailed dataset description specifies 34 menu rows with nine columns, six Restaurant Info rows and seven Test Conversations. Although the assignment heading refers to a 38-item menu, the implementation follows the actual menu rows supplied in the workbook and does not invent additional items.
2. Approach and Methodology
2.1 Understanding the Business Problem
A restaurant ordering assistant has to solve two problems simultaneously. First, it must understand flexible customer language: customers may combine several items, use natural wording, or change their minds. Second, it must behave like a transaction system: unavailable products cannot be accepted, allergen information must not be guessed, and bills must be mathematically correct. These requirements create a gap between conversational intelligence and transactional accuracy.
The initial chatbot experience exposed this gap. A natural-language model could respond fluently without reliably enforcing the workbook's available_today field. In a real restaurant, this could cause incorrect orders, customer dissatisfaction and potentially serious allergen-related risk. The project therefore treated the spreadsheet as the source of truth and added explicit validation before an item could enter the order.
2.2 Hybrid Architecture
Layer	Role	Example
Customer conversation	Accept natural-language input	“Add two paneer tikka”
AI/NLU	Identify intent, item and quantity	Intent = add; quantity = 2
Business-rule validation	Check structured rules	available_today = Yes
Order state	Maintain changes	Remove mutton; add veg kolhapuri
Billing engine	Calculate totals	Price × quantity; discount; GST
Response layer	Explain result naturally	Accepted/rejected items and total
2.3 Core Business Rules
•	Menu price, food type, spice level, preparation time, allergen note and availability are read from the Menu sheet.
•	Unavailable items are not silently added.
•	Allergen responses use allergen_note rather than general food knowledge.
•	The latest explicit customer instruction determines the final state when an item is replaced.
•	GST is 5% on the applicable food bill.
•	Orders above ₹999 on weekdays qualify for the stated 10% discount.
•	Delivery is limited to a 6 km radius; delivery is free above ₹499 and ₹40 otherwise.
•	When a policy is not specified, the chatbot should say so rather than inventing one.
2.4 Tool Selection
Google AI Studio Build Mode was selected for the user-facing prototype because it allowed the terminal-based logic to become a browser-based conversational interface. ChatGPT was used during development for requirement interpretation, rule design, code generation and debugging. This division was useful: AI accelerated development, while the final build provided a demonstrable chatbot interface.
3. Python Prototype and Google AI Studio Chatbot
3.1 Python Terminal Prototype
Python terminal prototype: The terminal chatbot was important because it made the business rules visible and testable before adding a graphical interface. It allowed me to identify a key weakness in an AI-only approach: the model could sound confident while accepting an unavailable item. The Python rule layer therefore became the reference implementation for availability and billing behaviour. The terminal version and the hosted version should be viewed as complementary: Python provided control and debugging, while AI Studio provided accessibility and presentation.
3.2 Hosted Google AI Studio Chatbot
The hosted build is the user-facing version of the project and is intended for demonstration/screen recording. Google AI Studio supports building web applications from natural-language instructions and provides live previews and deployment options. The project link should be checked for sharing permissions before submission.
4. Testing, Recommendations and Future Improvements
4.1 Testing Strategy
Testing was designed around the seven sample conversations and additional edge cases. The central principle was to compare chatbot decisions with the spreadsheet rather than judging responses only by how natural they sounded. Bills were to be checked manually because a convincing AI response is not evidence of arithmetic correctness.
Test area	Expected behaviour	Verification
Veg, non-spicy starters	Filter category, food type, spice and availability	Compare Menu fields
Mixed order	Accept available items; reject unavailable items	Check available_today
Jain request	Return only explicitly supported Jain options	Do not infer from Veg
Address change	Do not invent policy	Check Restaurant Info
GST total	Use exact prices and 5% GST	Hand calculation
Delivery location	Apply 6 km rule only when distance is known	Ask for exact location if needed
Order replacement	Remove old item and retain replacement	Inspect final order
Allergen query	Use allergen_note	Compare with Menu
4.2 Key Findings
The first major finding was that availability must be enforced outside the language model. For example, the workbook marks Dal Makhani unavailable while Paneer Tikka and Butter Naan are available. A correct assistant should therefore accept the available items and explicitly reject Dal Makhani rather than accepting the entire request.
The second finding concerned order modification. In the sample instruction, the customer first asks for Mutton Rogan Josh and immediately changes the request to Veg Kolhapuri. A robust assistant should treat the latest instruction as the final state. This is an order-state management problem as much as a language problem.
The third finding was that missing information must remain missing. The workbook gives a delivery radius but not the exact distance to every locality, and it does not state a policy for changing an address after ordering. The chatbot should therefore avoid a confident answer when the data is insufficient.
4.3 Verification Status
The current AI Use Log contains documented development iterations as well as planned verification prompts. Several rows are explicitly marked “To test”. These rows should be executed and replaced with the actual response and genuine screenshot before final submission. This is important because the assignment requires honest disclosure of AI use and verification rather than fabricated evidence.
4.4 Accuracy Approach
For every bill, the recommended verification sequence is: confirm each item exists in Menu; multiply unit price by quantity; calculate food subtotal; apply the weekday offer only when both conditions are met; calculate 5% GST according to the agreed order of operations; and add the correct delivery charge. This creates an auditable path from source data to final amount.
4.5 Business Recommendations
The project recommends a hybrid chatbot rather than a fully generative ordering bot. The chatbot can handle customer language and reduce routine workload, but structured business rules should remain deterministic. This is the safest practical architecture because menu availability, prices, discounts, delivery limits and allergen information are operational facts that should not be generated from model memory.
4.6 Recommended Operating Model
Priority	Recommendation	Business value
1	Connect chatbot to one maintained menu source	Prevents outdated prices and availability
2	Keep billing outside the LLM	Reduces financial errors
3	Add explicit allergen confirmation and escalation	Reduces safety risk
4	Maintain an order-state log	Prevents accidental retention of removed items
5	Add human handoff for uncertain cases	Handles exceptions safely
6	Track failed queries and rejected orders	Creates data for improvement
7	Regression-test after rule/menu changes	Prevents repeated errors
4.7 Suggested KPIs
•	Order completion rate: percentage of conversations reaching a confirmed order.
•	Availability accuracy: percentage of availability decisions matching the live menu.
•	Billing accuracy: percentage of bills matching the deterministic calculation.
•	Modification accuracy: percentage of add/remove/replace requests reflected correctly.
•	Escalation rate: percentage of conversations sent to a human because information is missing or sensitive.
•	Average resolution time: time from first customer message to confirmed order or handoff.
4.8 Responsible AI
The project uses synthetic data, which is appropriate for experimentation. In a real restaurant deployment, names, phone numbers, addresses, payment information and allergy information could be sensitive operational data. Such information should not be casually pasted into public AI tools. Secure storage, access control, logging and clear retention policies would be required.
Allergen handling deserves special treatment. The chatbot should communicate only what the maintained allergen data supports and should avoid claiming that a dish is medically safe. When source data is incomplete, escalation to restaurant staff is safer than a confident AI-generated assurance.
4.9 Overall Recommendation
The prototype is suitable as a proof of concept. It should not be deployed as an unsupervised production ordering system until the remaining tests are completed, live data integration is implemented, human escalation is added and billing/allergen rules are independently validated.
5. Reflections and Conclusion
5.1 Personal Reflection
The biggest learning from this project was that building an AI chatbot is not simply a matter of writing a good prompt. My initial assumption was that if the model understood the restaurant menu, it would naturally make correct ordering decisions. The early behaviour showed otherwise. The chatbot could sound confident while still accepting an unavailable item. This changed my approach from “make the AI answer correctly” to “design a system in which the AI cannot easily override the business rules.”
The transition from terminal to hosted application was also an important product lesson. A technically correct command-line program is not automatically a good customer-facing product. The AI Studio interface made the same logic easier to demonstrate, while the Python prototype gave me confidence that the underlying rules could be tested independently.
A second learning was the importance of separating interpretation from calculation. Natural language is flexible, but prices and taxes are not. AI is useful for understanding phrases such as “actually remove it and make it veg,” whereas deterministic logic is more appropriate for multiplying quantities, applying discounts and calculating GST. This distinction is directly relevant to business analytics because it combines customer experience with data integrity.
A third learning was the value of iterative testing. Each problem produced a more specific requirement: availability validation, allergen checking, missing-information handling, order replacement and deterministic billing. The AI Use Log therefore became more than documentation; it represented the evolution of the solution from a generic chatbot into a rule-aware business assistant.
5.2 What Worked Well
•	The workbook provided a structured source for menu and restaurant rules.
•	AI accelerated code generation, debugging and prompt refinement.
•	The hybrid design reduced the risk of hallucinated prices or availability.
•	The architecture is easy to explain because conversation and transaction rules are separated.
•	The project exposed realistic edge cases instead of testing only straightforward orders.
5.3 What Could Be Improved
•	Complete and document all required additional tests before submission.
•	Replace planned AI Use Log rows with actual responses and genuine screenshots.
•	Use a live database or API instead of a static workbook for production.
•	Introduce stronger confirmation for allergen-related requests.
•	Automate regression testing so every menu update triggers validation.
•	Add final order confirmation with a unique order ID and final bill.
5.4 Conclusion
The restaurant ordering chatbot demonstrates that the most effective use of generative AI in transactional business processes is not to let the model control everything. AI is valuable where language understanding and interaction are required, while business rules, calculations and validation should remain deterministic. For Tambda Rassa Kitchen, this approach can improve ordering convenience and reduce repetitive staff effort while maintaining stronger control over correctness.
The project therefore recommends continuing with the hybrid architecture as a controlled proof of concept. The next stage should focus on completing the remaining test evidence, connecting the assistant to a live menu source, adding human escalation and monitoring the proposed KPIs. The main personal takeaway is that reliable AI is created not only through better prompts, but through better system design, verification and responsible handling of uncertainty.
Sources
1. T25 Restaurant Ordering Chatbot project workbook supplied for the assignment: Menu, Restaurant Info, Test Conversations and AI Use Log.
2. Development conversations and code iterations documented in the AI Use Log.
3. Google AI Studio Build Mode prototype created for the project.
Live Application
https://ai.studio/apps/6be90b23-8d9f-4992-80d7-43aae4c174cf 


