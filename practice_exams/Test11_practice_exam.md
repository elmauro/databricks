### 1. You are deploying a predictive maintenance model on Databricks to predict machine failure from sensor data. The model is in a pyfunc environment, and post-processing logic is needed for actionable insights. Which components are part of the post-processing stage to convert the model output into actionable information for operators? (Select three)

- ✅ **Generating automated alerts based on the failure probability exceeding a certain threshold**
- ✅ **Generating a diagnostic report explaining which sensors contributed most to the predicted failure**
- ✅ **Thresholding the predicted failure probabilities into binary labels (e.g., fail/no-fail)**
- Returning raw sensor data alongside the model’s prediction
- Re-training the model with new data

Correct Answer: A. Generating automated alerts based on the failure probability exceeding a certain threshold
Correct Answer: B. Generating a diagnostic report explaining which sensors contributed most to the predicted failure
Correct Answer: C. Thresholding the predicted failure probabilities into binary labels (e.g., fail/no-fail)

---

### 2. You are developing a Retrieval-Augmented Generation (RAG) application that uses documents from various unstructured data sources. Some of these documents contain biased or harmful language that could negatively impact the generative model’s output. Which of the following techniques would be the most appropriate to mitigate problematic text in the data source?

- ✅ **Fine-tune the Foundation Model with a bias detection model that filters harmful language dynamically during text generation.**
- Apply word embeddings to transform the text into vectors and exclude outlier embeddings to filter out problematic language.
- Implement a rule-based system to flag and remove all text containing potentially problematic words before feeding the data to the RAG system.
- Pre-process the text using Named Entity Recognition (NER) to identify and mask sensitive entities before passing the text to the RAG system.

Correct Answer: A. Fine-tune the Foundation Model with a bias detection model that filters harmful language dynamically during text generation

---

### 3. You have deployed a Retrieval-Augmented Generation (RAG) model as an endpoint in Databricks, but the API returns errors when retrieving documents. Investigation reveals that the documents were not indexed properly in the vector store. How should you troubleshoot and fix the issue?

- Scale the API horizontally to accommodate more requests and avoid bottlenecks.
- ✅ **Ensure that the vector database is populated with embeddings of the documents before deploying the API.**
- Restart the API server to ensure it re-establishes a connection with the vector database.
- Test the API without using a vector database, focusing only on the language model’s response.

Correct Answer: B. Ensure that the vector database is populated with embeddings of the documents before deploying the API

---

### 4. You are deploying a Retrieval-Augmented Generation (RAG) application on Databricks that allows users to submit embedded queries, retrieve relevant documents, and pass them to a generative model for responses. To ensure seamless deployment and future integration with other teams, which essential components are required for this RAG application?

- Pre-trained language model, document retriever, tokenizer, SQL query generator, dependencies, and input pipeline.
- Language model, input format parser, retriever, output formatter, embedding index, and model signature.
- Retriever, vectorizer, generative model, dataset schema, hyperparameter configuration, and API gateway.
- ✅ **Embedding model, retriever, generative model, dependencies, model signature, and input examples.**

Correct Answer: D. Embedding model, retriever, generative model, dependencies, model signature, and input examples

---

### 5. You are developing a prompt template for a generative AI-powered chatbot designed for technical support. The chatbot should offer functionalities like fetching product docs, providing troubleshooting steps, and generating support tickets, while remaining user-friendly for both technical and non-technical users. Which prompt template best exposes these functions in an accessible way without overwhelming users?

- Please enter your issue description:
{description}
Available actions:
1. Fetch Documentation: (Yes/No)
2. Troubleshooting Guide: (Yes/No)
3. Create Support Ticket: (Yes/No)
- ✅ **Describe your issue:
{description}
We offer the following actions based on your needs:
1. Fetch Documentation: {fetch_docs()}
2. Troubleshooting Guide: {troubleshoot()}
3. Generate Support Ticket: {create_ticket()}
You can select one or more actions.**
- Describe your issue in detail:
Issue: {description}
Based on your description, we will:
1. Fetch relevant documentation.
2. Provide troubleshooting steps.
3. Create a support ticket if the issue is unresolved.
- Provide a description of your issue:
{description}
Functions available:
- Fetch documentation: {fetch_docs()}
- Troubleshooting guide: {troubleshoot()}
- Create support ticket: {create_ticket()}

Correct Answer: B. “Describe your issue: {description} … You can select one or more actions.”

---

### 6. You are deploying an LLM for a customer service AI system handling high volumes of real-time queries. The main concerns are latency, accuracy, and resource efficiency. Which metrics should be prioritized to ensure optimal performance?

- Token Perplexity
- Number of Layers in the LLM
- ✅ **Inference Latency**
- Model Training Loss

Correct Answer: C. Inference Latency

---

### 7. You are tasked to design a prompt for a generative AI model to extract key-value pairs from a product description document. The output should be in JSON format with keys like “price,” “weight,” and “dimensions.” Which prompt best achieves this?

- “Extract the product details and format them as bullet points.”
- “Extract the product specifications and list them as key-value pairs.”
- “List all product details in the format: key = specification, value = description.”
- ✅ **“Please extract the product specifications and output them in a JSON format, where the keys represent the specification type (e.g., price, weight) and the values represent the corresponding data.”**

Correct Answer: D. JSON-formatted extraction prompt

---

### 8. You are tasked with developing a chatbot on Databricks that uses an LLM to generate human-like responses. The chatbot must handle many concurrent users, dynamically adjust resource allocation based on workload, and use GPU resources efficiently to reduce inference time. Which strategy is best for deploying and scaling this generative AI application?

- Deploy the chatbot on a Databricks all-purpose cluster with auto-scaling enabled to adjust resources based on demand.
- Deploy the model as an interactive Python script within a Databricks Notebook, allowing users to directly interact with it.
- ✅ **Leverage Databricks Model Serving with GPU-backed clusters to handle concurrent users and ensure efficient resource usage.**
- Use a Databricks job cluster, schedule it using the Jobs API, and scale based on CPU usage to handle multiple users.

Correct Answer: C. Databricks Model Serving with GPU-backed clusters

---

### 9. You are working on a generative AI-powered chatbot that interacts with your company’s data lake through SQL queries. To protect against malicious user inputs like SQL injection attacks, which guardrail technique should you implement to secure your AI application without degrading performance?

- Allow users to input raw SQL queries but monitor query execution logs for suspicious activity.
- Develop a whitelist of allowed SQL commands and block all other queries from being executed.
- ✅ **Use parameterized SQL queries to ensure that user inputs are handled as parameters rather than executable code.**
- Automatically clean all user inputs by removing special characters and keywords such as “SELECT” and “DROP.”

Correct Answer: C. Use parameterized SQL queries

---

### 10. As a Generative AI Engineer using a GPT-based model for text summarization in a customer support app, users have reported degraded summary quality. Suspecting input data changes may cause performance drift, your goal is to monitor and detect data or concept drift. Which action would best help detect drift in the model’s performance?

- Periodically re-train the model on the same dataset used for initial training
- Apply manual evaluation of summaries by domain experts on random samples from production
- Track the model’s loss function and compare it with the loss during initial training
- ✅ **Monitor the distribution of input data over time using statistical tests like the Kolmogorov-Smirnov test**

Correct Answer: D. Monitor input distribution with statistical tests
