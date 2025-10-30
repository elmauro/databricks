### 1. A Generative AI Engineer must identify source documents to train a RAG system for answering technical support questions. What factors should they prioritize?

- ✅ **Relevance and quality of content based on the system’s technical support requirements.**
- Diversity of sources over content relevance.
- Length of documents to maximize training data.
- Availability of free or public domain documents.

Selecting high-quality, relevant documents ensures the system retrieves accurate answers aligned with technical support requirements.

---

### 2. A Generative AI Engineer is optimizing an LLM-based system for summarizing academic papers. The summaries often miss key points from the results section. What strategy should they use?

- Use vague prompts like “Summarize this paper.”
- Summarize without prompts to save computation.
- Include the entire paper in the prompt for context.
- ✅ **Include section-specific instructions in the prompt, such as “Focus on the results section.”**

Adding section-specific instructions ensures the model focuses on critical information, such as the results section, improving summary relevance.

---

### 3. A RAG system retrieves irrelevant historical data during queries. How should the engineer improve the relevance of retrieved documents?

- Increase the document chunk size to improve coverage.
- Retain all documents regardless of age.
- Filter based on keyword matches in queries.
- ✅ **Apply timestamp-based filtering to exclude outdated documents.**

Timestamp-based filtering ensures retrieved content remains up-to-date and relevant, addressing issues of outdated information.

---

### 4. A Generative AI Engineer must select an LLM for a question-answering system about medical guidelines. The system needs high accuracy and explainable outputs. Which LLM attribute should they prioritize?

- ✅ **Model size and domain-specific fine-tuning.**
- Pre-trained embeddings on generic text.
- Response speed and low latency.
- High perplexity to ensure diverse responses.

Prioritizing domain-specific fine-tuning ensures that the LLM delivers accurate and explainable outputs for medical guideline queries.

---

### 5. A Generative AI Engineer is tasked with identifying problematic text in a dataset feeding a RAG application. Some documents contain inappropriate language. What mitigation strategy should they implement?

- Rely on the LLM to ignore inappropriate language during processing.
- ✅ **Use a filtering tool to detect and remove inappropriate language before ingestion.**
- Allow all documents to be ingested and rely on output monitoring.
- Replace inappropriate words with placeholders in the ingested documents.

Pre-ingestion filtering removes problematic text, ensuring the dataset‘s quality and ethical compliance.

---

### 6. A Generative AI Engineer is tasked with chunking a lengthy novel for a RAG application. The novel includes chapters, dialogues, and descriptive passages. Which chunking strategy will optimize retrieval quality?

- Include the entire novel as one chunk for context preservation.
- ✅ **Split the text by chapters and further divide each chapter into sections based on paragraphs.**
- Overlap chunks with a 50% overlap to ensure content redundancy.
- Split the text into chunks of a fixed token size (e.g., 500 tokens).

Logical chunking by chapters and paragraphs ensures meaningful content for retrieval and alignment with model constraints.

---

### 7. A Generative AI Engineer is designing a conversational agent using Foundation Model APIs. The system must handle variable query loads efficiently. How can the engineer optimize performance?

- Fix resource allocation at the maximum expected load.
- Skip monitoring performance metrics to simplify deployment.
- ✅ **Use autoscaling to dynamically adjust resources based on query load.**
- Reduce API rate limits to restrict user queries.

Autoscaling dynamically adjusts resources to match workload demands, ensuring consistent and efficient performance.

---

### 8. A Generative AI Engineer must create a system that provides multi-turn dialogue capabilities for a customer support chatbot. The chatbot should maintain context across conversations and handle follow-up queries. What tool is essential for maintaining context?

- ✅ **A memory module integrated into the LLM workflow to store and retrieve past interactions.**
- A summarization model to condense conversation history.
- A retriever to fetch relevant past conversations.
- A classification model to predict user intents in each query.

Memory modules enable dynamic storage and retrieval of past interactions, ensuring continuity and context-aware dialogue.

---

### 9. A Generative AI Engineer must register a pyfunc model that predicts employee attrition to Unity Catalog. The model includes preprocessing for data normalization and postprocessing for interpretable outputs. What steps should the engineer take?

- Skip metadata configuration and use default settings for registration.
- Skip logging to MLflow and directly register the model to Unity Catalog.
- ✅ **Log the model to MLflow, configure metadata (e.g., schema, version, tags), and register it to Unity Catalog.**
- Use MLflow for logging but avoid registering the model to Unity Catalog.

Logging to MLflow and registering to Unity Catalog ensures the model is properly managed, tracked, and governed.

---

### 10. A Generative AI Engineer must design a system that generates customer response templates based on common queries. The templates should include a greeting, resolution, and follow-up action. What pipeline components are necessary?

- A rule-based system to select predefined templates.
- A summarization model to condense user queries.
- A classification model to label queries by type.
- ✅ **An LLM to generate templates dynamically based on queries.**

LLMs dynamically create structured templates with greetings, resolutions, and follow-ups, meeting user requirements.

---

### 11. A Generative AI Engineer is designing a system that converts job descriptions into candidate evaluation criteria. The system must generate a structured checklist for each job description. What components should be included in the pipeline?

- ✅ **Use an embedding model for job descriptions and an LLM to generate evaluation criteria.**
- Use a summarization model to condense job descriptions into shorter forms.
- Use a rule-based system to extract criteria.
- Use a classification model to label job descriptions by category.

Combining embeddings and LLMs ensures the pipeline generates accurate and structured evaluation criteria.

---

### 12. A Generative AI Engineer has developed an LLM application to answer questions about internal company policies. The engineer must ensure that the application doesn’t hallucinate or leak confidential data. Which approach should NOT be used to mitigate hallucination or confidential data leakage?

- Use a strong system prompt to ensure the model aligns with your needs.
- Add guardrails to filter outputs from the LLM before it is shown to the user.
- ✅ **Fine-tune the model on your data, hoping it will learn what is appropriate and not.**
- Limit the data available based on the user’s access level.

Fine-tuning alone is unreliable for preventing hallucinations or ensuring security. Guardrails, access control, and system prompts are more effective.

---

### 13. A Generative AI Engineer is tasked with designing a system for generating personalized workout routines. The system should consider user goals, preferences, and activity history. What is the most effective pipeline design?

- ✅ **Use an LLM to generate workout routines based on inputs, supported by a retriever for user activity history.**
- Use embeddings to represent user inputs semantically.
- Use a classification model to categorize user preferences.
- Use a rule-based system to map inputs to predefined workouts.

A combination of an LLM and retriever ensures personalized and dynamic workout routines based on user inputs.

---

### 14. A Generative AI Engineer must implement guardrails to prevent an AI chatbot from leaking sensitive customer information. What is the most effective approach?

- Enable temperature-based randomness in outputs.
- Rely on user feedback to detect sensitive data leakage.
- Reduce the token length of responses to avoid excessive detail.
- ✅ **Use prompt-based filters to detect and block sensitive data during inference.**

Prompt-based filters actively monitor and block sensitive content, ensuring responses remain secure and compliant with privacy standards.

---

### 15. A Generative AI Engineer is tasked with creating a prompt for generating job interview questions. The questions should focus on assessing problem-solving skills and teamwork. What is the best prompt structure?

- ✅ **Design a set of job interview questions that evaluate problem-solving and teamwork abilities, providing clear and concise queries.**
- Generate general interview questions for job candidates.
- Summarize common skills assessed during job interviews.
- Provide a list of common problem-solving scenarios.

Explicit prompts that specify the skills and structure needed ensure the generated questions are focused and relevant.

---

### 16. A Generative AI Engineer is tasked with writing chunked user feedback data into Delta Lake tables. Which operation sequence ensures optimal data preparation and storage?

- Write all feedback data as one large chunk into Delta Lake.
- Chunk the text first → Write the chunks into Delta Lake → Define the schema later.
- ✅ **Define schema for feedback data → Chunk the text → Write chunks into Delta Lake tables.**
- Skip chunking and directly write the feedback into Delta Lake.

Defining the schema first ensures data consistency, and chunking facilitates efficient storage and retrieval in Delta Lake tables.

---

### 17. A Generative AI Engineer is evaluating the performance of a RAG application that retrieves documents and generates summaries. The retrieval step has high recall but low precision. What does this indicate, and how should it be addressed?

- ✅ **The system retrieves many relevant documents but also includes irrelevant ones, requiring better filtering or embeddings.**
- The system retrieves all relevant documents but excludes others, requiring larger batch sizes.
- The summarization step is producing incomplete summaries.
- The system has low latency, which reduces retrieval accuracy.

High recall but low precision means the system retrieves many irrelevant documents, which can be addressed with better embeddings or retrieval filters.

---

### 18. A Generative AI Engineer at an electronics company just deployed a RAG application for customers to ask questions about products. However, they received feedback that the RAG response often returns information about an irrelevant product. What can the engineer do to improve the relevance of the RAG’s response?

- Implement caching for frequently asked questions.
- Use a different semantic similarity search algorithm.
- ✅ **Assess the quality of the retrieved context.**
- Use a different LLM to improve the generated response.

Evaluating the quality of retrieved context ensures that only accurate and relevant information is passed to the LLM, solving relevance issues effectively.

---

### 19. A Generative AI Engineer is tasked with selecting tools for a pipeline that converts text-based user reviews into categorized insights. The categories include product quality, delivery, and customer service. What components are critical?

- A retrieval system to fetch similar reviews.
- A summarization model to condense user reviews.
- ✅ **A classification model to categorize user reviews based on predefined categories.**
- An embedding model to represent reviews semantically.

A classification model ensures reviews are assigned to the correct categories, enabling actionable insights from user feedback.

---

### 20. A Generative AI Engineer must register a custom LLM to Unity Catalog using MLflow. What are the key steps in this process?

- ✅ **Log the model in MLflow, configure its metadata (e.g., schema and description), and register it to Unity Catalog for governance and discovery.**
- Skip metadata configuration during registration.
- Use only the MLflow tracking server without registering the model.
- Register the model directly in Unity Catalog without logging it in MLflow.

Proper logging, metadata configuration, and registration ensure that the model is discoverable and governed effectively within Unity Catalog.

---

### 21. A Generative AI Engineer is developing a customer support chatbot for an e-commerce platform. The chatbot must provide answers to common queries while escalating unresolved issues to human agents. What should the engineer prioritize in the design?

- Use rule-based logic for predefined query handling.
- ✅ **Implement a fallback mechanism to escalate unresolved queries to human agents.**
- Train the chatbot on a large dataset of historical conversations.
- Focus on embedding models to understand user queries semantically.

Fallback mechanisms enhance user experience by ensuring unresolved queries are appropriately escalated to human agents.

---

### 22. A Generative AI Engineer needs to develop a pipeline that identifies and extracts customer intents from chatbot queries. Which embedding model should they select?

- GloVe
- ✅ **Sentence Transformers**
- BERT-base for classification
- Word2Vec

Sentence Transformers excel in generating embeddings for entire sentences, making them ideal for tasks like intent detection in chatbot queries.

---

### 23. A Generative AI Engineer is building a generative application for processing user-provided content. The application must protect against malicious user inputs such as injection attacks. What guardrail technique should they use?

- Use a summarization model to preprocess user inputs.
- Allow unrestricted user inputs to encourage flexibility.
- ✅ **Sanitize user inputs by removing special characters and validating data formats.**
- Log and monitor user inputs for suspicious patterns.

Input sanitization effectively neutralizes harmful payloads, ensuring application security against malicious inputs.

---

### 24. A Generative AI Engineer must evaluate an LLM for summarizing financial news articles. The system must generate accurate, concise, and timely summaries. What metrics should guide model evaluation and deployment? (Choose Four)

- Token usage per query to track computational cost.
- ✅ **Latency to ensure the system delivers summaries quickly.**
- ✅ **ROUGE for evaluating content relevance and overlap with reference summaries.**
- ✅ **BLEU to compare word sequences in generated summaries.**
- ✅ **Perplexity to assess the fluency of summaries.**

A combination of latency, ROUGE, BLEU, and perplexity ensures the system is fast, accurate, and user-friendly for financial news summarization.

---

### 25. A Generative AI Engineer is tasked with reviewing the licensing requirements of a dataset containing medical records used in a generative application. What steps should they take to ensure legal compliance? (Choose two)

- Use the dataset as long as it is publicly available.
- ✅ **Check the licensing terms to confirm the dataset can be used for commercial purposes.**
- Assume non-commercial use does not require licensing checks.
- ✅ **Verify that the dataset complies with HIPAA or relevant data privacy regulations.**
- Ensure the LLM’s outputs include disclaimers about data origins.

Verifying privacy regulations and licensing terms ensures the dataset is legally compliant and suitable for use in the application.

---

### 26. A Generative AI Engineer is designing an AI assistant for lawyers. The assistant must retrieve legal documents and generate concise summaries. What is the most important consideration for ensuring accurate retrieval?

- Use a keyword-based search algorithm.
- Fine-tune the LLM on legal datasets.
- ✅ **Implement a vector database for semantic search.**
- Build a rule-based filtering system.

Semantic search via a vector database ensures the system retrieves relevant legal documents, providing accurate inputs for generating concise summaries.

---

### 27. A Generative AI Engineer needs to register a machine learning model in Unity Catalog for deployment. What benefits does this provide?

- Reduced model inference time.
- Automated hyperparameter tuning.
- ✅ **Centralized model governance, versioning, and access control.**
- Fine-tuning capabilities for custom tasks.

Unity Catalog centralizes model governance, enabling tracking, versioning, and access control for robust machine learning workflows.

---

### 28. A Generative AI Engineer is building a chatbot for a travel agency. The chatbot must recommend travel destinations based on weather preferences, budget, and activities. What is the first step in pipeline design?

- Select an LLM trained on travel data for recommendations.
- Create a database of destinations and activities.
- Gather feedback on early recommendations.
- ✅ **Define input fields like budget, preferred_weather, and activities and output fields like destination and activity_recommendations. Example: Input: { “budget”: “$3000”, “preferred_weather”: “tropical”, “activities”: “adventure” } → Output: { “destination”: “Thailand”, “activity_recommendations”: “snorkeling, jungle trekking” }.**

Clearly defining inputs like budget and outputs like destination recommendations ensures alignment with user requirements and guides the pipeline’s architecture.

---

### 29. A Generative AI Engineer must create and query a vector search index for a knowledge management system. What steps are essential to ensure accurate results?

- Rely on raw document storage without embeddings.
- Use only keyword-based matching for queries.
- ✅ **Preprocess the documents, create embeddings for the text, store the embeddings in a vector store, and use nearest-neighbor search for queries.**
- Skip document preprocessing and directly create embeddings.

A pipeline that preprocesses documents, creates embeddings, and queries using vector search ensures accurate and efficient results.

---

### 30. A Generative AI Engineer is tasked with building a Vector Search index to handle document queries for a knowledge base. What steps are required to create and query this index?

- Use a retrieval model directly on raw documents without embeddings.
- ✅ **Preprocess documents, embed them using an embedding model, store the embeddings in a vector store, and use a retriever for queries.**
- Use embeddings to encode queries but not documents.
- Skip preprocessing and store raw document text in a vector store for querying.

Creating a Vector Search index requires preprocessing, embedding, storing in a vector store, and using retrievers for query handling.

---

### 31. A Generative AI Engineer is evaluating the retrieval performance of a RAG system. Which metrics are most relevant to this evaluation?

- BLEU scores for retrieval evaluation.
- ✅ **Precision and recall to measure relevance and coverage of retrieved results.**
- Perplexity to measure response fluency.
- Latency to measure response time.

Precision and recall are key metrics for evaluating the relevance and coverage of retrieved results in RAG systems.

---

### 32. A Generative AI Engineer is tasked with registering an LLM to Unity Catalog using MLflow. What information must be configured during registration?

- ✅ **Model metadata such as name, version, schema, and tags for tracking and governance.**
- Use only MLflow without registering the model to Unity Catalog.
- Only the model name and version without schema or tags.
- Skip metadata configuration and register the model with default settings.

Metadata like schema and tags ensures models are registered with all necessary details for effective governance and tracking.

---

### 33. A Generative AI Engineer is tasked with coding a LangChain-based RAG application that queries a large dataset of financial reports. The dataset includes structured and unstructured data. What components should the chain include? (Choose two)

- A summarization model to condense retrieval results.
- ✅ **A retriever to fetch relevant data from the vector store.**
- A classification model to categorize financial queries.
- A pre-trained sentiment analysis model.
- ✅ **An embedding model to process both structured and unstructured data.**

Retrievers and embedding models ensure accurate and efficient query handling in the financial reports dataset.

---

### 34. Which metric is most suitable for evaluating the accuracy of summaries generated by an LLM?

- BLEU score
- Perplexity
- ✅ **ROUGE score**
- F1 score

The ROUGE score is specifically designed to evaluate summarization tasks, making it the best metric for assessing summary accuracy.

---

### 35. A Generative AI Engineer is designing a customer service chatbot to handle product return requests. The chatbot must ensure that responses are both accurate and polite. What approach should they use to implement this?

- ✅ **Fine-tune the LLM on a polite tone dataset and return policy information.**
- Rely on summarization techniques to handle user inputs.
- Add politeness rules into the system as post-processing filters.
- Use a generic pre-trained model for handling responses.

Fine-tuning aligns the LLM’s tone and knowledge base, ensuring polite and accurate responses for customer service scenarios.

---

### 36. A Generative AI Engineer is tasked with building a Retrieval-Augmented Generation (RAG) system for a healthcare organization. The system needs to retrieve patient-specific documents and summarize key findings securely. What are the critical components for building this pipeline? (Choose four)

- Classification model for tagging medical conditions.
- ✅ **Embedding model trained on healthcare datasets.**
- ✅ **Summarizer model optimized for clinical reports.**
- ✅ **Secure document retriever with access controls.**
- ✅ **Low-latency GPU infrastructure for real-time queries.**

RAG systems for healthcare require domain-specific embedding models, secure retrievers, summarization capabilities, and high-performance compute resources to ensure accuracy, compliance, and efficiency.

---

### 37. A Generative AI Engineer is deploying a chatbot for a healthcare application. How can the engineer ensure the chatbot does not generate private patient information in its outputs?

- Encrypt all chatbot responses before returning them to the user.
- Limit the chatbot‘s response length to prevent detailed outputs.
- Train the chatbot on anonymized datasets only.
- ✅ **Use input filtering and response validation to block private data during processing and output generation.**

Input filtering and response validation ensure that private information is excluded from chatbot outputs, complying with privacy requirements.

---

### 38. A Generative AI Engineer must create a pipeline that retrieves financial reports and summarizes key performance metrics. The system must handle thousands of reports efficiently. What is the correct sequence for designing this pipeline?

- 1. Preprocess all reports → 2. Deploy a retriever → 3. Fine-tune a language model → 4. Summarize outputs.
- 1. Fine-tune a summarization model → 2. Generate embeddings → 3. Build a retriever → 4. Deploy to production.
- 1. Deploy the retriever → 2. Store raw reports in a database → 3. Summarize retrieved reports → 4. Index the database.
- ✅ **1. Create embeddings for all financial reports → 2. Store embeddings in a Vector Search index → 3. Build a retriever → 4. Deploy a summarization model for retrieval outputs.**

Embedding generation, vector indexing, retrieval, and summarization ensure the pipeline is scalable and optimized for financial document processing.

---

### 39. A Generative AI Engineer must design a prompt for a chatbot that provides software installation instructions. The response should include step-by-step instructions and warnings for common issues. What should the prompt include?

- Provide a concise explanation of software features.
- Include only the main steps of the installation process.
- Summarize the installation process for the software.
- ✅ **Provide detailed installation instructions, including common warnings and troubleshooting tips.**

A clear, detailed prompt ensures the chatbot provides step-by-step instructions with warnings, aligning with user expectations.

---

### 40. A Generative AI Engineer needs to evaluate the output of a chatbot that generates legal advice. The outputs often include ambiguous statements. What method should they use to identify and resolve these issues?

- Use a summarization model to shorten responses.
- Measure BLEU scores for the chatbot’s responses.
- ✅ **Qualitatively assess the responses for clarity and accuracy, then refine prompts to reduce ambiguity.**
- Train the chatbot on more extensive datasets.

Qualitative assessments and prompt refinement directly tackle the root causes of ambiguous responses, ensuring improved outputs.

---

### 41. A Generative AI Engineer is tasked with building a prompt for a chatbot that answers customer product questions. The chatbot should prioritize factual accuracy over creativity. What should the prompt include?

- No instructions, relying on the model’s default behavior.
- General guidance like “Answer the query.”
- Instructions to “Be creative in your response.”
- ✅ **Explicit instructions such as “Provide concise, factual answers to customer questions.”**

Clear and explicit prompts guide the LLM to produce accurate, consistent responses tailored to the application‘s needs.

---

### 42. A Generative AI Engineer is optimizing document retrieval for a customer support chatbot. User feedback indicates that irrelevant results frequently appear. What should the engineer focus on improving?

- ✅ **Precision**
- Document length filtering
- Response generation diversity
- Recall

Focusing on precision ensures the chatbot retrieves only relevant content, improving user satisfaction and system accuracy.

---

### 43. A Generative AI Engineer is selecting a model for generating secure medical chatbot responses. Which feature is critical for ensuring data privacy?

- ✅ **The model should not store user input or generate responses based on external data storage.**
- The model should always prioritize speed over accuracy.
- The model should allow unlimited external API calls.
- The model should prioritize response diversity.

A model that avoids storing user input and external dependencies ensures compliance with privacy standards in sensitive domains like healthcare.

---

### 44. A Generative AI Engineer is designing a prompt for a QA system to answer questions about historical events. The response must include dates, key figures, and outcomes. How should the prompt be structured?

- Provide a summary of the historical event.
- Explain the causes of the historical event.
- Provide a general overview of historical events.
- ✅ **Answer the question with details about dates, key figures, and outcomes.**

A clear prompt specifying required fields ensures the QA system generates accurate and comprehensive responses.

---

### 45. A Generative AI Engineer is designing a pipeline for multi-turn dialogue in a customer support chatbot. The chatbot often forgets earlier turns. What should they implement?

- Increase the token limit.
- Use a lower temperature setting.
- ✅ **State tracking to retain the context of previous dialogue turns.**
- Reset the context after each turn.

State tracking ensures the chatbot remembers previous conversation turns, improving coherence and user experience.

---

### 46. A Generative AI Engineer needs to extract customer feedback from PDFs containing tables, images, and text. What tool is most appropriate for this task?

- BeautifulSoup for parsing PDF content.
- ✅ **pdfplumber for extracting structured text and tables from PDFs.**
- PyPDF2 for reading and extracting PDF content.
- pytesseract for OCR extraction.

pdfplumber is the best tool for extracting structured content like text and tables from PDFs, ensuring accurate data retrieval.

---

### 47. A Generative AI Engineer must assess the scalability of a production LLM application in handling increasing query volumes. What metrics are most critical?

- Token usage and cost per query.
- ✅ **Throughput and latency under varying workloads.**
- BLEU and perplexity.
- Model size and parameter count.

Throughput and latency are critical for evaluating how well a production LLM application scales under increasing workloads.

---

### 48. A Generative AI Engineer is using inference logging to monitor a RAG application. What insights can inference logging provide?

- BLEU scores and perplexity for language outputs.
- ✅ **Query patterns, response times, and error occurrences.**
- Token usage and model size.
- Model training loss over time.

Inference logging provides actionable data about query patterns, latency, and errors, helping to optimize application performance.

---

### 49. A Generative AI Engineer is tasked with extracting text from HTML files containing product catalogs. What is the most suitable Python package for this task? (Choose two)

- PyPDF2 for extracting text from PDFs.
- ✅ **BeautifulSoup for parsing HTML content and extracting text.**
- ✅ **LXML for faster and efficient parsing of large HTML files.**
- pytesseract for OCR tasks.
- pdfplumber for text extraction.

Combining BeautifulSoup and LXML ensures robust text extraction from HTML files, meeting the task requirements.

---

### 50. A Generative AI Engineer is debugging a system where an LLM provides incomplete responses to user queries. What is the most likely cause?

- The user queries are too short.
- The temperature setting is too high.
- The model is too large for the task.
- ✅ **The prompt does not provide clear instructions or adequate context.**

Ensuring prompts are detailed and clear helps guide the LLM to generate complete and accurate responses.

---

### 51. A Generative AI Engineer is evaluating an embedding model for a system that recommends job candidates based on resumes and job descriptions. The model has a 512-token context length. How should they adapt the system for longer resumes?

- Ignore longer resumes and process only those within the token limit.
- Train a new embedding model with a longer context length.
- ✅ **Chunk resumes into smaller sections within the 512-token limit and process each chunk independently.**
- Use a summarization model to reduce resume length.

Chunking resumes into smaller sections allows the system to process longer inputs while adhering to the token limit.

---

### 52. A Generative AI Engineer is tasked with creating a simple chain in LangChain that processes user queries about product specifications. The chain must return precise answers from a structured product database. What components are required? (Choose two)

- A pre-trained classification model for query analysis.
- ✅ **A database retriever to fetch relevant product specifications.**
- A rule-based filtering system for product categories.
- ✅ **A structured prompt template to format user queries.**
- A summarization model to shorten user queries.

Combining retrievers and structured prompts ensures accurate and precise responses for product-related queries.

---

### 53. A Generative AI Engineer must generate multiple-choice questions from a large corpus of educational material. The questions should assess conceptual understanding and application of knowledge. What approach should they take?

- Use a summarization model to distill the material into key points.
- Train a classification model to generate answers from the material.
- Fine-tune the LLM on educational datasets.
- ✅ **Use a prompt template that specifies question structure, options, and the correct answer.**

Prompt templates ensure that the LLM generates questions aligned with the educational goals, including structured options and correct answers.

---

### 54. A Generative AI Engineer must build a system to generate educational content summaries for students. The summaries should include three fields: key concepts, examples, and practical applications. What is the best pipeline design?

- ✅ **Use an LLM to generate summaries dynamically, structured into key concepts, examples, and applications.**
- Use a rule-based system to extract predefined educational elements.
- Use a summarization model to condense content into shorter text.
- Use an embedding model to represent educational text semantically.

LLMs dynamically generate structured educational content, aligning with requirements for fields like key concepts and applications.

---

### 55. A Generative AI Engineer must preprocess a dataset of forum posts for a support chatbot. Many posts include hyperlinks and embedded media. What preprocessing step should the engineer take?

- Exclude posts containing hyperlinks or media entirely.
- Summarize posts without removing hyperlinks or media.
- ✅ **Remove hyperlinks and embedded media, retaining only the text content.**
- Retain hyperlinks to preserve additional context.

Removing hyperlinks and media ensures the dataset remains focused on language content, optimizing the chatbot’s training and performance.

---

### 56. A Generative AI Engineer must create a prompt that ensures consistent responses from an LLM when summarizing financial reports. What is the most effective prompt strategy?

- Use a short, vague prompt like “Summarize the report.”
- ✅ **Provide clear instructions and examples in the prompt, such as “Summarize the key financial figures and trends clearly and concisely.”**
- Allow the model to interpret the task without guidance.
- Use multiple prompts with contradictory instructions.

A well-structured prompt with explicit instructions and examples improves response consistency and aligns outputs with expectations.

---

### 57. A Generative AI Engineer is working with a dataset containing both primary and secondary healthcare documents for a medical chatbot. What type of documents should they prioritize for ensuring factual accuracy?

- Patient blogs and forum discussions.
- Summaries of medical textbooks.
- Health-related social media posts.
- ✅ **Peer-reviewed medical research papers and government health guidelines.**

Using credible sources like peer-reviewed papers and government guidelines ensures reliable and factual medical chatbot responses.

---

### 58. A Generative AI Engineer must sequence the steps to deploy a RAG application endpoint. What is the correct order? (Choose two)

- Use hardcoded responses instead of a retriever.
- Deploy the endpoint first and configure embeddings later.
- Rely only on the LLM without embedding or vector search.
- ✅ **Train the embedding model, preprocess documents, create a vector store, and deploy the endpoint.**
- ✅ **Preprocess documents and create a vector store before deploying the endpoint.**
- Skip preprocessing and deploy documents directly to the endpoint.

Preprocessing, embeddings, and vector store creation are essential steps for deploying an effective RAG application endpoint.

---

### 59. When developing an LLM application, it’s crucial to ensure that the data used for training the model complies with licensing requirements to avoid legal risks. Which action is NOT appropriate to avoid legal risks?

- Only use data explicitly labeled with an open license and ensure the license terms are followed.
- Use any available data you personally created which is completely original and you can decide what license to use.
- ✅ **Reach out to the data curators directly after you have started using the trained model to let them know.**
- Reach out to the data curators directly before you have started using the trained model to let them know.

Always establish permissions or verify licensing terms before using third-party data in training to avoid legal risks.

---

### 60. To reduce hallucinations in LLM responses, what is the most effective approach when implementing a retrieval-augmented generation (RAG) pipeline?

- Increase the temperature parameter during inference.
- Shorten the length of model responses.
- ✅ **Use a high-quality document retrieval system to provide accurate context for the model.**
- Train the model on a larger dataset.

Grounding model responses in high-quality, retrieved context ensures factual outputs, which is critical for reducing hallucinations.
