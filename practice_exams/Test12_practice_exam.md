### 1. In the context of evaluating a Retrieval-Augmented Generation (RAG) model using MLflow, which of the following metrics is most appropriate for assessing the model‘s retrieval component?

- Cross-Entropy Loss
- ✅ **Precision@K**
- BLEU score
- Perplexity

B. Precision@K is a standard information retrieval metric that evaluates the quality of the top-K retrieved documents. In the context of a RAG model‘s retrieval component, Precision@K measures how many of the top-K documents retrieved by the system are relevant to the given query. A high Precision@K indicates that the retrieval component is effectively fetching relevant context to augment the generation process. This directly assesses the accuracy and quality of the retrieval step, which is crucial for the overall performance of the RAG model.

---

### 2. You are developing a customer service chatbot using a large language model (LLM) in Databricks. The baseline model generates formal, fact-based responses, but you want the chatbot to respond more empathetically and conversationally when handling complaints. How should you modify the prompt to adjust the tone?
Your baseline prompt is:
“Analyze the customer’s complaint and provide a solution”

- ✅ **“Analyze the customer’s complaint and offer a solution in a friendly and empathetic tone, acknowledging their feelings.”**
- “Give a technical solution to the customer’s issue in detail, ensuring accuracy.”
- “Provide a concise solution to the customer’s issue, focusing only on facts.”
- “Respond to the customer by repeating their concern and providing a brief technical fix.”

A. This prompt directly instructs the LLM to adopt the desired tone by explicitly mentioning “friendly and empathetic“ and the need to acknowledge the customer‘s feelings. This provides clear guidance to the model on how to adjust its response style beyond just providing a factual solution.

---

### 3. You are optimizing a cost-effective Retrieval-Augmented Generation (RAG) system on Databricks, where large-scale LLM inference for many users has become costly. You aim to reduce costs while maintaining high request volumes using Databricks-native capabilities, including feature stores. Which strategy would most effectively lower LLM usage costs in your Databricks RAG application?

- ✅ **Store retrieved documents in Databricks Feature Store, which will allow re-use of document embeddings, reducing the number of calls to the LLM.**
- Utilize Databricks FileStore to store and cache frequently used responses to prevent repeated LLM calls.
- Increase the timeout for LLM responses so that fewer resources are used during peak traffic.
- Enable cluster auto-termination in Databricks to shut down the LLM inference cluster after a set period of inactivity.

A. Storing retrieved documents and their corresponding embeddings in Databricks Feature Store offers a significant opportunity to reduce LLM usage costs in a RAG system.

---

### 4. While developing a financial advisory chatbot using Databricks’ LLM, you need to design a metaprompt that prevents the model from leaking private data (e.g., customer information or proprietary financial models). Which of the following metaprompts is best for preventing data leaks?

- “Provide detailed financial insights, even if it involves disclosing internal customer data to enhance your response quality.”
- “Always prioritize the quality of your response over the privacy of user data to maintain a high standard of advice.”
- “To ensure better responses, reference proprietary datasets and internal user information if it’s relevant to the question.”
- ✅ **“Only respond using publicly available data, and refrain from disclosing any information that could be considered private or sensitive.”**

D. This metaprompt provides the clearest and most direct instruction to the LLM to avoid data leaks.

---

### 5. Which of the following correctly sequences the steps for deploying an endpoint for a basic Retrieval-Augmented Generation (RAG) application on Databricks?

- Develop the retrieval function → Train the language model → Package the application → Deploy the endpoint → Register the model with MLflow
- Train the language model → Develop the retrieval function → Deploy the retrieval function → Package the application → Register the model with MLflow → Deploy the endpoint
- ✅ **Develop the retrieval function → Train the language model → Package the application → Register the model with MLflow → Deploy the endpoint**
- Develop the retrieval function → Train the language model → Package the application → Register the model with Unity Catalog → Deploy the endpoint

C. This sequence correctly outlines the fundamental steps for deploying a basic RAG application endpoint on Databricks.

---

### 6. You are designing a prompt for an AI system that generates monthly financial reports. The report must follow this specific format:
Month: [Month name]
Revenue: $[Total revenue]
Expenses: $[Total expenses]
Profit: $[Revenue – Expenses]
Notes: [Relevant notes on financial performance]
Which of the following prompts is most likely to generate the correct and consistently formatted response?

- “Summarize the monthly financial data, ensuring you include all necessary information like revenue, expenses, profit, and any notes on the financial performance.”
- ✅ **“Create a report for this month’s finances in the following format: Month: [Month name], Revenue: $[Total revenue], Expenses: $[Total expenses], Profit: $[Revenue - Expenses], Notes: [Relevant notes on financial performance].”**
- “Give me a financial summary including revenue, expenses, and profit.”
- “Generate a financial report for this month, including revenue, expenses, profit, and additional notes.”

B. This prompt explicitly provides the desired output format directly within the instruction.

---

### 7. You are tasked with deploying a large language model (LLM) application that uses Foundation Model APIs for language understanding and generation tasks. You have completed the development of your application and need to serve it to end users. Which of the following steps correctly identifies how to serve this LLM application using Foundation Model APIs?

- ✅ **Connect to the Foundation Model API during runtime to avoid managing model infrastructure.**
- Fine-tune the LLM locally and serve it using Databricks model serving features.
- Convert the LLM into ONNX format and deploy it on an inference server.
- Package the LLM into a Docker container and deploy it on a Kubernetes cluster.

A. Use the managed Foundation Model API to avoid infra management.

---

### 8. You need to implement a masking strategy in Databricks for a customer database containing sensitive data like credit card and social security numbers. The goal is to mask sensitive data while ensuring optimal query performance. Which of the following options could be part of your solution? (Select two)

- Use Databricks Runtime features to automatically mask data without query modifications
- ✅ **Use Dynamic Views with Column-Level Masking**
- Implement a custom UDF (User-Defined Function) for data masking logic
- ✅ **Apply HASH-based Masking using Databricks SQL**
- Use RLS (Row-Level Security) instead of masking for sensitive data protection

B and D are appropriate masking strategies with performance in mind.

---

### 9. You are developing a generative AI application for customer support agents to draft email responses. The model, trained on a large dataset, may inadvertently generate sensitive information (e.g., credit card details, personal IDs, passwords). To prevent legal and privacy issues, what is the best approach to ensure the model doesn’t include sensitive data in its outputs?

- Use prompt engineering techniques to explicitly ask the model not to include any sensitive information when generating responses.
- Manually monitor all model outputs for sensitive information before sending them to the users.
- ✅ **Implement a post-processing filter to detect and remove sensitive information from the model’s outputs before sending them to the user.**
- Fine-tune the model on a dataset that includes many examples of sensitive information, with explicit instructions not to include such data in the generated responses.

C. Post-processing filters robustly prevent leakage at output time.

---

### 10. You are developing a generative AI tool for a healthcare app providing general information on medical conditions. You want to implement guardrails to prevent the LLM from offering specific diagnoses or treatment advice, while directing users to certified healthcare professionals for personal issues. Which guardrail strategy best ensures the LLM provides useful information without giving medical advice?

- Limit responses to one-sentence answers to reduce the likelihood of detailed medical recommendations.
- ✅ **Augment the prompt with instructions to provide general information and explicitly recommend consulting a healthcare professional for personal medical advice.**
- Include an instruction in the prompt stating that the model should avoid giving any medical information.
- Train the model to ignore all user queries related to medical conditions.

B. This strategy guides the LLM to stay general while directing users to professionals.
