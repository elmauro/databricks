### 1. A healthcare organization uses a generative model to create synthetic medical data while preserving patient privacy. To meet performance goals, they need a masking technique that protects sensitive information like patient IDs, but still retains enough detail for tasks like diagnosis prediction. What is the most suitable masking approach for this scenario?

- Differential Privacy-Based Masking
- Random Noise Injection Masking
- Full Redaction Masking
- ✅ **Partial Token Masking with Context Preservation**

D. Partial Token Masking with Context Preservation
This approach strikes a balance between privacy protection and data utility for diagnosis prediction.

---

### 2. You are tasked with building a generative AI system that can extract user intent (e.g., refund request, order status inquiry, product information) from their input and dynamically modify the prompt to reflect this intent. What is the most appropriate method to handle this?

- Feed the user’s input into the model without processing, and let the model infer both the intent and context.
- Use Named Entity Recognition (NER) models to extract keywords and directly feed them into the prompt.
- ✅ **Apply an intent classification model to categorize the user’s input, then select a prompt template that aligns with the identified intent.**
- Implement a rule-based system to identify intent keywords and adjust the prompt accordingly.

C. Apply an intent classification model and select a matching prompt template.

---

### 3. You are developing a Retrieval-Augmented Generation (RAG) app that integrates LLMs with external knowledge sources. Which source document offers the most relevant and high-quality knowledge for answering technical questions on cloud architecture?

- User forums and discussion boards (e.g., Stack Overflow, Reddit)
- ✅ **Official documentation from cloud service providers (e.g., AWS, Azure, GCP)**
- Internal project documentation from a non-cloud-related IT project
- Wikipedia articles on cloud computing

B. Official documentation is the most reliable and up-to-date source.

---

### 4. When designing a prompt template for a customer support chatbot powered by a Generative AI model, which of the following best practices ensures the generated responses remain accurate and relevant to the user’s query?

- Avoid providing context in the prompt to keep it concise.
- Use multiple unrelated examples in the template to increase versatility.
- Use ambiguous placeholders to allow the model to infer the user’s intent.
- ✅ **Define placeholders with specific labels and provide clear instructions for the model.**

D. Clear placeholders and instructions guide accurate, relevant outputs.

---

### 5. You are building a PyFunc model in Databricks for named entity recognition (NER) on raw text. The text needs tokenization and cleaning before model input, and the output must be converted into human-readable named entities. Which of the following best describes the correct structure for your PyFunc model with pre- and post-processing?

- Add the pre-processing logic as part of the model training pipeline and post-processing in a separate Python script after the model prediction.
- ✅ **Create separate functions for pre-processing (tokenization) and post-processing (entity conversion), and invoke them in the script before and after calling predict().**
- Implement both tokenization and entity conversion in the predict() function.
- Handle both pre-processing and post-processing in the load_context() function.

B. Separate pre- and post-processing functions; call them around predict().

---

### 6. You are building a Named Entity Recognition (NER) application on Databricks to extract entities (names, organizations, dates) from text and store them in a Delta table. The NER model is a Python function, and it should be integrated into a pipeline that triggers automatically when a file is uploaded to Azure Data Lake Storage (ADLS). The results must be queryable in Delta Lake. How would you deploy this pipeline?

- Deploy the NER model as a REST API endpoint on Azure Kubernetes Service (AKS), trigger the API when new data is uploaded, and store the results in a SQL database.
- Set up an Azure Event Grid to trigger the NER function, process the file, and store the entities in a Delta table.
- ✅ **Use Databricks Delta Live Tables to define a pipeline that reads from the ADLS directory, runs the NER function, and stores the result in the Delta table.**
- Create a PySpark job that monitors the ADLS directory, extracts entities using the NER function, and writes them to a Delta table.

C. Delta Live Tables provides a managed, declarative pipeline with Delta output.

---

### 7. You are building a semantic search engine with an LLM to query unstructured documents using vector search. After embedding the documents into vectors, you need to create a vector search index in Databricks for fast querying. Which step is correct for this in Databricks?

- ✅ **Load the vector embeddings into a Delta Lake table and use Databricks’s built-in support for vector search by creating an index with the CREATE VECTOR INDEX command.**
- Directly create an index using MLlib’s built-in feature extraction library, which supports vector searches natively.
- Use the mlflow.create_vector_index() function to directly create a vector search index on the embeddings.
- Run dbutils.vector.create_index() after loading the vectorized data into a DataFrame.

A. Store embeddings in Delta and use CREATE VECTOR INDEX for indexing.

---

### 8. You are developing a document classification application on Databricks to categorize legal documents (contracts, case law, statutes) using pretrained models from a model hub. Your task is to assemble and deploy the application with minimal additional training. What is the most efficient workflow?

- Build a custom model from scratch using a small dataset of legal documents.
- Use a clustering algorithm to automatically classify documents without any supervised model training.
- Fine-tune a pretrained language model for text generation on your dataset before deploying.
- ✅ **Use a pretrained model specifically designed for document classification and deploy it using Databricks Model Serving.**

D. Leverage a pretrained classifier and serve via Model Serving.

---

### 9. You are building an AI sentiment analysis app in Databricks using an LLM to classify customer reviews as positive or negative. You notice inconsistent results with different input formats. Which prompt format is most likely to generate the most accurate sentiment classification?

- Prompt: “The customer says: ‘[REVIEW TEXT]’. Is the sentiment positive?”
- ✅ **Prompt: “Classify the following review as either Positive or Negative: [REVIEW TEXT].”**
- Prompt: “[REVIEW TEXT]. What is the mood of the speaker?”
- Prompt: “Analyze the sentiment of this text: [REVIEW TEXT].”

B. Explicitly specifying the task and labels improves accuracy.

---

### 10. You are developing a search system that needs to embed and search legal documents, some of which can exceed 10,000 words in length. The system must optimize for both accuracy and latency, ensuring fast responses to user queries. Given the constraints, what is the most appropriate context length for the embedding model? Which context length is best suited for this application?

- 128 tokens
- ✅ **4096 tokens**
- 512 tokens
- 256 tokens

B. 4096 tokens balances context coverage with practical latency.
