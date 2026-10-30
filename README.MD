PDF Semantic Accessibility Auditor

Step 1: Project Overview
My proposal for a web application is a tool that allows users to upload a single PDF document and receive feedback from AI in the form of an accessibility report focused on semantic structure. It checks whether the document is tagged and evaluates basic elements needed by screen readers (language, title, headings, and other detectable structure).
A large percentage of PDFs are digitally inaccessible, making it a barrier for those who have accessibility needs to consume the document. Without proper tags and structure, screen readers cannot reliably navigate headings, lists, tables, or describe images.

Step 2: Use Case & Workflow
The primary users of this application are developers or compliance teams who remediate or consume PDFs.
Those who are visually impaired would benefit from the final remediated document that this application can assist in producing.

Present workflow: 
	1.	A user has a PDF.
	2.	User manually checks for HTML tags using tools such as Adobe Acrobat. 
	3.	User notes which tags are missing. This process is prone to human error, tedious, time-consuming or deferred.
	4.	The missing tags can then be added.

AI Integrated Workflow: 
	1.	The user uploads the PDF to the web app. 
	2.	The app utilizes an AI model to scan the PDF to identify the tag structure information and/or analyzes if the PDF is semantically tagged.
	3.	The app will then use an LLM to generate a plain-language report with specific recommendations on which tags are missing, how severe the missing tag is and how to resolve the missing tag. This will help in reducing the time required to perform a manual accessibility review, make accessibility review straight forward and minimize human error.

Step 3: AI Features to Be Implemented
	•	Prompt engineering: A carefully designed system prompt will instruct the model to act as a PDF accessibility specialist, only analyze from the extracted data, avoid inventing tags that were not detected, and produce prioritized, practical recommendations. This is essential for consistent output. A framework I plan on utilizing is the PROMPT (Persona, Requirements, Organization, Medium, Purpose, Tone) framework.
	⁃	Persona: A PDF Digital Accessibility Specialist 
	⁃	Requirements: Generate a comprehensive, plain-language PDF remediation report based on a scan of a document's logical tagging structure. The goal is to reduce manual audit times, provide straightforward remediation instructions, and minimize human error for the development team.
	⁃	Organization: The final output will consist of JSON text that describes which section is tagged/untagged as well as the issues within that section and the recommendations on how to address the missing tags.
	⁃	Medium: The final output will return Pydantic schema on the UI.
	⁃	Purpose: The final result will then aid developers and compliance teams in understanding which parts of a document is lacking the proper tags.
	⁃	Tone: The tone of the output will be suggestive, that way consumers of the app can feel more inclined to remediate the document.

	•	Structured outputs: The model will return a formatted report that was previously in Pydantic schema that was passed from JSON containing fields such as PDF name, is_tagged, untagged_field_name and a list of issues (severity, description, recommendation). Structured outputs ensure the report is consistent, easy to display in the UI and consumable. 
	•	Retrieval-Augmented Generation (RAG) and vector databases: The model will retrieve relevant WCAG standards to ensure recommendations are consistent with current regulations.
	•	Evaluation frameworks: The model will be evaluated for correctness in schema ensuring constraints are considered. This will ensure the output is not faulty. 
	•	Observability tools: To observe outcomes, logging will record filename, processing time, input/output, and any errors that appear during processing. This provides visibility into performance and failure patterns during development and early use.

Step 4: Technical Approach
The application will be built utilizing Cursor for rapid development and easy demonstration.
	•	PDF analysis will use pikepdf (a python library used to identify PDF structure / if a document is tagged or not) to detect tagging status, language, title, and available structure tree.
	•	Extracted metadata and structure summary will be passed to the OpenAI API (gpt-4o-mini).
	•	Prompt engineering and OpenAI’s structured outputs (or JSON mode with Pydantic validation) will produce the report.
	•	The node.js will handle the front end, where the file upload is and it will display the formatted report.
	•	Configuration will use the API key from OpenAI.

Step 5: Example Prompts & Expected Outputs
Example Prompt
“You are a PDF digital accessibility remediator. Analyze only the HTML tag semantic structure data provided. Do not invent tags or features that were not detected. Return a structured readable report that prioritizes issues affecting screen-reader users and please provide specific recommendations that includes the type of tag to add and ensure this PDF document adheres to current WCAG standards.”
Example Output:
PDF Title: “Expense Report”
Tagged: False

Issue Identified: Table heading tag is missing
Severity: High
Recommendation: Add <TH> to appropriate table headers.
  
Issue Identified: Paragraph tag is missing on paragraph “This is an example of what the expense report includes…….”
Severity: High
Recommendation: Add <P> to appropriate paragraph.


Step 6: Evaluation Strategy
Effectiveness will be assessed through:
	•	Schema validation success rate (percentage of responses that pass Pydantic validation).
	•	Manual review of reports on a small set of known test PDFs (tagged vs. untagged, with and without language/title).
	•	Qualitative checks that recommendations to ensure the extracted data is what is being analyzed and the model does not hallucinate undetected structure.

Step 7: Observability Plan
Each analysis run will log:
	•	Timestamp and filename
	•	Processing latency
	•	Number of issues identified
	•	Any errors or validation failures
These logs will be written in a structured format so they can be reviewed during development and used to identify common failure patterns or performance bottlenecks over time. In a future iteration, the same data could feed a simple dashboard or external monitoring tool.
