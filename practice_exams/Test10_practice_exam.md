### 1. You are designing a Generative AI agent that interacts with APIs to retrieve account info, update settings, and send notifications. To ensure the agent uses the APIs correctly, you need to build a prompt template exposing the available functions. Which approach best exposes these functions to the model?

- ✅ **Expose all available functions but include short descriptions or signatures of each function for clarity.**
- Only expose the functions that are most frequently used, to reduce prompt size and improve performance.
- Expose the function names but leave out their descriptions to save space in the prompt.
- List all available functions in the prompt and provide detailed descriptions of each.

A. Expose all available functions with concise descriptions or signatures.

---

### 2. You are tasked with implementing data masking for a multi-tiered access control system. Users in different roles should have varying visibility into sensitive data fields (e.g., full, partial, or full masking). Performance must remain stable for critical queries against the data lakehouse. Which strategy best accomplishes this?

- Use dynamic SQL to create role-based queries that return different masked data versions for each role.
- Implement a single masking policy at the table level, masking all sensitive data for all roles equally.
- ✅ **Use role-based masking policies via Unity Catalog to apply specific masking levels based on user roles.**
- Store fully masked, partially masked, and unmasked versions of each table and assign access by role.

C. Unity Catalog role-based masking policies apply specific masking per role.

---

### 3. Which of the following is the correct set of resources needed to serve features for a Retrieval-Augmented Generation (RAG) application in Databricks?

- A distributed GPU cluster for real-time inference, raw text data, a feature store for storing models, and a cloud-based SQL server for querying embeddings.
- A trained language model, a vector search index, a pre-built feature store, and a GPU cluster for real-time inference.
- ✅ **A vector search index, a feature store to manage embeddings, pre-processed text data, and a scalable inference cluster.**
- A fine-tuned LLM, a feature store for text embeddings, a SQL database for storing queries, and a distributed training cluster.

C. Vector index + feature store + pre-processed text + scalable inference.

---

### 4. You developed a Retrieval-Augmented Generation (RAG) system for a legal question-answering platform that retrieves relevant documents and generates responses. After testing with 100 queries, the system retrieves 50 documents per query, with an average of 30 relevant ones, while the ground truth considers 40 relevant per query. Which pair of metrics best evaluates your retrieval system’s performance?

- ✅ **Precision and Recall**
- Precision and F1 Score
- Accuracy and Recall
- F1 Score and Accuracy

A. Precision (30/50) and Recall (30/40) are key retrieval metrics.

---

### 5. In the context of a RAG application, a company discovers that a portion of the data feeding the application contains offensive language and biased statements. Which of the following approaches best addresses governance concerns by mitigating this problematic content without significantly altering the factual integrity of the dataset?

- ✅ **Implement content moderation tools that flag and transform inappropriate data into non-offensive, neutral text.**
- Use fine-tuning to train the model on acceptable content, ensuring the problematic data is ignored.
- Apply NLP techniques to summarize the offensive parts of the text and remove those sections.
- Exclude the entire dataset from the model training to ensure no inappropriate data is used.

A. Moderation tools transform problematic text while preserving facts.

---

### 6. You are building an application that interacts with a generative AI model to extract and return structured data about people from text input. The response should be formatted in JSON, with the following keys: “name”, “age”, “occupation”, and “location”. Which of the following prompts would best instruct the model to output this information in the required format? (Select two)

- “Provide a JSON-formatted summary of the following text, extracting relevant details about the person’s name, age, occupation, and location. Ensure the information is complete and accurate.“
- “From the given text, summarize the person’s details, focusing on their name, age, occupation, and location. Output this information in a readable format.“
- ✅ **“Using the provided text, return the person’s name, age, occupation, and location in a JSON structure, ensuring the fields are named ‘name’, ‘age’, ‘occupation’, and ‘location’.“**
- “List the person’s name, age, occupation, and location in a well-organized paragraph. Include any relevant details about their background.“
- ✅ **“Extract the person’s name, age, occupation, and location from the text and return them in JSON format with the keys ‘name’, ‘age’, ‘occupation’, and ‘location’.“**

C and E explicitly demand JSON with exact key names.

---

### 7. You are developing a Retrieval-Augmented Generation (RAG) application that helps with legal document review. The RAG system needs to be fed high-quality and relevant documents to ensure it can answer legal questions effectively. Which of the following types of documents should you prioritize to ensure your RAG application delivers accurate legal insights?

- ✅ **Case law databases and court rulings**
- Internal company HR policies
- General news articles covering legal matters
- Personal blogs written by attorneys

A. Primary legal sources provide authoritative precedents.

---

### 8. You are developing a customer support chatbot that needs to handle a wide variety of inquiries across multiple domains, with a primary focus on generating conversational and natural-sounding responses. Which of the following LLM attributes should you prioritize when selecting the model for this task?

- A model with a low latency and limited vocabulary
- A model optimized for extractive question answering
- Fine-tuning on a small, specialized dataset
- ✅ **Large model size with extensive pre-training on diverse text data**

D. Large, broadly pre-trained models excel at conversational generation.

---

### 9. You are building a recommendation engine for a news platform with articles of varying lengths, from short blogs to long investigative pieces. The application must optimize the embedding model for query performance and space efficiency. What is the most appropriate embedding context length to achieve both query performance and space efficiency?

- 8192 tokens
- 2048 tokens
- ✅ **512 tokens**
- 128 tokens

C. 512 tokens balances context capture and efficiency for news.

---

### 10. You are building a Databricks application using a generative AI model to summarize financial reports. The quarterly earnings report includes key metrics like revenue, expenses, profit margins, and projections. You want a concise yet comprehensive summary. Which prompt structure will best achieve this?

- Prompt: “What are the key highlights from the company’s latest quarterly earnings report?”
- Prompt: “Explain the company’s overall performance in the last quarter.”
- ✅ **Prompt: “Summarize the quarterly earnings report in two sentences, focusing on the company’s financial performance.”**
- Prompt: “Give a high-level overview of the company’s performance.”

C. Two-sentence, finance-focused instruction yields concise, relevant summaries.
