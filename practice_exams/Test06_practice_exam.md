### 1. A Generative AI Engineer has successfully trained a large language model (LLM) on Databricks, and it is now ready for deployment.
Which of the following steps outlines the easiest process for deploying a model on Databricks?

- A. Log the model as a pickle object, upload the object to Unity Catalog Volume, register it in Unity Catalog using MLflow, and start a serving endpoint.
- ✅ **B. Log the model using MLflow during the training phase, register the model directly in Unity Catalog via the MLflow API, and start a serving endpoint.**
- C. Save the model and its dependencies in a local directory, build a Docker image, and run the Docker container.
- D. Wrap the LLM’s prediction function in a Flask application and serve it using Gunicorn.

Correct Answer:
B. Log the model using MLflow during the training phase, register the model directly in Unity Catalog via the MLflow API, and start a serving endpoint.
Justification for the Correct Answer (B)
The easiest way to deploy an LLM on Databricks is by using MLflow and Databricks Model Serving. Databricks provides native integration with MLflow, which streamlines model tracking, registration, and deployment.

---

### 2. A Generative AI Engineer is building a Retrieval-Augmented Generation (RAG) application to help users find answers about technical regulations for a new sport they are learning. What is the correct sequence of steps to design, build, and deploy this RAG application?

- A. Ingest documents from a source → Index the documents and store them in a vector search → Users submit queries to the LLM → LLM retrieves relevant documents → Evaluate the model → LLM generates a response → Deploy the application using Model Serving.
- ✅ **B. Ingest documents from a source → Index the documents and store them in a vector search → Users submit queries to the LLM → LLM retrieves relevant documents → LLM generates a response → Evaluate the model → Deploy the application using Model Serving.**
- C. Ingest documents from a source → Index the documents and store them in a vector search → Evaluate the model → Deploy the application using Model Serving.
- D. Users submit queries to the LLM → Ingest documents from a source → Index the documents and store them in a vector search → LLM retrieves relevant documents → LLM generates a response → Evaluate the model → Deploy the application using Model Serving.

Correct Answer:
Option B: Ingest documents → Index them in vector search → Users submit queries → LLM retrieves relevant documents → LLM generates a response → Evaluate → Deploy.

---

### 3. A Generative AI Engineer is building a healthcare-focused chatbot for patients. If a patient’s inquiry is not an emergency, the chatbot should gather more information to relay to the doctor’s office and recommend relevant pre-approved medical articles. In the case of urgent inquiries, the chatbot should instruct the patient to contact their local emergency services.
Given the following user input:
“I have been experiencing severe headaches and dizziness for the past two days.”
Which response should the chatbot provide?

- A. Here are some relevant articles for you to browse. Feel free to ask questions after reading them.
- ✅ **B. Please call your local emergency services.**
- C. Headaches can be challenging. I hope you feel better soon!
- D. Please provide your age, recent activities, and any other symptoms you’ve experienced along with your headaches and dizziness.Please contact your local emergency services.

Correct Answer:
B. Please call your local emergency services.

---

### 4. A Generative AI Engineer has developed a Retrieval-Augmented Generation (RAG) application to find answers for questions about a series of fantasy novels posted on the author’s online forum. The text from the novels is divided into chunks, embedded into a vector store with metadata (e.g., page number, chapter number, and book title), retrieved in response to user queries, and passed to a language model for generating answers. Initially, the engineer relied on intuition to select the chunking strategy and related configurations, but now wants to optimize these choices using a more systematic approach.
Which TWO strategies should the Generative AI Engineer adopt to refine their chunking strategy and parameters? (Choose two.)

- A. Try different embedding models and compare their performance.
- B. Implement a query classifier that predicts which book is most likely to contain the answer and use it to filter retrieval results.
- ✅ **C. Select an appropriate evaluation metric (e.g., recall or NDCG) and experiment with variations in the chunking strategy, such as splitting by paragraphs or chapters, to identify the best-performing approach.**
- ✅ **D. Provide the LLM with known questions and their ideal answers, then ask the model to suggest the optimal token count. Use summary statistics (e.g., mean, median) of these token counts to determine the best chunk size.**
- ✅ **E. Develop a metric where the LLM acts as a judge, scoring how well previous questions are answered by the retrieved chunks. Adjust the chunking parameters based on the results of this metric.**

The two best strategies are C and E as described in the rationale.

---

### 5. A Generative AI Engineer at an electronics company has deployed a RAG (Retrieval-Augmented Generation) application that allows customers to ask questions about the company’s products. However, users have reported that the responses sometimes provide information about irrelevant products.
What should the engineer do to improve the relevance of the responses?

- ✅ **A. Evaluate the quality of the context being retrieved.**
- B. Implement caching for commonly asked questions.
- C. Switch to a different LLM to enhance the response generation.
- D. Use a different algorithm for semantic similarity search.

Correct Answer:
Option A: Evaluate the quality of the context being retrieved.

---

### 6. A Generative AI Engineer is developing a live sports commentary platform powered by a language model. This platform delivers real-time updates and AI-generated analyses for users who prefer live summaries over reading outdated news articles.
Which tool would enable the platform to access real-time game data for generating up-to-date analyses based on the most recent scores?

- A. DatabricksIQ
- B. Foundation Model APIs
- ✅ **C. Feature Serving**
- D. AutoML

Correct Answer:
Option C: Feature Serving

---

### 7. A Generative AI Engineer is developing an application that uses a language model. The documents for the retrieval system have been divided into chunks, each with a maximum of 512 tokens. Since the focus for this application is on reducing cost and latency rather than maximizing quality, the engineer needs to select an appropriate context length from several available options.
Which option best meets these requirements?

- A. Context length of 514; the smallest model size is 0.44GB, with an embedding dimension of 768.
- B. Context length of 2048; the smallest model size is 11GB, with an embedding dimension of 2560.
- C. Context length of 32768; the smallest model size is 14GB, with an embedding dimension of 4096.
- ✅ **D. Context length of 512; the smallest model size is 0.13GB, with an embedding dimension of 384.**

Correct Answer:
Option D

---

### 8. A Generative AI Engineer has just deployed a language model application at a digital marketing company to help handle customer service inquiries. What is the most important metric to monitor for this customer service LLM application once it’s running in production?

- ✅ **A. The number of customer inquiries processed within a given time frame.**
- B. The amount of energy consumed per query.
- C. The final perplexity score from the model’s training phase.
- D. The HuggingFace leaderboard rankings for the base language model.

Correct Answer:
Option A

---

### 9. A Generative AI Engineer is designing a system to recommend the most suitable employee for newly defined projects. The employee is selected from a large pool of team members. The selection needs to consider the employee’s availability during the project timeline and how closely their profile aligns with the project’s requirements. Both the employee profiles and project scopes are composed of unstructured text.
What approach should the engineer take to design this system?

- A. Develop a tool that checks team member availability based on project dates. Store the project descriptions in a vector database and retrieve the best-matched team member profiles by comparing them to the project scope.
- B. Create a tool to track team member availability according to project timelines and build another tool that uses an LLM to pull out key terms from the project descriptions. Then, go through the available team member profiles and match keywords to find the most suitable employee.
- C. Build a tool to identify available team members based on project dates, and another tool to calculate a similarity score between each team member’s profile and the project description. Go through all the team members and rank them by score to select the best match.
- ✅ **D. Create a tool that finds available team members for the given project dates. Embed team member profiles in a vector store and use the project description to search and filter for the best-matched team members who are available.**

Correct Answer:
Option D

---

### 10. A Generative AI Engineer is tasked with designing an LLM-based application that fulfills a business requirement: answering employee HR-related questions by referencing HR PDF documentation.
Which set of high-level tasks should the engineer‘s system perform?

- A. Compute averaged embeddings for each HR document, compare the embeddings to the user‘s query to identify the best document, and then pass the document along with the query into an LLM with a large context window to generate a response.
- B. Use an LLM to summarize the HR documents, then provide the summaries along with the user’s query to an LLM with a large context window to generate a reply.
- C. Build an interaction matrix using historical employee questions and HR documents. Apply ALS to factorize the matrix and create embeddings. For new queries, calculate embeddings and use them to retrieve the best HR documentation, then use an LLM to generate a response.
- ✅ **D. Break the HR documentation into chunks and store them in a vector database. Use the employee‘s question to retrieve the most relevant chunks, and use the LLM to generate a response based on the retrieved documentation.**

Correct Answer:
Option D

---

### 11. You are developing a knowledge retrieval system for customer support, using a large language model to answer technical questions from a structured database of customer queries and solutions. After evaluation, you find that small chunks miss relevant context, while large chunks include irrelevant information, reducing precision. Which chunking strategy is most likely to improve retrieval precision while maintaining context?

- Chunk by token length with dynamic overlap based on query complexity.
- Chunk by sentence length with no overlap.
- ✅ **Chunk by section or topic with overlap between chunks.**
- Do not chunk the data, and use whole documents for retrieval.

C. Chunk by section or topic with overlap between chunks.
This strategy aligns chunks with the semantic structure of the knowledge base.

---

### 12. You are tasked with deploying a retrieval-augmented generation (RAG) system using a large language model (LLM) on Databricks. The key focus is to reduce infrastructure costs while maintaining performance. Which of the following Databricks features would most effectively help you control costs for this application?

- Structured Streaming
- Delta Caching
- ✅ **Auto-scaling clusters**
- Databricks SQL Endpoint

C. Auto-scaling clusters.
Auto-scaling clusters in Databricks automatically adjust the number of worker nodes based on the workload demands.
