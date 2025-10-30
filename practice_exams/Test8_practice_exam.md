### 1. You are tasked with managing models in Unity Catalog using MLflow. You need to register a new version of an existing model and are unsure how versioning and lifecycle management work with Unity Catalog and MLflow. What is the correct approach to register and manage the model version?

- ✅ **Call mlflow.log_model() with the same model name and Unity Catalog URI to automatically create a new version.**
- Directly overwrite the existing model in Unity Catalog using the Databricks Model Registry UI.
- Call mlflow.update_model_version() and provide the new model data and the existing version number.
- Use mlflow.register_model() to automatically create a new version if the model name already exists in Unity Catalog.

A. Call mlflow.log_model() with the same model name and Unity Catalog URI to automatically create a new version.
When using MLflow with Unity Catalog, the mlflow.log_model() function logs the model artifacts within an MLflow run. To register a new version of an existing model in Unity Catalog, you would then call mlflow.register_model() with the model_uri pointing to the newly logged model and the existing model name.

---

### 2. A tech support company wants to build a chatbot that answers customer questions and troubleshoots issues using a knowledge base of technical documentation. The chatbot should generate natural responses and retrieve relevant information when needed. What model architecture would best fit this need?

- A sequence-to-sequence model (e.g., Transformer-based) fine-tuned for text classification
- A text classification model trained on customer service tickets
- A machine translation model adapted for customer service language
- ✅ **A question-answering model with a retriever and generator architecture (e.g., RAG)**

D. A question-answering model with a retriever and generator architecture (e.g., RAG)
This architecture is specifically designed to address the requirements of a chatbot that needs to answer questions based on a knowledge base and generate natural-sounding responses.

---

### 3. A legal team needs to extract textual content from thousands of PDF files containing legal contracts. They want to automate this process using Python. Which Python package is most appropriate for extracting text from PDF documents?

- Tika
- ✅ **PyPDF2**
- openpyxl
- docx2txt

B. PyPDF2 is a Python library specifically designed for working with PDF files.

---

### 4. You are optimizing a cost-concerned RAG system on Databricks with large-scale LLM inference for many users. You need cost-saving techniques while maintaining high request volume handling. Databricks features, including feature stores, are in use. Which strategy most effectively reduces LLM costs in this RAG setup?

- Increase the timeout for LLM responses so that fewer resources are used during peak traffic.
- Enable cluster auto-termination in Databricks to shut down the LLM inference cluster after a set period of inactivity.
- Utilize Databricks FileStore to store and cache frequently used responses to prevent repeated LLM calls.
- ✅ **Store retrieved documents in Databricks Feature Store, which will allow re-use of document embeddings, reducing the number of calls to the LLM.**

D. Store retrieved documents in Databricks Feature Store to re-use embeddings and reduce calls.

---

### 5. You are working on a Retrieval-Augmented Generation (RAG) application that sources data from a large public dataset. However, some of the textual data contains inappropriate or biased content. To ensure the model generates responses with proper governance, what is the best strategy for mitigating problematic text in the data source feeding the application?

- Use a data augmentation technique to overwrite biased or inappropriate content with neutral alternatives.
- Manually review and remove all problematic data before feeding it into the application.
- Apply prompt engineering techniques to avoid triggering problematic topics during inference.
- ✅ **Implement pre-trained AI filters to automatically flag and censor problematic content before it’s used by the model.**

D. Pre-trained AI filters automatically flag and filter problematic content at scale.

---

### 6. Your company operates a subscription-based streaming service and is looking to improve its recommendation system. The goal is to recommend content (e.g., movies, TV shows) to users based on their past viewing history, the preferences of similar users, and emerging trends in content consumption. Which type of AI model should you select to achieve this? (Select two)

- BERT-based Model for Natural Language Processing (NLP)
- Topic Modeling for Content Discovery
- Generative Adversarial Network (GAN)
- ✅ **Collaborative Filtering Model**
- ✅ **Content-Based Filtering Model**

D and E are best for personalized recommendations based on behavior and item features.

---

### 7. You are developing an AI-powered knowledge base for a global research organization to generate detailed technical reports from user queries. Evaluation metrics include perplexity, throughput, and memory usage. The LLM must provide accurate, contextually relevant information while minimizing resource consumption. Which LLM configuration best balances high accuracy, moderate throughput, and efficient memory usage?

- A 1-billion parameter model with high perplexity, low memory usage, and very high throughput.
- A 6-billion parameter model with moderate perplexity, low memory usage, and high throughput.
- ✅ **A 13-billion parameter model with low perplexity, moderate memory usage, and moderate throughput.**
- A 30-billion parameter model with very low perplexity but high memory usage and low throughput.

C. 13B parameters balances low perplexity with moderate memory and throughput.

---

### 8. You are working on a text classification problem to predict customer sentiment (positive, neutral, or negative) from product reviews. The evaluation metrics for four models are:
Model A: Accuracy: 88%, Precision: 91%, Recall: 85%, ROC-AUC: 0.95
Model B: Accuracy: 90%, Precision: 87%, Recall: 90%, ROC-AUC: 0.88
Model C: Accuracy: 85%, Precision: 86%, Recall: 89%, ROC-AUC: 0.90
Model D: Accuracy: 89%, Precision: 89%, Recall: 88%, ROC-AUC: 0.92
Which model is the best choice based on the metrics provided?

- Model A
- ✅ **Model D**
- Model C
- Model B

B. Model D provides balanced high performance across metrics.

---

### 9. Design an LLM-enabled solution on Databricks for an e-commerce company to enhance customer support, handling queries like product inquiries, order status, and refunds. The solution should provide real-time responses, manage large data volumes (e.g., chat logs, product catalogs), and prioritize cost efficiency and scalability. Which approach demonstrates effective problem decomposition for design and implementation?

- Implement an LLM model first, without breaking the tasks down, and optimize after deployment based on feedback and usage data.
- Start by building a single LLM model that can handle all types of customer queries, then focus on integrating data sources.
- ✅ **Decompose the problem by identifying key sub-tasks: (1) query categorization, (2) LLM fine-tuning for each query type, (3) real-time response handling, (4) data source integration, and (5) cost and performance optimization.**
- Use a pre-trained LLM as-is without further decomposition, and focus on scaling the model for large volumes of data.

C. Decomposition into key sub-tasks ensures structured implementation.

---

### 10. You are managing access to a deployed machine learning model in Databricks, and the security team requires that only a specific set of users can query the model’s serving endpoint. What is the most appropriate approach to meet this requirement?

- ✅ **Set model access controls using Databricks’ workspace role-based access control (RBAC)**
- Use Databricks Admin Console to enable fine-grained access control at the notebook level
- Create a network security group and assign users to the group
- Enable Databricks Cluster ACLs and assign users to specific clusters

A. Workspace RBAC grants “Can Query” permission on the serving endpoint to authorized users.
