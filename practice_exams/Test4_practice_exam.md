### 1. A Generative AI Engineer is tasked with selecting an embedding model for a recommendation system. The system must process product descriptions and customer reviews. What factors are most critical for selecting the embedding model?

- Compatibility with pre-trained LLMs.
- Tokenization efficiency to reduce computational costs.
- Availability of public pre-trained embeddings.
- ✅ **Context length and semantic understanding of product descriptions and reviews.**

Embedding models with long context lengths and robust semantic understanding improve the system’s ability to align reviews with product descriptions.

---

### 2. A Generative AI Engineer is deploying a model that requires both embeddings and an LLM for document summarization. The model must handle high throughput for real-time queries. What infrastructure should they prioritize?

- Skip embedding storage and rely only on the LLM for summarization.
- ✅ **Deploy on a distributed cloud infrastructure with GPU acceleration for model inference and storage optimization for embeddings.**
- Use a local server with CPU-only resources to save costs.
- Deploy the model on a single cloud instance with minimal scaling capabilities.

Distributed cloud infrastructure with GPUs and optimized storage ensures scalability and performance for real-time summarization tasks.

---

### 3. A Generative AI Engineer is responsible for developing a chatbot to enable their company’s internal HelpDesk Call Center team to more quickly find related tickets and provide resolution. While creating the GenAI application work breakdown tasks for this project, they realize they need to start planning which data sources (either Unity Catalog volume or Delta table) they could choose for this application. They have collected several candidate data sources for consideration: call_rep_history: a Delta table with primary keys representative_id, call_id. This table is maintained to calculate representatives’ call resolution from fields call_duration and call_start_time. transcript Volume: a Unity Catalog Volume of all recordings as *.wav files, but also a text transcript as *.txt files. call_cust_history: a Delta table with primary keys customer_id, call_id. This table is maintained to calculate how much internal customers use the HelpDesk to make sure that the charge-back model is consistent with actual service use. call_detail: a Delta table that includes a snapshot of all call details updated hourly. It includes root_cause and resolution fields, but those fields may be empty for calls that are still active. maintenance_schedule: a Delta table that includes a listing of both HelpDesk application outages as well as planned upcoming maintenance downtimes. They need sources that could add context to best identify ticket root cause and resolution. Which TWO sources do that? (Choose two.)

- ✅ **call_detail**
- maintenance_schedule
- call_rep_history
- ✅ **transcript Volume**
- call_cust_history

Using call_detail for its structured fields (root_cause and resolution) and transcript Volume for detailed call context ensures the chatbot has comprehensive data to resolve tickets efficiently.

---

### 4. A Generative AI Engineer must deploy a basic RAG application for a customer support system. The application requires embedding models for vector search, retrievers for document lookup, and an LLM for response generation. What are the steps to deploy this application?

- Deploy the LLM first, then configure retrievers and embeddings post-deployment.
- Only configure the retriever and directly connect it to the LLM for responses.
- ✅ **Install dependencies, prepare embedding models, configure retrievers, and deploy an endpoint using model serving.**
- Use an embedding model for storage and retrieval, skipping retrievers entirely.

A proper sequence, including embedding, retrievers, and endpoint deployment, ensures a functional and optimized RAG application.

---

### 5. A Generative AI Engineer is tasked with selecting a chunking strategy for legal documents to optimize retrieval in a RAG system. The documents include lengthy paragraphs and footnotes. What strategy should they adopt?

- Split the document into equal-sized chunks without considering content structure.
- Process the entire document as a single chunk.
- ✅ **Divide the document into logical sections based on paragraphs, ensuring chunks remain within token limits.**
- Overlap all chunks by 200 tokens.

Logical chunking ensures that sections remain meaningful and fit within token limits, optimizing retrieval results.

---

### 6. A Generative AI Engineer is tasked with training a chatbot to summarize long technical manuals. Many manuals contain redundant examples that inflate the dataset size. What should the engineer prioritize during preprocessing?

- Retain all examples for completeness.
- Replace redundant examples with placeholders.
- Exclude entire manuals containing redundancy.
- ✅ **Remove redundant examples while preserving unique, meaningful content.**

Eliminating redundant examples ensures concise, high-quality input, which improves summarization model training.

---

### 7. A Generative AI Engineer is working on a chatbot that summarizes customer queries for a support team. The chatbot occasionally leaks sensitive data in summaries. What guardrail technique is most effective?

- Manually review all summaries for sensitive data before sharing.
- Rely on the chatbot’s training to avoid sensitive data leakage.
- ✅ **Use postprocessing filters to detect and redact sensitive information from generated outputs.**
- Reduce the model’s temperature to limit variability in summaries.

Postprocessing filters ensure sensitive data is identified and redacted from summaries, maintaining user trust and compliance.

---

### 8. A Generative AI Engineer is processing invoices stored as scanned PDFs for a retrieval-based application. The text includes numerical details like invoice IDs, amounts, and dates. Which Python package is the best fit for extracting this content? (Choose two)

- ✅ **PyTesseract**
- Tika
- PyPDF2
- ✅ **Textract**

PyTesseract and Textract are ideal for extracting numerical and textual details from scanned PDFs, providing accurate OCR-based text retrieval.

---

### 9. A Generative AI Engineer is tasked with choosing an embedding model for a search engine handling long research papers. What factor is most critical? (Choose two)

- ✅ **Context length supported by the embedding model.**
- Cost of inference.
- Token diversity.
- Training dataset size.
- ✅ **Semantic similarity accuracy.**

Long context support and high semantic similarity accuracy ensure effective embeddings for handling long research papers.

---

### 10. A Generative AI Engineer must deploy an LLM application using Foundation Model APIs for real-time customer support. What considerations are necessary for optimal serving?

- Skip API rate limits to allow unrestricted queries.
- ✅ **Ensure sufficient compute resources, manage API rate limits, and monitor latency.**
- Reduce compute resources to minimize costs.
- Use Foundation Model APIs without monitoring latency or compute resources.

Managing compute resources, API rate limits, and latency ensures optimal performance and user satisfaction in real-time applications.

---

### 11. A Generative AI Engineer is tasked with developing a multi-stage reasoning pipeline for a customer support chatbot. The chatbot must identify intent, retrieve relevant documents, and generate detailed responses. What is a critical design principle for the pipeline?

- Focus on generating responses without retrieval.
- Minimize the number of tools to reduce complexity.
- Use rule-based systems for document retrieval.
- ✅ **Ensure tools are interoperable and can share data seamlessly between stages.**

Multi-stage reasoning systems rely on interoperable tools to enable efficient workflows, ensuring smooth transitions between tasks like intent identification, retrieval, and response generation.

---

### 12. A Generative AI Engineer must handle biased data in a RAG application that delivers hiring recommendations. What is the best approach to address bias in the dataset?

- ✅ **Analyze the dataset for potential biases and remove or reweight biased entries to ensure fairness.**
- Use only pre-trained models and avoid modifying the dataset.
- Reduce the dataset size to exclude potentially biased information.
- Ignore the bias, as the model generates outputs based on user queries.

Analyzing and addressing biases in the dataset ensures fairness in the application and reduces the risk of perpetuating discrimination.

---

### 13. What is the primary purpose of guardrails in Generative AI applications?

- To limit model outputs to a fixed length.
- To improve response diversity for creative tasks.
- ✅ **To ensure outputs are safe, accurate, and aligned with application requirements.**
- To reduce latency in model inference.

Guardrails ensure that Generative AI applications produce outputs that are safe, ethical, and meet specific quality requirements.

---

### 14. A Generative AI Engineer must select a model from a marketplace for generating personalized financial advice. The system must prioritize accuracy and compliance. What attributes of the model should guide the selection? (Choose two)

- Choose a model with low latency for real-time responses.
- ✅ **Prioritize models with high accuracy on financial datasets.**
- Focus on models with larger parameter counts for better performance.
- ✅ **Evaluate the model’s metadata for training domain and fine-tuning details.**
- Select a model with low perplexity to ensure fluency.

Reviewing metadata and prioritizing domain-specific accuracy ensures the selected model meets the requirements for personalized financial advice.

---

### 15. A Generative AI Engineer must preprocess legal contracts before using them in a retrieval system. What step is most critical for improving retrieval accuracy?

- Retain the entire contract as a single document.
- ✅ **Chunk the contracts by logical sections, such as clauses or exhibits, and ensure chunks fit within the model’s token limits.**
- Split the contracts into equal-sized chunks.
- Randomly remove redundant sections.

Chunking legal contracts by logical sections ensures the retrieval system maintains context and provides accurate responses.

---

### 16. A Generative AI Engineer must select chain components for a system that answers user queries about company policies and cites specific policy sections. What components are essential?

- A decision tree for guiding users to policy answers.
- ✅ **A retriever to fetch relevant policy sections and an LLM to generate responses with citations.**
- A summarization model to condense policy documents.
- A classification model to categorize user queries by policy topic.

Combining retrievers and LLMs ensures accurate query responses with properly cited policy references.

---

### 17. A Generative AI Engineer must preprocess multilingual documents for a RAG system that serves English-only queries. What preprocessing step ensures consistency?

- ✅ **Translate all non-English content into English before further processing.**
- Retain all documents to provide potential multilingual functionality.
- Detect and filter non-English documents using a language detection tool.
- Process all documents without identifying their language.

Filtering non-English documents ensures the dataset is aligned with the system’s language requirements, avoiding irrelevant responses.

---

### 18. A Generative AI Engineer is asked to deploy a pipeline that retrieves relevant documents and answers user queries. Which component is critical for improving retrieval accuracy?

- A fine-tuned classification model.
- ✅ **A high-quality embedding model trained on domain-specific data.**
- Pre-trained response generation templates.
- Shorter document chunk lengths.

Using a domain-specific embedding model ensures that embeddings align with both documents and queries, improving retrieval accuracy.

---

### 19. A Generative AI Engineer must augment prompts dynamically based on user input. The user queries a customer support chatbot about “late delivery.” What is the best way to handle this?

- Exclude all contextual information to keep the prompt concise.
- ✅ **Include relevant context such as the order date, ID, and expected delivery time in the prompt.**
- Rely only on pre-written responses.
- Use a generic prompt like “Explain the issue.”

Augmenting prompts with relevant context ensures the LLM generates precise and user-specific responses.

---

### 20. A Generative AI Engineer is developing a RAG system to answer questions about company financial reports. Users have complained that answers are vague and lack citations. How should the engineer improve the system?

- Fine-tune the model on financial report datasets.
- Use a summarization model to simplify the financial data.
- ✅ **Add a step in the pipeline to include relevant citations with retrieved content.**
- Increase the model temperature to encourage more creative responses.

Adding citations improves transparency and trustworthiness, addressing user concerns about vague answers.

---

### 21. A Generative AI Engineer needs to evaluate the performance of a RAG system trained on legal documents. What metrics and tools should be used? (Choose two)

- BLEU scores for evaluating output text similarity.
- Token usage to track computational costs.
- ✅ **NDCG (Normalized Discounted Cumulative Gain) to measure ranking quality.**
- ✅ **Recall to measure how many relevant documents are retrieved.**
- Latency to measure query processing speed.

Combining recall and NDCG ensures the system retrieves relevant documents efficiently and ranks them accurately.

---

### 22. How can metaprompts help in reducing private data exposure in an LLM application?

- ✅ **Metaprompts can instruct the model to avoid including specific types of sensitive information in its outputs.**
- Metaprompts reduce response length, minimizing information exposure.
- Metaprompts block all numerical outputs.
- Metaprompts adjust the temperature for less risky responses.

Metaprompts help guide LLMs to avoid exposing private data by explicitly defining rules or safeguards in the instructions.

---

### 23. A Generative AI Engineer must deploy an LLM application that uses Foundation Model APIs. The application needs to handle document summarization and integrate retrieval capabilities. What sequence should they follow to deploy the application effectively?

- Use pre-trained summarization models without retrieval support.
- ✅ **Configure Foundation Model API access, create embeddings for documents, build a vector store, and deploy the endpoint.**
- Skip embedding creation and rely on document metadata.
- Deploy the endpoint first and configure the embedding model later.
- Skip retrieval capabilities and rely solely on the LLM for summarization.

Configuring APIs, embeddings, and vector stores ensures the LLM application handles summarization and retrieval seamlessly.

---

### 24. A Generative AI Engineer must select a chunking strategy for summarizing government reports with 10,000+ words. What is the best approach?

- Overlap all chunks by 200 tokens.
- ✅ **Chunk based on sections like “Introduction” and “Findings,” ensuring each chunk stays within token limits.**
- Split the document into equal-sized chunks of 1,000 tokens.
- Process the entire document as a single chunk.

Chunking by sections preserves the logical flow and ensures the model processes content effectively within token constraints.

---

### 25. A Generative AI Engineer must select an embedding model for FAQ retrieval. The system must support fast lookups and high accuracy. What strategy ensures optimal performance? (Choose two)

- ✅ **Use approximate nearest neighbor (ANN) search for faster lookups.**
- Use a generative model for response generation instead of embeddings.
- ✅ **Choose an embedding model with high accuracy on semantic similarity benchmarks.**
- Prioritize token length over speed.
- Use a small embedding model to save costs.

High semantic similarity accuracy combined with ANN search ensures fast and accurate retrieval in FAQ systems.

---

### 26. A Generative AI Engineer is tasked with selecting a model from a marketplace to perform entity recognition for legal documents. What factors should they prioritize in the model metadata? (Choose two)

- Multilingual capabilities.
- ✅ **Fine-tuning capabilities for legal datasets.**
- ✅ **Performance on entity recognition benchmarks.**
- The number of training parameters.
- Response generation speed.

Focusing on entity recognition benchmarks and fine-tuning capabilities ensures the model is effective for legal document tasks.

---

### 27. A Generative AI Engineer must evaluate a new LLM for summarizing research papers. The evaluation should prioritize accuracy and informativeness. What metrics and tools are most appropriate? (Choose two)

- Latency to measure the model’s response speed.
- Token usage to evaluate computational cost.
- ✅ **Perplexity for evaluating the fluency of summaries.**
- ✅ **Human qualitative assessment for relevance and informativeness.**
- ROUGE for measuring content overlap between generated summaries and reference summaries.

Combining ROUGE with human qualitative assessments ensures the evaluation captures both quantitative and qualitative aspects of summary quality.

---

### 28. A Generative AI Engineer must select an LLM for an FAQ retrieval system. The system must balance accuracy, cost efficiency, and scalability. Which criteria should guide the selection process?(Choose three)

- Training dataset diversity for better generalization.
- ✅ **Model size and architecture to control computational costs.**
- ✅ **Latency metrics to ensure real-time responsiveness for user queries.**
- ✅ **Token usage per query for cost tracking.**
- Response perplexity to ensure fluent and natural outputs.

Balancing latency, model size, and perplexity ensures the FAQ system is accurate, cost-effective, and scalable.

---

### 29. A Generative AI Engineer is tasked with deploying an LLM application that uses a vector store for document retrieval. The application must ensure low latency and scalability. What infrastructure should they prioritize?

- ✅ **Deploy the application on a scalable cloud infrastructure with a high-performance database for the vector store.**
- Deploy the application on a single-node server to minimize complexity.
- Use local storage for vector data to minimize costs.
- Skip using a vector store and rely on direct LLM query processing.

Scalable cloud infrastructure with a high-performance vector store ensures low latency and efficient scaling for production-grade systems.

---

### 30. A Generative AI Engineer is deploying a pyfunc model that predicts loan approvals. The model requires pre- and post-processing. What steps should the engineer take to ensure usability?

- Pass raw inputs directly to the model for faster predictions.
- Skip post-processing and return raw model scores.
- Use only preprocessing without any post-processing.
- ✅ **Preprocess inputs (e.g., clean and standardize data), run predictions through the pyfunc model, and format outputs (e.g., “Approved” or “Denied”) for user clarity.**

A workflow combining preprocessing, pyfunc predictions, and post-processing ensures accurate and user-friendly loan approval results.

---

### 31. A Generative AI Engineer is tasked with designing a system that converts long academic papers into concise summaries for students. The system should highlight key arguments and conclusions. What LLM configuration is most effective?

- Increase the model’s temperature to produce more diverse summaries.
- Reduce the model’s response length to ensure conciseness.
- ✅ **Fine-tune a summarization LLM on academic texts to generate concise summaries.**
- Use a generic LLM without fine-tuning.

Fine-tuning a summarization LLM ensures that the generated summaries capture key arguments and conclusions accurately.

---

### 32. A Generative AI Engineer is tasked with designing a pipeline to summarize legal documents accurately. How should they evaluate the effectiveness of the summarization model?

- Focus solely on user feedback.
- Measure response latency only.
- ✅ **Use metrics such as ROUGE and BLEU scores to measure summarization quality.**
- Use token counts to measure the length of the summaries.

ROUGE and BLEU are widely used metrics for evaluating the accuracy and quality of summarization models, ensuring reliable performance.

---

### 33. A Generative AI Engineer is selecting source documents for training a RAG system that answers healthcare questions. What criteria should guide document selection?

- Prioritize lengthy documents to maximize training data size.
- ✅ **Select documents with high-quality medical information relevant to the target domain.**
- Include all publicly available medical texts regardless of relevance.
- Select documents with diverse but non-specialized medical content.

Choosing high-quality, relevant medical documents ensures the RAG system retrieves accurate answers aligned with healthcare queries.

---

### 34. A Generative AI Engineer is tasked with summarizing user reviews for a product. The summary must highlight common themes and sentiment. What is the most suitable model task?

- Text classification
- ✅ **Text summarization**
- Named entity recognition
- Sentiment analysis

Text summarization effectively captures the essence of user reviews, providing concise insights into themes and sentiment.

---

### 35. A Generative AI Engineer must evaluate the success of a summarization pipeline for legal documents. The generated summaries must be concise and include all critical legal points. Which metrics should the engineer prioritize?

- ✅ **ROUGE and recall to measure content relevance and coverage.**
- Precision and retrieval accuracy to optimize inputs.
- BLEU and response length to measure accuracy and conciseness.
- Latency and perplexity to track speed and fluency.

ROUGE and recall ensure the summaries are concise, relevant, and comprehensive, meeting the requirements for legal document analysis.

---

### 36. A Generative AI Engineer is designing a conversational AI for customer service. The AI must process queries and recommend actions based on the conversation’s context. What should the engineer prioritize in the prompt design?

- Focus on brevity to generate concise responses.
- Specify only the tone of the conversation.
- ✅ **Include clear context and desired output. Example: “User is asking about late deliveries. Recommend next steps to resolve their issue.”**
- Avoid examples to encourage creativity.

Providing clear context and expected output guides the model to generate accurate, user-relevant recommendations for customer service interactions.

---

### 37. A Generative AI Engineer is creating an LLM-based application where documents for the retriever are chunked to 512 tokens each. The engineer prioritizes cost and latency over quality. Which configuration fulfills their need?

- Context length 514; smallest model is 0.44GB and embedding dimension 768.
- ✅ **Context length 512; smallest model is 0.13GB and embedding dimension 384.**
- Context length 2048; smallest model is 11GB and embedding dimension 2560.
- Context length 32768; smallest model is 14GB and embedding dimension 4096.

A context length of 512 aligns perfectly with the application’s chunk size and reduces costs and latency due to the smaller model size.

---

### 38. A Generative AI Engineer must deploy a Foundation Model API for a conversational agent. The API usage is expected to grow rapidly. How should the engineer optimize for scalability?

- ✅ **Implement horizontal scaling with autoscaling policies to handle variable workloads.**
- Implement rate limits without scaling the infrastructure.
- Use a fixed number of compute resources to handle peak demand.
- Reduce the context length to save computational resources.

Horizontal scaling ensures the infrastructure adapts to changing workloads while maintaining high performance and reliability.

---

### 39. A Generative AI Engineer must deploy a Vector Search index for a knowledge base of scientific papers. The system must support real-time queries with low latency. What combination of tools and resources should they use? (Choose three)

- A summarization model trained on scientific datasets.
- ✅ **High-performance GPUs for low-latency computations.**
- ✅ **Distributed databases to store raw documents.**
- Lightweight CPU infrastructure to reduce costs.
- ✅ **FAISS for creating and querying the vector index.**

Building a Vector Search index requires FAISS for efficient indexing, GPUs for low-latency queries, and distributed databases for scalable document storage.

---

### 40. A Generative AI Engineer needs to design a prompt for a document summarization system that highlights key points and outputs summaries in bullet points. What should the prompt include?

- Highlight the main ideas of the document in a concise paragraph.
- Generate a summary of the document.
- ✅ **Summarize the document in bullet points highlighting the key points.**
- Create an abstract of the document for a general overview.

Explicit prompts defining both the format (bullet points) and content requirements ensure that the summarization system outputs aligned results.

---

### 41. A Generative AI Engineer is creating a pre- and post-processing pipeline for a PyFunc model. Why is this approach useful?

- It reduces the need for retraining the model.
- ✅ **It allows data to be transformed before and after model inference, ensuring compatibility and optimized outputs.**
- It automates hyperparameter tuning for the model.
- It minimizes API costs by batching inputs.

Pre- and post-processing pipelines streamline workflows by transforming inputs and outputs, improving compatibility and system efficiency.

---

### 42. A Generative AI Engineer is tasked with masking sensitive user information in a customer support chatbot. What is the most effective approach to achieve this?

- ✅ **Implement regex-based patterns to identify and mask sensitive data like phone numbers and credit card details.**
- Rely on the LLM’s training to avoid generating sensitive outputs.
- Manually filter user inputs before processing.
- Use a summarization model to exclude sensitive details.

Regex-based masking ensures sensitive data is identified and securely masked, maintaining compliance with privacy standards.

---

### 43. A Generative AI Engineer is tasked with monitoring a production LLM application for a multilingual customer support system. What key metrics should they track? (Choose two)

- ✅ **Latency, response accuracy, and language coverage.**
- BLEU scores for multilingual responses.
- ✅ **Token usage and cost per query.**
- Input-output length ratio.
- Fine-tuning accuracy for each language.

Tracking latency, accuracy, language coverage, and costs ensures comprehensive monitoring of a multilingual customer support LLM.

---

### 44. A Generative AI Engineer is tasked with controlling LLM costs for a RAG application deployed on Databricks. What strategies should they implement? (Choose two)

- Rely on fine-tuning the largest model for all queries.
- ✅ **Limit API calls by caching frequently retrieved outputs.**
- Increase context length for more comprehensive outputs.
- ✅ **Optimize token usage to minimize input-output lengths for each query.**
- Use smaller LLMs for non-complex queries while reserving larger models for critical tasks.

Optimizing token usage and selecting models based on task complexity are effective strategies for managing LLM costs in Databricks.

---

### 45. A Generative AI Engineer is building a QA system to assist medical professionals. The system must retrieve answers from medical guidelines and provide explanations. What chain components are required?

- ✅ **A retriever to fetch guideline sections and an LLM to generate responses with explanations.**
- A rule-based system to map queries to guideline sections.
- A summarization model to condense guideline content.
- A classification model to categorize queries by topic.

Using retrievers and LLMs ensures that the system dynamically retrieves and explains answers from medical guidelines.

---

### 46. A Generative AI Engineer must filter a dataset of emails for a RAG application that extracts customer complaints. Many emails contain greetings, sign-offs, and irrelevant information. What is the best preprocessing step?

- ✅ **Remove standard greetings and sign-offs during preprocessing.**
- Retain all text to preserve context.
- Group emails based on their similarity score before filtering.
- Exclude emails shorter than 50 words.

Filtering out greetings and sign-offs ensures the dataset focuses on meaningful text, improving RAG system efficiency and response quality.

---

### 47. A Generative AI Engineer needs to deploy an LLM for product recommendations. The system must balance cost efficiency and response latency. What strategy should they implement?

- ✅ **Use a smaller fine-tuned model for inference while caching frequent queries.**
- Deploy multiple models to process the same query simultaneously.
- Use the largest pre-trained model available.
- Focus solely on reducing latency by increasing compute resources.

Combining fine-tuning, model size optimization, and caching ensures a cost-effective, low-latency recommendation system.

---

### 48. A Generative AI Engineer is tasked with creating a pipeline to store structured text extracted from scanned receipts in Delta Lake tables. What sequence ensures consistency and efficiency?

- Use regex on the raw text and write results directly to Delta Lake.
- Manually clean and load data into Delta Lake tables.
- Write raw extracted text into Delta Lake without defining a schema.
- ✅ **Define a schema, apply OCR to extract text, and write structured data to Delta Lake using Spark.**

A structured pipeline using a schema, OCR, and Spark ensures reliable, scalable, and efficient data ingestion into Delta Lake tables.

---

### 49. A Generative AI Engineer is tasked with improving the quality of a RAG system by addressing inflammatory outputs in its responses. What is the most effective action to mitigate offensive outputs?

- Inform the user of the expected RAG behavior.
- Restrict access to the data sources to a limited number of users.
- Increase the frequency of upstream data updates.
- ✅ **Curate upstream data through manual review before ingestion.**

Properly curated data is essential for mitigating offensive outputs and improving RAG system quality.

---

### 50. A Generative AI Engineer is designing a QA system for a technical support platform. The system must include safeguards to avoid generating inaccurate troubleshooting steps. What is the most effective solution?

- ✅ **Implement LLM guardrails, such as verification steps and retrieval-based augmentation.**
- Use a summarization model to condense troubleshooting guides.
- Train the LLM on technical manuals and support guides.
- Reduce the model’s temperature to limit randomness.

Implementing guardrails like verification and retrieval ensures accurate, contextually relevant troubleshooting responses.

---

### 51. A Generative AI Engineer is tasked with filtering a dataset of academic papers for a RAG system. The papers include irrelevant sections like acknowledgments and appendices. What is the best approach?

- ✅ **Filter out irrelevant sections like acknowledgments and appendices during preprocessing.**
- Use the entire paper to maximize context for retrieval.
- Merge all sections into a single document for consistency.
- Retain all sections for completeness.

Preprocessing academic papers by filtering irrelevant sections ensures higher-quality retrieval in RAG systems.

---

### 52. A team is deploying a RAG application to assist in legal document review. The application frequently retrieves irrelevant documents despite using embeddings. What adjustment should the engineer prioritize to improve document retrieval accuracy?

- ✅ **Use a retriever optimized for semantic similarity and train the embedding model on domain-specific data.**
- Add pre-trained classification models for better retrieval.
- Implement random sampling to generate diverse retrieval results.
- Increase the chunk size of documents to include more context.

Domain-specific embeddings and an optimized retriever improve retrieval accuracy, ensuring relevant legal documents are surfaced efficiently.

---

### 53. A Generative AI Engineer needs to assess the quality of an LLM’s output for generating product descriptions. What qualitative assessment should they focus on?

- Number of unique words used.
- Length and style consistency.
- Response generation speed.
- ✅ **Relevance, creativity, and factual accuracy.**

Evaluating relevance, creativity, and factual accuracy ensures high-quality product descriptions that meet user expectations.

---

### 54. What is the primary benefit of registering a model’s signature when deploying an endpoint?

- It enables real-time logging of model performance.
- ✅ **It ensures that the model receives inputs and returns outputs in the correct format.**
- It increases model training efficiency.
- It reduces latency during inference.

Model signatures define input and output formats, ensuring that the model operates correctly during deployment and inference.

---

### 55. A Generative AI Engineer is building a chatbot for troubleshooting software issues. What guardrail strategy can prevent the chatbot from suggesting harmful actions?

- Reduce the length of chatbot responses.
- Allow the chatbot to generate unrestricted responses.
- ✅ **Use predefined filters to exclude unsafe or potentially harmful responses.**
- Prioritize response speed over accuracy.

Predefined filters act as guardrails, preventing unsafe suggestions and ensuring the chatbot remains reliable and safe to use.

---

### 56. A Generative AI Engineer must design a prompt template for an agent that performs database queries. The template should expose available functions such as create_record, update_record, and delete_record. What prompt structure is most effective?

- Use a generic instruction like “Manipulate database records as needed.”
- Summarize the database schema and let the model infer functions.
- ✅ **Provide a list of available functions (create_record, update_record, delete_record) and instructions for using them.**
- Instruct the model to perform actions based on its training data.

Providing a clear list of functions and instructions ensures the model correctly interacts with the database according to the requirements.

---

### 57. A Generative AI Engineer is tasked with deploying an LLM in a RAG application on Databricks. The team needs to monitor both system performance and cost. Which combination of tools and techniques should they use? (Choose three)

- ✅ **MLflow to track retrieval metrics like precision and recall.**
- ✅ **Inference logging to track token usage and response latency for each query.**
- ✅ **GPU utilization monitoring to optimize hardware performance.**
- Increasing context length to reduce token usage.
- Fine-tuning the LLM on Databricks.

Combining inference logging, MLflow, and GPU monitoring provides a comprehensive view of system performance and cost, enabling optimization.

---

### 58. A Generative AI Engineer is building an AI tool for summarizing meeting transcripts. The tool must identify key points, action items, and decisions. What model task is best suited for this requirement?

- Named entity recognition
- Sentiment analysis
- ✅ **Text summarization**
- Text classification

Summarization is the most suitable task for condensing meeting transcripts into concise, actionable outputs like key points, decisions, and action items.

---

### 59. A Generative AI Engineer is building a RAG (Retrieval-Augmented Generation) system for a legal firm. The system retrieves relevant clauses from contracts and summarizes them for lawyers. Which component is most critical to ensure relevant clauses are retrieved?

- Build a rule-based system for clause extraction.
- ✅ **Implement a vector database for semantic retrieval of clauses.**
- Fine-tune an LLM for legal documents.
- Use a keyword-based search algorithm.

Vector databases ensure retrieval of contextually relevant clauses, providing accurate inputs for generating high-quality summaries.

---

### 60. A Generative AI Engineer must prepare text from a series of fragmented documents and write the chunked text into Delta Lake tables using Unity Catalog. What is the correct sequence of operations?

- Use a summarization model to preprocess the text before storage.
- Define the schema after writing the data into Delta Lake tables.
- Write all text as a single batch into Delta Lake tables without chunking.
- ✅ **Chunk the text → Define a Delta Lake schema → Batch the chunks → Write them into Delta Lake tables.**

This workflow ensures efficient data preparation, chunking, and storage in Delta Lake tables for Unity Catalog.
