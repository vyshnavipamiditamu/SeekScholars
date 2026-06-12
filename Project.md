
## What is SeekerScholar? 

"SeekerScholar is a web-based academic paper search engine built on the ogbn-arxiv dataset, which contains over 160,000 computer science papers from arXiv.

The system offers **four different search methods**:
- **BM25** for fast keyword-based search
- **BERT** for semantic understanding
- **PageRank** for finding authoritative papers
- **Hybrid** which combines all three for the best results

Users can search in two ways:
1. **Text search** - simply type in a query
2. **File upload** - upload a PDF, DOCX, or text file, and the system extracts the content and finds similar papers

The interface is clean and modern, showing results with relevance scores, paper titles, abstracts, and direct links to arXiv."

---

## Technical Architecture 

"Let me walk you through how SeekerScholar works under the hood.

**Frontend:**
- Built with React and TypeScript
- Modern, responsive UI with real-time search
- Handles file uploads and displays results beautifully

**Backend:**
- FastAPI framework for high-performance REST API
- Clean architecture with separate modules for configuration, search engine, PDF processing, and data loading

**The Search Engine:**
The core innovation is our **2-stage retrieval pipeline**:

**Stage 1 - Fast Candidate Generation:**
- Always starts with BM25, which is extremely fast
- Retrieves the top 300 most relevant candidates based on keyword matching
- This narrows down from 160,000 papers to just 300 in milliseconds

**Stage 2 - Intelligent Re-ranking:**
- Only processes those 300 candidates with more sophisticated methods
- For BERT: computes semantic similarity using neural embeddings
- For PageRank: combines keyword relevance with citation authority
- For Hybrid: intelligently combines all three methods

This approach ensures that even complex neural models only process a small subset, keeping everything fast."

---

## Search Methods Explained 

"Let me explain each search method:

**BM25 Search:**
- Uses traditional keyword matching
- Fast and effective for exact term searches
- Best when you know specific terms or phrases
- Typical response time: under 50 milliseconds

**BERT Search:**
- Uses sentence transformers to understand meaning, not just keywords
- Finds papers that are semantically similar even if they use different terminology
- Great for conceptual searches like 'attention mechanisms in deep learning'
- Processes only the 300 BM25 candidates, so it's still fast - typically under 150 milliseconds

**PageRank Search:**
- Combines keyword relevance with citation network analysis
- Finds papers that are both relevant AND influential
- Uses the citation graph to identify authoritative papers
- PageRank scores are precomputed at startup for instant access
- Typical response time: under 60 milliseconds

**Hybrid Search - The Default:**
- This is our recommended method
- Combines all three approaches with weighted scoring:
  - 30% BM25 for keyword relevance
  - 50% BERT for semantic understanding
  - 20% PageRank for authority
- **How we determined these weights:**
  - These weights are based on information retrieval best practices and heuristic tuning
  - BERT gets the highest weight (50%) because semantic understanding is often the most valuable for finding conceptually similar papers
  - BM25 (30%) provides a solid baseline for keyword matching, ensuring exact term matches are still prioritized
  - PageRank (20%) adds authority signals without overwhelming relevance - we want relevant papers first, then boost authoritative ones
  - Scores are normalized to [0,1] range before combining, ensuring fair contribution from each method
  - These weights can be tuned based on evaluation metrics or user feedback
- Typically under 200 milliseconds, but provides the most comprehensive results"

---

## Performance Optimizations

"We've implemented several key optimizations:

**1. Two-Stage Pipeline:**
- Neural models only process 300 candidates, not 160,000 papers
- This makes BERT and Hybrid searches 500 times faster than naive approaches

**2. Precomputed Data:**
- All embeddings, BM25 index, and PageRank scores are precomputed
- Loaded once at startup, not per-request
- This means instant access during searches

**3. Query Optimization:**
- Text queries are truncated to 2048 characters
- PDF uploads use only the first 100 words as the search query
- This ensures consistent performance regardless of input length

**4. Smart Caching:**
- In-memory LRU cache stores 256 recent search results
- Cached queries return in under 10 milliseconds
- Perfect for repeated searches or similar queries

**5. Efficient Model Loading:**
- All models loaded once at startup
- Shared across all requests
- No per-request overhead"

---

## Key Features 

"SeekerScholar includes several user-friendly features:

**Multiple Search Methods:**
- Users can choose the method that best fits their needs
- Dropdown selector for easy switching

**File Upload Support:**
- Upload PDFs, DOCX, or text files
- Automatic text extraction
- Finds papers similar to your document

**Relevance Scores:**
- Each result shows a relevance score
- Higher scores mean better matches
- Helps users quickly identify the most relevant papers

**Direct arXiv Links:**
- One-click access to full papers on arXiv
- No need to manually search

**Real-time Search:**
- Instant feedback as you type
- Cancel button for long-running searches
- Loading indicators for better UX"


## Technical Stack 

"From a technical perspective:

**Backend:**
- Python 3.11+ with FastAPI
- SentenceTransformers for BERT embeddings
- NetworkX for graph analysis
- PyPDF and python-docx for file processing

**Frontend:**
- React with TypeScript
- Vite for fast development and building
- Modern CSS for responsive design

**Deployment:**
- Backend ready for Render or similar platforms
- Frontend ready for Vercel or Netlify
- Docker support included"

---

## Results and Impact 

"SeekerScholar delivers impressive performance:

- **Speed**: Most searches complete in under 200 milliseconds
- **Accuracy**: Hybrid method combines the strengths of all approaches
- **Scalability**: 2-stage pipeline handles large datasets efficiently
- **User Experience**: Clean interface with instant feedback

The system successfully bridges the gap between traditional keyword search and modern semantic search, while maintaining the speed users expect. Researchers can now find relevant papers faster, whether they're looking for specific terms, conceptual ideas, or authoritative sources."

---

## Challenges and Solutions 

"During development, we faced several challenges:

**Challenge 1: Speed with Large Datasets**
- Problem: Processing 160,000 papers with neural models would be too slow
- Solution: 2-stage pipeline with BM25 candidate generation

**Challenge 2: Long Query Performance**
- Problem: Long abstracts or PDFs slow down neural models
- Solution: Query truncation and first-100-words optimization for PDFs

**Challenge 3: Memory Management**
- Problem: Loading all embeddings and models into memory
- Solution: Efficient precomputation and single-load architecture

**Challenge 4: User Experience**
- Problem: Long searches feel unresponsive
- Solution: Cancel functionality, loading indicators, and smart caching"

---

## Future Enhancements 

"Potential future improvements include:

- **Multi-language support** for international papers
- **Citation network visualization** to explore paper relationships
- **Saved searches and favorites** for user accounts
- **Advanced filters** by date, category, or author
- **Recommendation system** based on search history
- **Export functionality** to save search results"

---

## Conclusion 

"In conclusion, SeekerScholar demonstrates how combining traditional information retrieval with modern machine learning can create a powerful, fast, and user-friendly search system. The 2-stage pipeline architecture ensures speed, while multiple search methods provide flexibility for different use cases.

Whether you're a researcher looking for specific papers, exploring a new field, or finding similar work to your own, SeekerScholar makes academic paper discovery faster and more intuitive.

Thank you for your attention. I'm happy to answer any questions!"


