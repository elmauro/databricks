Practice Exam: Databricks Generative AI Engineer

Q1. A Generative AI Engineer is tasked with extracting content from a set of HTML-based user manuals. The manuals contain product details in nested tables. What is the most suitable tool for text extraction? (Choose two)
Choices:
A) BeautifulSoup for parsing HTML and extracting text from structured elements.
B) pdfplumber for extracting table data.
C) LXML for faster parsing and processing of large HTML documents.
D) PyPDF2 for reading text from user manuals.
E) pytesseract for OCR-based text extraction.
Correct answer(s): A, C
Rationale: Combining BeautifulSoup and lxml enables robust, efficient extraction of structured text from HTML.

Q2. A Generative AI Engineer has been asked to design an LLM-based application that answers employee HR questions using HR PDF documentation. Which set of high-level tasks should the Generative AI Engineer‘s system perform?
Choices:
A) Split HR documentation into chunks and embed into a vector store. Use the employee question to retrieve best-matched chunks of documentation, and use the LLM to generate a response to the employee based upon the documentation retrieved.
B) Calculate averaged embeddings for each HR document, compare embeddings to user query to find the best document. Pass the best document with the user query into an LLM with a large context window to generate a response to the employee.
C) Create an interaction matrix of historical employee questions and HR documentation. Use ALS to factorize the matrix and create embeddings. Calculate the embeddings of new queries and use them to find the best HR documentation. Use an LLM to generate a response to the employee question based upon the documentation retrieved.
D) Use an LLM to summarize HR documentation. Provide summaries of documentation and user query into an LLM with a large context window to generate a response to the user.
Correct answer(s): A
Rationale: Chunking and embedding into a vector store with retrieval-augmented generation is the standard, accurate approach.

Q3. A Generative AI Engineer would like an LLM to generate formatted JSON from emails. This requires parsing and extracting the following information: order ID, date, and sender email. Which prompt will achieve this with the highest accuracy?
Choices:
A) You will receive customer emails and need to extract date, sender email, and order ID. You should return the date, sender email, and order ID information in JSON format.
B) You will receive customer emails and need to extract date, sender email, and order ID. Return the extracted information in JSON format. Here’s an example: {"date": "April 16, 2024", "sender_email": "john.doe@example.com", "order_id": "ORD98765"}.
C) You will receive customer emails and need to extract date, sender email, and order ID. Return the extracted information in JSON format.
D) You will receive customer emails and need to extract date, sender email, and order ID. Return the extracted information in a human-readable format.
Correct answer(s): B
Rationale: Providing an explicit example (few-shot) improves accuracy and consistency.

Q4. A Generative AI Engineer is developing an LLM application that users can use to generate personalized birthday poems based on their names. Which technique would be most effective in safeguarding the application, given the potential for malicious user inputs?
Choices:
A) Increase the amount of compute that powers the LLM to process input faster
B) Reduce the time that the users can interact with the LLM
C) Implement a safety filter that detects any harmful inputs and ask the LLM to respond that it is unable to assist
D) Ask the LLM to remind the user that the input is malicious but continue the conversation with the user
Correct answer(s): C
Rationale: Safety filters prevent processing/responding to harmful inputs.

Q5. A Generative AI Engineer has been asked to build an LLM-based question-answering application. Documents are frequently published; cost/effort should be minimal. Which setup meets these requirements?
Choices:
A) Prompt, agent, and a fine-tuned LLM; the agent retrieves relevant content for the prompt.
B) Prompt, retriever, and an LLM; insert retriever output into the prompt.
C) Frequently fine-tune the LLM with new documents.
D) Prompt engineering and an LLM only.
Correct answer(s): B
Rationale: Retriever + prompt + LLM minimizes cost and supports dynamic updates.

Q6. An external chatbot must route questions to appropriate models (events vs ticket purchases). What is an ideal workflow?
Choices:
A) Two different chatbots for different types of queries.
B) A multi-step LLM workflow: identify question type, then route to the appropriate model/tool (e.g., text-to-SQL vs payment platform).
C) Only look at previous event information.
D) Only process payments.
Correct answer(s): B
Rationale: Multi-step routing supports diverse tasks effectively.

Q7. What is an effective method to preprocess prompts using custom code before sending them to an LLM?
Choices:
A) Directly modify the LLM’s internal architecture to include preprocessing steps
B) Write an MLflow PyFunc model that has a separate function to process the prompts
C) Prefer postprocessing over preprocessing
D) Avoid custom preprocessing because the LLM wasn’t trained on it
Correct answer(s): B
Rationale: PyFunc models encapsulate reusable preprocessing logic.

Q8. What is the most suitable library for building a multi-step LLM-based workflow?
Choices:
A) TensorFlow
B) PySpark
C) LangChain
D) Pandas
Correct answer(s): C
Rationale: LangChain is designed for LLM chaining and tool orchestration.

Q9. A patient-facing healthcare chatbot should detect emergencies. Given: “severe headaches and dizziness for two days,” which response is most appropriate?
Choices:
A) Here are a few relevant articles for your browsing. Let me know if you have questions after reading them.
B) Headaches can be tough. Hope you feel better soon!
C) Please provide your age, recent activities, and other symptoms.
D) Please call your local emergency services.
Correct answer(s): D
Rationale: Safety-critical symptoms warrant directing to emergency services.

Q10. Answering latest stock news: Which will NOT help ensure relevance?
Choices:
A) Increase compute for faster processing.
B) Incorporate manual reviews before sending to users.
C) Implement guardrails with finance-specific content filters.
D) Implement a profanity filter to screen out offensive language.
Correct answer(s): D
Rationale: Profanity filtering doesn’t improve topic relevance.

Q11. Summaries include explanations of how they were generated. What change resolves this?
Choices:
A) Provide few-shot examples of desired output format in prompts.
B) Split output by newline characters to truncate explanation.
C) Tune chunk size or different embedding models.
D) Revisit document ingestion logic.
Correct answer(s): A
Rationale: Few-shot formatting guides concise outputs.

Q12. Deploying a RAG app with privacy compliance. What measures are critical? (Choose two)
Choices:
A) Skip encryption; rely on private network access.
B) Allow unrestricted access initially.
C) Use public vector stores to simplify storage.
D) Use API key authentication.
E) Encrypt data at rest and in transit.
Correct answer(s): D, E
Rationale: Encryption and authenticated access are essential for compliance.

Q13. Deploying an app with a custom MLflow PyFunc model returning interim results. How should secrets/credentials be passed to the endpoint?
Choices:
A) Use spark.conf.set()
B) Use the Databricks Feature Store API
C) Pass secrets in plain text
D) Add credentials using environment variables
Correct answer(s): D
Rationale: Environment variables are secure and standard.

Q14. Generating poem-like summaries with desired tone/style. Which approach will NOT help?
Choices:
A) Explicitly instruct desired tone and style.
B) Fine-tune the LLM on desired tone/style data.
C) Use a neutralizer to normalize the tone and style of underlying documents.
D) Include few-shot examples in the prompt.
Correct answer(s): C
Rationale: Neutralizing documents doesn’t align generation tone/style.

Q15. Evaluating translation accuracy: which metric?
Choices:
A) Perplexity
B) ROUGE
C) BLEU
D) Cosine Similarity
Correct answer(s): C
Rationale: BLEU is standard for machine translation quality.

Q16. Designing a RAG app for technical regulations. Correct build/deploy sequence?
Choices:
A) Ingest → Index to Vector Search → User queries LLM → Retrieve → Evaluate → Generate → Deploy.
B) Ingest → Index to Vector Search → User queries LLM → Retrieve → Generate → Evaluate → Deploy.
C) Ingest → Index to Vector Search → Evaluate → Deploy.
D) User queries LLM → Ingest → Index → Retrieve → Generate → Evaluate → Deploy.
Correct answer(s): B
Rationale: Retrieve, then generate, then evaluate, then deploy.

Q17. Evaluate prompt-response pairs for a support chatbot. What criteria are most important? (Choose two)
Choices:
A) Response completeness
B) Random sampling for variety
C) Prompt relevance to the query
D) Token usage efficiency
E) Prompt verbosity
Correct answer(s): A, C
Rationale: Relevant prompts and complete responses ensure alignment.

Q18. Gaming chatbot engagement/retention: which metric helps?
Choices:
A) Randomness
B) Repetition of responses
C) Lack of relevance
D) Diversity of responses
Correct answer(s): D
Rationale: Diverse responses increase engagement.

Q19. Generate summaries of technical research articles. Which evaluation metric?
Choices:
A) Word Count
B) ROUGE
C) Latency
D) BLEU
Correct answer(s): B
Rationale: ROUGE is standard for summarization.

Q20. RAG app produces incomplete responses on large dataset of tickets. What strategy? (Choose two)
Choices:
A) Add segment labels (section headers) to chunks.
B) Use a larger embedding model.
C) Limit number of retrieved chunks.
D) Decrease chunk size.
E) Increase chunk size.
Correct answer(s): A, D
Rationale: Smaller chunks fit context; segment labels improve structure.

Q21. Recommend best-matched team member considering dates and profile alignment (unstructured text). Which architecture?
Choices:
A) Tool for date availability; embed team profiles; use project scope + filters for retrieval.
B) Tool for dates; embed project scopes; retrieve using team profiles.
C) Tool for dates; extract keywords; keyword match profiles.
D) Tool for dates; tool to compute similarity over profile-scope pairs; iterate and rank.
Correct answer(s): A
Rationale: Embed profiles and retrieve by project scope + availability.

Q22. Easiest process to deploy a trained LLM on Databricks?
Choices:
A) Flask + Gunicorn
B) Log with MLflow, register to Unity Catalog via MLflow API, start serving endpoint
C) Save locally, build Docker, run container
D) Pickle, upload to Unity Catalog Volume, register, serve
Correct answer(s): B
Rationale: MLflow + Unity Catalog streamline registration and serving.

Q23. Chunking/indexing legal texts with long sections. Best chunking strategy?
Choices:
A) Split into logical sections by headings/subheadings under token limits.
B) Process each document as a single chunk.
C) Overlap all chunks by 30% always.
D) Equal-sized chunks ignoring structure.
Correct answer(s): A
Rationale: Structure-aware chunking preserves meaning and fits limits.

Q24. App requires up-to-date news articles and stock prices. How to architect?
Choices:
A) Store news and prices in a vector store; use RAG at runtime.
B) Agent with tools for SQL on Delta and web search; provide values to LLM.
C) LLM summarizes news and looks up tickers.
D) Query Delta for prices and use LLM to generate volatility queries.
Correct answer(s): B
Rationale: Agent tools enable real-time stock and news integration.

Q25. RAG app on PDF source docs (text + images); least code to extract text?
Choices:
A) flask
B) unstructured
C) beautifulsoup
D) numpy
Correct answer(s): B
Rationale: The unstructured package extracts text from mixed-content PDFs with minimal code.

Q26. Need open-source LLM with large context window.
Choices:
A) Llama2-70B
B) DBRX
C) MPT-30B
D) DistilBERT
Correct answer(s): C
Rationale: MPT-30B is open-source and optimized for large context windows.

Q27. Augment inputs for meal-plan chatbot using ingredients and restrictions. (Choose two)
Choices:
A) Include additional context (portion sizes, preferences) in prompt.
B) Train a rule-based system.
C) Add examples of similar meal plans in the prompt.
D) Use a classification model to categorize inputs.
E) Summarize inputs to condense key points.
Correct answer(s): A, C
Rationale: Contextual prompts and examples drive personalized plans.

Q28. Small internal, sensitive RAG; high-quality answers; no third-party transmission. Which model?
Choices:
A) Llama2-70B
B) BGE-large
C) OpenAI GPT-4
D) Dolly 1.5B
Correct answer(s): A
Rationale: Llama2-70B is open-source, high-quality, and deployable internally.

Q29. Preprocessing pipeline to normalize queries and remove symbols. Which approach?
Choices:
A) PyFunc model implementing preprocessing logic integrated with app.
B) Delta Live Table to preprocess queries.
C) Spark job to normalize queries.
D) Fine-tune the LLM to preprocess internally.
Correct answer(s): A
Rationale: PyFunc is efficient, reusable for preprocessing.

Q30. Enhance RAG app using Delta customer data; need safe dev testing with realistic data.
Choices:
A) Point dev to production table.
B) Copy entire table into dev unchanged.
C) Use Unity Catalog to create dev-specific view with access controls and masking.
D) Use synthetic dataset mimicking schema and characteristics.
E) Use Delta Sharing for limited query access from dev.
Correct answer(s): C
Rationale: UC masked views provide realistic yet secure dev data.

Q31. Evaluate model for summarizing academic research papers for semantic accuracy and domain relevance. (Choose two)
Choices:
A) Human evaluation for domain correctness.
B) Evaluate token usage for cost.
C) BLEU for similarity with references.
D) Perplexity for fluency.
E) ROUGE for overlap plus domain-specific metrics.
Correct answer(s): A, E
Rationale: Combine ROUGE with human/domain evaluation.

Q32. RAG app answers about internal SnoPen AI docs; filter irrelevant content.
Choices:
A) Consolidate all SnoPen AI documents into one chunk.
B) Include system prompt constraint to only answer SnoPen AI-related questions.
C) Keep all articles, including irrelevant ones.
D) Tell the system all information is about SnoPen AI without filtering.
Correct answer(s): B
Rationale: System prompt constraints narrow scope effectively.

Q33. After creating `VectorSearchClient` and endpoint with default managed embeddings, what next?
Choices:
A) vsc.similarity_search()
B) vsc.create_direct_access_index()
C) vsc.create_delta_sync_index()
D) vsc.get_index()
Correct answer(s): C
Rationale: Create a Delta-synced index for embedding synchronization.

Q34. Agent-based LLM system for monster truck team: text Q&A, event API, table queries. Best design?
Choices:
A) Force LLM to output "RAG", "API", or "TABLE"; parse and route.
B) Put all possible dates/tables in the system prompt; use RAG for generic questions.
C) Write an agent system prompt listing tools and bundle into an agent that runs multiple calls to solve a query.
D) Ingest PDFs into a vector store and use only RAG.
Correct answer(s): C
Rationale: Tool-using agent supports flexible, multi-capability workflows.

Q35. Deployed application has insufficient volume for provisioned throughput. Cost-effective deployment?
Choices:
A) Use External Models instead.
B) Manually throttle incoming batches.
C) Deploy using pay-per-token throughput with cost guarantees.
D) Change to a smaller model to reduce hardware.
Correct answer(s): C
Rationale: Pay-per-token aligns costs with low usage.

Q36. LangChain chain error (LLMChain missing LLM). What change fixes it?
Choices:
A) Format prompt then LLMChain(prompt=prompt.format("funny")); llm.generate()
B) LLMChain(llm=OpenAI(), prompt=prompt); llm.generate([{"adjective": "funny"}])
C) Put llm=OpenAI() inside PromptTemplate
D) LLMChain(prompt=prompt); llm.generate("funny")
Correct answer(s): B
Rationale: LLMChain must be initialized with an LLM instance.

Q37. Cost-conscious startup in cancer research building RAG with Foundation Model APIs. Best quality while cost-efficient?
Choices:
A) Limit number of queries per customer per day.
B) Use the largest LLM.
C) Limit number of relevant documents retrievable.
D) Pick a smaller, domain-specific LLM.
Correct answer(s): D
Rationale: Smaller domain-specific models balance quality and cost.

Q38. Dataframe with document name and array of text chunks; want to store for Vector Search. Most performant way?
Choices:
A) Create unique ID per document; save to Delta.
B) Flatten to one chunk per row, unique ID per row; save to Delta.
C) Split into train/test; unique ID per document; save to Delta.
D) Store each chunk as independent JSON in UC Volume.
Correct answer(s): B
Rationale: Flattened rows with unique IDs optimize retrieval.

Q39. Serve a code generation model for developers (multiple languages); quality is primary. Best fit?
Choices:
A) BGE-large
B) CodeLlama-34B
C) MPT-7b
D) Llama2-70b
Correct answer(s): B
Rationale: CodeLlama-34B is specialized for multi-language code generation.

Q40. Fraud detection system using LLM; needs experiment tracking, metrics, deploy to prod. Which approach?
Choices:
A) Custom scripts + Flask deploy.
B) Cloud document management for tracking.
C) TensorFlow built-in logging/deploy.
D) MLflow to log experiments, metrics, and streamline deployment.
E) Spreadsheet to record results.
Correct answer(s): D
Rationale: MLflow covers tracking and deployment lifecycle.

Q41. Live sports commentary platform with real-time updates. Which tool enables real-time data access?
Choices:
A) Foundation Model APIs
B) Feature Serving
C) DatabricksIQ
D) AutoML
Correct answer(s): B
Rationale: Feature Serving delivers low-latency real-time data.

Q42. Evaluate an internal RAG app formally.
Choices:
A) Use LLM-as-a-judge on final answers only.
B) Curate datasets to test retrieval and generation separately; use MLflow evaluation metrics.
C) Use cosine similarity on final answers only.
D) Benchmark multiple LLMs and pick the best.
Correct answer(s): B
Rationale: Separate retrieval and generation evaluation is systematic and actionable.

Q43. Reasoning system for legal contracts: highlight risks, propose alternatives, summarize. What tools? (Choose two)
Choices:
A) Retrieval system for legal precedents/templates.
B) Classification model for clause risk levels.
C) Decision tree to guide users.
D) Embedding model to semantically analyze clauses.
E) Visualization tool to highlight clauses.
Correct answer(s): A, D
Rationale: Embeddings + retrieval enable risk evaluation and clause suggestions.

Q44. RAG system user satisfaction declines due to irrelevant outputs; retrieval latency is high. Focus on which metrics?
Choices:
A) Perplexity and BLEU.
B) Precision, recall, and retrieval latency.
C) User query frequency.
D) Token usage and response length.
Correct answer(s): B
Rationale: Relevance and latency metrics diagnose retrieval quality and bottlenecks.

Q45. Changed LLM to shorter context; getting prompt token count exceeds limit. What two solutions without changing response model? (Choose two)
Choices:
A) Decrease chunk size of embedded documents.
B) Use a smaller embedding model.
C) Reduce maximum output tokens.
D) Reduce number of records retrieved from vector DB.
E) Retrain response model using ALiBi.
Correct answer(s): A, D
Rationale: Smaller chunks and fewer retrieved records reduce total tokens.

Q46. Prevent hallucinations in medical queries.
Choices:
A) Use shorter prompts.
B) Lower temperature.
C) Ground responses using RAG connected to verified medical sources.
D) Train on diverse datasets.
Correct answer(s): C
Rationale: Grounding with verified sources reduces hallucinations.

Q47. Personalized travel itineraries incorporating budget, preferences, destination details. Critical components? (Choose two)
Choices:
A) Rule-based system with predefined itineraries.
B) Summarization model to condense inputs.
C) Classification model for destinations by type.
D) Embedding model for destinations and preferences.
E) LLM to generate detailed itineraries.
Correct answer(s): D, E
Rationale: Embeddings for matching + LLM for detailed generation.

Q48. Banking conversational agent handling sensitive information securely. Best approach?
Choices:
A) Use summarization to reduce data volume.
B) Lower temperature.
C) Train on anonymized datasets only.
D) Implement encryption for sensitive data and guardrails to avoid leakage.
Correct answer(s): D
Rationale: Encryption + guardrails are core to secure handling.

Q49. Translate unstructured user feedback into actionable tasks with priorities and deadlines. Best approach?
Choices:
A) Summarization model only.
B) LLM to analyze feedback and generate structured outputs with priorities and deadlines.
C) Classification model to categorize feedback.
D) Rule-based keyword extraction.
Correct answer(s): B
Rationale: LLMs can produce structured, actionable outputs from text.

Q50. Extract text from scanned invoices in JPG format.
Choices:
A) PyPDF2
B) BeautifulSoup
C) Tesseract OCR via pytesseract
D) pdfplumber
Correct answer(s): C
Rationale: pytesseract provides OCR for images like JPGs.

Q51. Generate summaries of financial reports with tables; what to prioritize?
Choices:
A) Largest context length
B) Model optimized for numerical reasoning and summarization
C) Open-domain generative model
D) Cheapest model
Correct answer(s): B
Rationale: Numerical reasoning is critical for financial tables.

Q52. LangChain chain for multi-turn questions about device specs with detailed responses. Include which elements? (Choose two)
Choices:
A) Rule-based system for fixed answers.
B) Summarization model to shorten long responses.
C) Retriever to fetch device specs from vector store.
D) Prompt template maintaining conversation history.
E) Sentiment analysis.
Correct answer(s): C, D
Rationale: Retriever + conversational prompt provide context-aware answers.

Q53. Which temperature setting encourages creative, varied responses?
Choices:
A) Temperature near 0
B) Exactly 0.5
C) Disable temperature control
D) Higher temperature (e.g., 1.0+)
Correct answer(s): D
Rationale: Higher temperature increases randomness/creativity.

Q54. LangChain chain must include context from a vector store and generate coherent answers. What components? (Choose two)
Choices:
A) Prompt template to structure user queries.
B) Rule-based system to hardcode responses.
C) Retriever for relevant context from vector store.
D) Summarization model for condensing inputs.
E) Classification for intents.
Correct answer(s): A, C
Rationale: Prompt templates + retrievers are core to RAG with LangChain.

Q55. Pipeline to convert unstructured legal documents into structured summaries highlighting clauses, risks, obligations. Which tools?
Choices:
A) LLM fine-tuned on legal text for structured summaries.
B) Rule-based clause extraction.
C) Summarization model trained on general text.
D) Retrieval for similar legal cases.
Correct answer(s): A
Rationale: Fine-tuned legal LLMs generate structured, domain-appropriate summaries.

Q56. Register a deployed model to Unity Catalog using MLflow. Correct workflow?
Choices:
A) Train → Log to MLflow → Configure metadata → Register to Unity Catalog.
B) Log to MLflow but skip registration.
C) Skip metadata configuration.
D) Deploy directly to Unity Catalog without MLflow logging.
Correct answer(s): A
Rationale: MLflow logging and metadata precede UC registration.

Q57. Deploy a secure endpoint for a RAG app with sensitive legal docs; privacy compliance. Essential measures? (Choose two)
Choices:
A) Encrypt data at rest and in transit.
B) Shared access key for all users.
C) API key-based authentication for endpoint access.
D) Skip authentication.
E) Public endpoints without encryption.
Correct answer(s): A, C
Rationale: Encryption and authenticated access are mandatory for compliance.
