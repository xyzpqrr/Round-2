# Intelligent Document Analyzer – Methodology

The Intelligent Document Analyzer is a lightweight and efficient tool designed to extract the most relevant insights from PDF documents, tailored to a specific user's role and objective. The project was developed with a strong emphasis on offline execution, CPU-only compatibility, and fast processing (under 10 seconds), making it ideal for hackathon scenarios and resource-constrained environments.

## 1. PDF Parsing and Structure Detection

Using the `PyMuPDF` library, the system extracts content from each page of the PDF, capturing font size, boldness, and spatial layout information. This allows for the identification of structural elements like titles, headings (H1, H2), and paragraphs. A heuristic-based classification system is employed to label each chunk of text appropriately.

## 2. Chunking and Metadata Association

Once the structural elements are identified, the text is split into logical chunks. Each chunk is paired with metadata such as the document name, page number, and heading context. This enables downstream modules to maintain a traceable context and hierarchy during relevance evaluation.

## 3. Semantic Embedding Using `bge-small`

To enable semantic search, all text chunks are converted into vector embeddings using the `bge-small` model — a compact and efficient transformer-based model (approximately 100MB) suitable for local use. The user’s input — consisting of their role and job-to-be-done — is also embedded into the same vector space.

Cosine similarity is calculated between the user prompt embedding and each chunk embedding. This determines the relevance score for each chunk with respect to the user’s intent.

## 4. Top Chunk Selection

Chunks are ranked based on their semantic similarity scores. The top N (typically 10–20) chunks are selected as the most relevant portions of the content. These are carried forward for summarization and final output generation.

## 5. Local Summarization with `phi-2`

Each top chunk is summarized using the `phi-2` language model executed via the Ollama runtime. This compact LLM is capable of running locally without a GPU, and provides accurate, focused summaries of each selected section.

## 6. Output Format

The final output is a structured JSON file containing:
- Document name and page number
- Detected heading or title
- Semantic similarity score
- Summary generated by `phi-2`

This format ensures that the output is both human-readable and machine-usable for further integration.

## Summary

By combining rule-based document parsing, lightweight local embedding, and efficient summarization, the Intelligent Document Analyzer delivers fast, accurate, and contextual insights — all without needing internet access or heavy compute. This makes it a strong fit for time-bound hackathons and deployment in low-resource environments.