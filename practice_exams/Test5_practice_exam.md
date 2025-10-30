### 1. A Generative AI Engineer is tasked with creating a multilingual chatbot for e-commerce. What factor should they prioritize when selecting an LLM?

- ✅ **Multilingual performance benchmarks.**
- Latency.
- Model size.
- Token length support.

Prioritizing multilingual benchmarks ensures the LLM can handle a wide range of languages effectively for global e-commerce scenarios.

---

### 2. A Generative AI Engineer is creating a chain in LangChain to answer customer questions about technical manuals. The system should handle lengthy manuals with a 4,096-token limit. How should the engineer structure the chain?

- Skip retrievers and directly query the entire document.
- Process entire manuals without chunking.
- Summarize manuals into short paragraphs before querying.
- ✅ **Use chunking to divide manuals into manageable sections, a retriever to fetch relevant chunks, and an LLM to generate answers.**

Chunking, retrieval, and LLMs ensure token limits are respected and responses remain accurate and detailed.

---

### 3. A Generative AI Engineer is building a travel chatbot. The chatbot must answer user queries about destinations, suggest activities, and recommend accommodations. What is the best model task for this use case?

- ✅ **Retrieval-augmented generation (RAG)**
- Named entity recognition
- Text classification
- Summarization

Retrieval-augmented generation (RAG) ensures chatbot responses are grounded in retrieved knowledge, enabling accurate and personalized recommendations for users.

---

### 4. A Generative AI Engineer must deploy a Retrieval-Augmented Generation (RAG) application for technical documentation. What is the first step in the deployment process?

- Configure the endpoint for inference.
- Register the model in Unity Catalog.
- ✅ **Create vector embeddings for the technical documents.**
- Fine-tune the LLM for summarization.

Creating embeddings is the foundation of the retrieval process, enabling RAG applications to search and retrieve relevant documents effectively.

---

### 5. A Generative AI Engineer is building a pipeline for multi-turn dialogue generation in a customer support chatbot. Which approach minimizes errors over multiple turns?

- Reset the context after every turn.
- Limit dialogue length to a fixed number of turns.
- ✅ **Use memory or state tracking to retain user context across turns.**
- Use random sampling to increase diversity in responses.

State tracking ensures the chatbot maintains continuity and coherence throughout multi-turn interactions.

---

### 6. A Generative AI Engineer needs to design a prompt for an LLM to provide personalized fitness advice. The advice should include a recommended workout routine, calorie target, and hydration tips. What is the most effective prompt?

- Summarize the user’s fitness goals into actionable steps.
- ✅ **Provide personalized fitness advice, including a workout routine, daily calorie target, and hydration tips, based on user inputs.**
- Generate fitness advice based on user data.
- Explain general fitness principles to the user.

Explicit prompts detailing required fields ensure the LLM generates personalized, comprehensive fitness advice.

---

### 7. A retrieval-based AI system retrieves too many irrelevant documents. What preprocessing step should an engineer prioritize?

- Focus on documents with fewer tokens.
- Increase the document chunk size to include more context.
- ✅ **Filter documents based on relevance metrics or keywords before passing them to the LLM.**
- Randomly remove some documents to reduce system load.

Filtering irrelevant documents ensures the retrieval system focuses on high-quality inputs, improving downstream AI system performance.

---

### 8. A Generative AI Engineer is tasked with coding a simple chain using LangChain to query a customer database. The chain must extract user details based on their email ID. What components are essential?

- ✅ **A retriever to query the database, an LLM to process responses, and a prompt template for query generation.**
- A summarization model to condense query results.
- A rule-based system for hardcoded email queries.
- A classification model to categorize user emails.

Combining retrievers, LLMs, and prompt templates ensures dynamic querying and accurate response generation using LangChain.

---

### 9. A Generative AI Engineer is designing a system to generate meeting summaries. Each summary should include participants, key decisions, and next steps. What prompt structure is most effective? (Choose two)

- ✅ **Add an example: Participants: Alice, Bob; Decisions: Approve budget; Next Steps: Schedule follow-up.**
- Focus on condensing the text into fewer words.
- Use a generic prompt like Summarize the meeting.
- ✅ **Specify fields in the prompt: Provide a summary with participants, key decisions, and next steps.**
- Use a high-temperature setting to generate creative responses.

Specifying fields and providing examples ensure the LLM generates structured meeting summaries aligned with user requirements.

---

### 10. A Generative AI Engineer needs to generate responses to user queries that align with a company’s branding and tone. What approach ensures the LLM aligns with these requirements? (Choose two)

- Adjust the model temperature to achieve desired creativity.
- ✅ **Fine-tune the LLM on company documents and communication guidelines.**
- ✅ **Use metaprompts to explicitly define the desired tone and style.**
- Use a summarization model to simplify responses.
- Train a classification model to label responses by tone.

Combining fine-tuning with metaprompts ensures the LLM generates responses aligned with the company’s tone and branding.

---

### 11. A Generative AI Engineer must design a chain for a chatbot that uses LangChain to process user inputs, retrieve relevant data, and generate conversational responses. What should the chain include? (Choose two)

- Hardcoded templates for fixed responses.
- Pretrained embeddings without retrievers.
- A summarization model to reduce response length.
- ✅ **A classifier to identify user intents for retrieval.**
- ✅ **A prompt template for structuring user inputs, a retriever for relevant data, and an LLM for generating responses.**

Combining prompt templates, retrievers, LLMs, and classifiers ensures a dynamic and context-aware chatbot chain using LangChain.

---

### 12. A Generative AI Engineer must ensure that their deployed RAG application adheres to enterprise compliance standards for data governance. What steps are critical? (Choose two)

- Rely only on LLMs without embedding models.
- ✅ **Use audit logs to track endpoint queries and responses.**
- ✅ **Register all models to Unity Catalog for centralized governance.**
- Skip metadata configuration for deployed models.
- Use public vector stores to reduce costs.

Registering models and using audit logs ensure governance and compliance for enterprise-grade RAG applications.

---

### 13. A Generative AI Engineer must evaluate a deployed RAG application using MLflow. Which evaluation metrics are most relevant?

- Latency and error rates.
- Throughput and cost per query.
- ✅ **Retrieval relevance metrics like precision and recall, and response accuracy metrics like BLEU or ROUGE.**
- Training loss and validation accuracy.

Precision, recall, and BLEU/ROUGE provide a holistic view of retrieval and generation quality in a RAG application.

---

### 14. A Generative AI Engineer is tasked with designing a pipeline to generate personalized travel itineraries based on user preferences. The system must consider budget, preferred destinations, and activity types. What is the best approach? (Choose two)

- ✅ **Use an embedding model to represent user preferences and destinations contextually.**
- Use a summarization model to condense user inputs into concise descriptions.
- Use a rule-based system to match users with predefined itineraries.
- ✅ **Use an LLM to generate complete itineraries tailored to user inputs.**
- Use a classification model to categorize user preferences by travel type.

Combining embeddings and LLMs ensures dynamic and contextually relevant travel itineraries tailored to user needs.

---

### 15. A Generative AI Engineer must extract text from a batch of scanned legal documents in PNG format for training a RAG system. What is the best extraction tool for this task?

- PyPDF2 for reading scanned images.
- pdfplumber for parsing text.
- ✅ **pytesseract for OCR-based text extraction from images.**
- BeautifulSoup for parsing text.

pytesseract enables accurate text extraction from image files, making it ideal for scanned legal documents in PNG format.

---

### 16. A Generative AI Engineer must deploy a multi-user RAG application that analyzes scientific research articles. The application should support concurrent queries while protecting sensitive data. What measures should they implement? (Choose two)

- Skip concurrency testing and deploy with default settings.
- ✅ **Encrypt data at rest and in transit to protect sensitive content.**
- Use shared access keys for user authentication.
- ✅ **Use role-based access control (RBAC) to manage user permissions.**
- Use public vector stores to simplify deployment.

RBAC and encryption ensure secure and compliant multi-user access to sensitive data in RAG applications.

---

### 17. A Generative AI Engineer needs to monitor the performance of a deployed LLM in production. What key metrics should they prioritize?

- ✅ **Latency, throughput, and error rate.**
- Accuracy and F1 score.
- BLEU and perplexity.
- Token usage and input-output length ratios.

Latency, throughput, and error rate provide critical insights into the operational performance of a deployed LLM.

---

### 18. A Generative AI Engineer is building a RAG system to retrieve answers from legal documents. The documents include extraneous information like case timestamps and redundant metadata. What preprocessing strategy should they implement?

- Remove all metadata from the documents during preprocessing.
- ✅ **Filter out irrelevant metadata while retaining case-specific information like case names and citations.**
- Exclude documents with metadata entirely.
- Process all documents without filtering metadata to retain completeness.

Removing only extraneous metadata ensures the RAG system focuses on meaningful content, improving both retrieval and generated answers.

---

### 19. A Generative AI Engineer is tasked with designing a multi-agent system to automate customer queries about order statuses, delivery times, and refunds. Which tools are required to expose agent functions effectively?

- Apply embeddings to represent order details semantically.
- Train a rule-based system to handle each type of query.
- ✅ **Build a prompt template that specifies agent capabilities and provides examples for each function.**
- Use a summarization model to condense customer queries.

Prompt templates provide clear instructions, ensuring the system effectively exposes and utilizes agent functions.

---

### 20. A Generative AI Engineer has provisioned a throughput model serving endpoint for a RAG application. They currently use a microservice to log incoming requests and outgoing responses to a remote server but are looking for a native Databricks feature to perform the same task. Which feature should they use?

- Lakeview
- ✅ **Inference Tables**
- Vector Search
- DBSQL

Inference Tables provide built-in support for tracking request/response logs, making them the ideal replacement for custom microservices.

---

### 21. A Generative AI Engineer must create metaprompts for a language model to prevent generating toxic content in a social media moderation system. What should the metaprompts emphasize?

- Random sampling to reduce repetition.
- Shorter prompts to minimize complexity.
- Response diversity to improve engagement.
- ✅ **Contextual filtering to block any toxic or harmful phrases.**

Metaprompts emphasizing contextual filtering ensure the model avoids generating harmful or toxic content.

---

### 22. A Generative AI Engineer is designing a RAG application that ingests legal documents from multiple public sources. What governance steps should be taken to ensure legal compliance with the data sources?

- ✅ **Verify the licensing terms of each data source and ensure they permit commercial use.**
- Skip verifying licenses for non-commercial applications.
- Assume all public data can be used commercially without verification.
- Use the data without licensing verification but cite the sources in application outputs.

Verifying licensing terms ensures the application complies with legal requirements, avoiding potential liabilities.

---

### 23. A Generative AI Engineer is deploying a RAG application for legal document analysis. The system must support multi-user access with restricted permissions. What access control measures should they implement?

- Use a shared access key for all users.
- Deploy the application without access controls to simplify development.
- Rely on network-level restrictions without authentication.
- ✅ **Use role-based access control (RBAC) to assign permissions based on user roles.**

RBAC ensures secure and controlled multi-user access, meeting the requirements for legal document analysis systems.

---

### 24. A Generative AI Engineer must deploy a Generative AI application that performs real-time language translation. Which factor is most critical for selecting the LLM?

- ✅ **Low latency and high translation accuracy.**
- Large model size.
- Multilingual training data only.
- High token limits.

Prioritizing low latency and accuracy ensures the LLM delivers high-quality translations in real-time, suitable for this application.

---

### 25. When designing a multi-turn chatbot, why is it important to track conversation state across turns?

- To reduce response length by limiting context size.
- To increase randomness in responses.
- ✅ **To ensure context is preserved, leading to coherent and relevant responses.**
- To prioritize speed over context.

Tracking state ensures the chatbot generates contextually aware responses, improving user satisfaction and system effectiveness.

---

### 26. A Generative AI Engineer is tasked with designing a multi-user application where different teams access the same LLM. How can the engineer ensure appropriate access controls?

- Implement temperature settings for user-specific queries.
- ✅ **Assign role-based permissions that control access to specific functionalities of the application.**
- Use encryption to restrict unauthorized actions.
- Enable all users to access the full application.

Role-based permissions provide robust access controls, ensuring that users interact with only the parts of the application appropriate to their needs.

---

### 27. A Generative AI Engineer is evaluating the cost performance of a Databricks-deployed LLM system for a question-answering application. The team needs to ensure scalability for increased traffic. What are the most effective strategies to monitor and optimize costs? (Choose three)

- Fine-tune the LLM to handle higher throughput.
- Increase retrieval batch sizes to improve precision.
- ✅ **Use inference logging to analyze trends in cost and performance under high traffic.**
- ✅ **Monitor token usage per query to identify cost-heavy operations.**
- ✅ **Scale GPU resources dynamically based on demand.**

Monitoring token usage, inference logging, and dynamic GPU scaling ensures cost-effective scalability for high-traffic applications.

---

### 28. A Generative AI Engineer must deploy a Retrieval-Augmented Generation (RAG) application for e-commerce customer support. What are the critical steps to set up this system?

- 1. Fine-tune the LLM on customer reviews → 2. Build a retriever → 3. Generate embeddings → 4. Deploy the system.
- 1. Train a classification model for product categories → 2. Deploy a retriever → 3. Summarize queries → 4. Store outputs.
- ✅ **1. Generate embeddings for product descriptions → 2. Store embeddings in a Vector Search index → 3. Integrate the retriever with an LLM → 4. Deploy an endpoint for the pipeline.**
- 1. Build a retriever → 2. Preprocess customer queries → 3. Deploy a summarizer → 4. Store embeddings in a database.

The correct sequence includes embedding generation, indexing, retrieval integration, and deploying an endpoint to serve customer queries dynamically.

---

### 29. A Generative AI Engineer is tasked with creating an AI pipeline to recommend movies based on user preferences. The recommendations must consider genres, user watch history, and ratings. How should the pipeline be structured?

- Use a classification model to predict user ratings for movies.
- ✅ **Use a retriever for user watch history and an LLM to generate personalized recommendations.**
- Use a summarization model to highlight key points from user preferences.
- Use a rule-based system to match users with movies.

A combination of retrievers and LLMs enables dynamic, context-aware movie recommendations based on user inputs.

---

### 30. A team intends to deploy a code generation model to assist their software developers, ensuring support for multiple programming languages. The primary focus is on maintaining high quality in the generated code.
Which of the Databricks Foundation Model APIs or models available in the Marketplace would be the most suitable choice?

- A. Llama2-70B
- B. BGE-large
- C. MPT-7B
- ✅ **D. CodeLlama-34B**

Correct Answer: D. CodeLlama-34B?
Justification:
CodeLlama-34B is a specialized large language model explicitly fine-tuned for code generation tasks across multiple programming languages. Its design focuses on generating high-quality code, making it particularly suitable for assisting software developers. The model‘s substantial parameter size (34 billion) enhances its ability to understand and generate complex code structures, thereby maintaining a high standard in the generated code. ?ai.meta.com
Supporting Information:
Enhanced Coding Capabilities: CodeLlama-34B has been trained on extensive code-specific datasets, enabling it to generate code and provide natural language explanations about code. This training allows it to perform tasks such as code synthesis, code completion, and debugging assistance effectively. ?huggingface.co
Multi-Language Support: The model supports various programming languages, including Python, Java, C++, Bash, PHP, Typescript, and C#, making it versatile for diverse development environments. ?codecademy.com
Performance: Among the CodeLlama models, the 34B version provides the best results for code generation and development tasks, offering a balance between performance and computational requirements. ?github.com+3codecademy.com+3ai.meta.com+3
Analysis of Other Options:
A. Llama2-70B:
Limitation: While Llama2-70B is a powerful language model, it is a general-purpose model not specifically fine-tuned for code generation tasks. Therefore, it may not perform as effectively in generating high-quality code compared to models like CodeLlama-34B that are specialized for such tasks.? 
B. BGE-large:
Limitation: BGE-large is not primarily designed for code generation. Its architecture and training data are oriented towards different applications, making it less suitable for generating high-quality code across multiple programming languages.? 
C. MPT-7B:
Limitation: MPT-7B is a smaller model with 7 billion parameters and is not specifically optimized for code generation tasks. Its smaller size may limit its ability to generate complex code structures effectively, resulting in lower quality code outputs.? 
Conclusion:
For a team aiming to deploy a code generation model that supports multiple programming languages and maintains high-quality code output, CodeLlama-34B is the most suitable choice. Its specialized training for coding tasks, multi-language support, and superior performance in code generation make it well-suited to assist software developers effectively.? 
References:
Introducing Code Llama, a state-of-the-art large language model for coding
Code Llama on Hugging Face
How To Use Code Llama | Codecademy
