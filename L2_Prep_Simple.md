# L2 Interview Prep — Simple Language (3 hrs prep)

Rule of thumb: L1 checks "have you done this?" L2 checks "do you *understand* this well enough to defend your choices?" So expect more "why" and "what if" questions.

---

## TOPIC 1: ChromaDB vs Azure AI Search

**Q1. Why choose Azure AI Search over ChromaDB for a real project?**
A: Think of ChromaDB like a personal notebook — quick, free, easy to set up, good for small projects or testing. Azure AI Search is like a professional filing system built by a company — it can handle huge amounts of data, has security built in, works well with other Microsoft tools, and can search using both keywords AND meaning (called hybrid search) at the same time. For a big company project, Azure AI Search is usually safer and easier to scale.

**Q2. How does the search actually work behind the scenes (HNSW)?**
A: Imagine you're looking for a book in a library, but instead of checking every single book, you use a smart map that jumps you close to where similar books are kept. That "smart map" trick is called HNSW (Hierarchical Navigable Small World). It's fast because it doesn't check everything — just the most likely matches. The tradeoff: it's slightly less accurate than checking every single item, but way faster.

**Q3. What is hybrid search?**
A: It's combining two search styles together — (1) keyword search (matching exact words) and (2) semantic/vector search (matching meaning, even if words are different). Example: searching "car problems" should also find a document that says "vehicle issues" — that's semantic search catching what keyword search would miss.

**Q4. How would you make search faster if you have millions of documents?**
A: Simple idea — don't search everything every time. Break data into smaller groups (sharding), filter out irrelevant stuff first using tags/categories, and cache (save) answers to common questions so you don't repeat work.

---

## TOPIC 2: Prompting Strategies

**Q5. Zero-shot vs Few-shot vs Chain-of-Thought — what's the difference?**
A: 
- Zero-shot = just ask the question directly, no examples given.
- Few-shot = give 2-5 examples first, so the AI understands the pattern/format you want.
- Chain-of-thought = ask the AI to "think step by step" out loud before giving the final answer — helps with math, logic, multi-step problems.

**Q6. What is ReAct prompting?**
A: Short for "Reasoning + Acting." The AI doesn't just answer directly — it thinks a bit, then takes an action (like searching the internet or calling a tool), looks at the result, thinks again, and repeats until it has a final answer. Useful when the AI needs to use outside tools, not just its own knowledge.

**Q7. How do you know which prompting method is working best?**
A: Test it like a science experiment — run the same set of questions through different prompt styles, measure how many answers were correct, check how long it took, and how many "tokens" (cost) it used. Pick whichever gives the best balance of accuracy, speed, and cost.

**Q8. What is prompt chaining?**
A: Breaking one big task into smaller steps, where each step's answer feeds into the next step. Example: Step 1 — extract key facts from a document. Step 2 — summarize those facts. Step 3 — classify the summary. Like an assembly line instead of doing everything in one go.

---

## TOPIC 3: Chunking Strategies

**Q9. What is chunking and why do we need it?**
A: AI models can only "read" a limited amount of text at once. So we break big documents into smaller pieces (chunks) before feeding them to the AI for search or answering questions. Good chunking = better answers, because the AI gets clean, relevant pieces instead of a huge messy document.

**Q10. How do you decide chunk size?**
A: It's a balancing act. Too small = you lose context (like reading only half a sentence). Too big = you include unrelated stuff that confuses the AI. Common practice: 300–800 words per chunk, with a little overlap (repeating the last few lines in the next chunk) so nothing important gets cut off at the boundary.

**Q11. Difference between fixed-size, recursive, and semantic chunking?**
A:
- Fixed-size = just cut text every X words, no matter what — simple but can cut mid-sentence.
- Recursive = try to cut at natural breaks first (paragraph, then sentence, then word) — smarter, keeps sentences whole.
- Semantic = use AI to group sentences that are *about the same topic* together, even if they're not next to each other — most accurate, but slower and costs more (because it uses AI to decide).

**Q12. What libraries are used for semantic chunking?**
A: LangChain's "SemanticChunker," LlamaIndex's "SemanticSplitterNodeParser," and tools like spaCy or NLTK to first split text into sentences before grouping them by meaning.

**Q13. Can an LLM (like GPT) itself do the chunking?**
A: Yes — you can literally ask the AI "split this document into meaningful sections." It gives the best quality chunks because it understands context, but it's expensive and slow if you have thousands of documents — so it's only worth it for small, high-value documents, not bulk processing.

**Q14. How do you chunk something tricky like a table or code?**
A: Don't break tables or code in the middle — keep them as one whole chunk. Use special "structure-aware" splitters that understand tables/code/markdown format, so they know not to cut through a table row or a function.

---

## TOPIC 4: Classical Machine Learning

**Q15. What is bias-variance tradeoff?**
A: Bias = when your model is too simple and makes the same mistakes everywhere (underfitting — like using a straight ruler to draw a curve). Variance = when your model is too sensitive and changes wildly based on small data differences (overfitting — memorizing instead of learning). A good model needs to balance both — not too simple, not too complicated.

**Q16. Beyond what you know already — how do you prevent overfitting?**
A: 
- Regularization (a penalty that stops the model from getting too complicated)
- Cross-validation (testing on multiple different slices of data, not just one)
- Early stopping (stop training before the model starts memorizing)
- Using more training data
- Removing unnecessary features

**Q17. Precision vs Recall — when do you care about which one?**
A: Precision = "Of everything I said was positive, how many were actually correct?" Use this when false alarms are costly (e.g., marking a good email as spam). Recall = "Of everything that was actually positive, how many did I catch?" Use this when missing something is dangerous (e.g., missing a disease in a medical test). F1 score = a balance of both.

**Q18. Difference between L1 and L2 regularization?**
A: Both are techniques to stop overfitting by "punishing" the model for being too complex. L1 (Lasso) can shrink some features all the way down to zero — basically removing them, useful for picking the most important features. L2 (Ridge) shrinks features gently but rarely removes any completely — better when features are related to each other.

---

## TOPIC 5: Python

**Q19. Generators vs Iterators — simple explanation?**
A: An iterator is anything you can loop through one item at a time. A generator is an easy way to build an iterator using the `yield` keyword — it gives you one value at a time instead of creating the whole list in memory at once. Great for very large data where you don't want to load everything into memory.

**Q20. What is a context manager (the `with` keyword)?**
A: It's a way to automatically clean up after yourself. Example: when you open a file using `with open(file) as f:`, Python automatically closes the file for you afterward — even if an error happens in between. Saves you from forgetting to close things manually.

**Q21. What is the GIL (Global Interpreter Lock)?**
A: Python can only run ONE thread of actual code at a time (even on multi-core computers) because of the GIL. So if your task needs heavy CPU work, using multiple threads won't actually speed things up — you need "multiprocessing" instead (running separate full Python programs). But for tasks that just wait (like downloading files), threads work fine because they're mostly just waiting, not computing.

**Q22. What's a closure?**
A: A function that "remembers" variables from where it was created, even after that outer function has finished running. It's the trick behind how decorators work.

**Q23. Quick recap — decorator vs monkey patching (since you covered these in L1)?**
A: Decorator = wrapping a function to add extra behavior, done cleanly using `@decorator_name` syntax, without changing the original function's code. Monkey patching = changing or replacing a function/method while the program is already running, directly, without using the clean decorator syntax — powerful but risky because it can silently break other parts of the code.

---

## TOPIC 6: Likely NEW L2 Questions (system design flavor — go deeper than L1)

**Q24. How do you stop an AI from making up false information (hallucination)?**
A: 
- Make sure the retrieval step finds the RIGHT documents (better chunking + reranking)
- Lower the "temperature" setting (makes AI less random/creative)
- Ask the AI to double-check its own answer against the source documents
- If confidence is low, make it say "I don't know" instead of guessing

**Q25. How do you check if your RAG (retrieval) system is actually working well?**
A: Two parts — check if it's finding the RIGHT documents (retrieval accuracy) and check if the final answer is actually correct and grounded in those documents (generation accuracy). Tools like RAGAS can automate this. Also do manual human review on a sample of answers.

**Q26. What is reranking and why do we need it after search?**
A: After the first search finds, say, top 20 possible matches, reranking is a second, smarter pass that re-orders those 20 by TRUE relevance using a more powerful (but slower) model. It's like a rough first sort, then a careful second sort — improves accuracy but adds a bit of delay.

**Q27. What are AI agents, and how are they different from a simple chatbot?**
A: A simple chatbot just answers questions using its own knowledge. An AI agent can actually DO things — call tools, search the internet, run code, use APIs, and make decisions about what steps to take next, often working through multiple steps on its own to complete a task.

**Q28. What is multi-agent orchestration?**
A: Instead of one AI doing everything, you split the work between multiple specialized "AI agents" that each handle a part of the task and talk to each other — like a team where one agent researches, another writes, another checks quality. Frameworks like LangGraph, CrewAI, and Microsoft Agent Framework help build and manage this.

**Q29. What is LLMOps and how is it different from MLOps?**
A: MLOps = practices for deploying/monitoring traditional ML models (training, versioning, retraining). LLMOps = similar idea but specific to large language models — includes things like prompt versioning, monitoring token costs, tracking hallucination rates, and managing model updates from providers (like OpenAI/Azure).

**Q30. In banking, what's a realistic use case for Agentic AI, and what would you be careful about?**
A: Example: a loan-processing assistant agent that reads documents, checks KYC details, and flags missing info automatically. Be careful about: data privacy/compliance (banking data is sensitive), avoiding wrong decisions that affect real money, keeping a human in the loop for final approval, and following Responsible AI guidelines (bias, fairness, explainability).

---

## Fast Recap Table (last-minute glance)

| Topic | Key thing to remember |
|---|---|
| ChromaDB vs Azure Search | Chroma = light/dev, Azure = enterprise/hybrid/scale |
| HNSW | Smart shortcut search, fast but approximate |
| Prompting | Zero-shot (no example) → Few-shot (examples) → CoT (step-by-step) |
| Chunking | Fixed (simple) → Recursive (smart breaks) → Semantic (meaning-based, costly) |
| Overfitting fix | Regularization, cross-validation, early stopping |
| Precision vs Recall | Precision = avoid false alarms, Recall = don't miss anything |
| GIL | One thread runs Python code at a time; use multiprocessing for CPU work |
| Agents vs Chatbot | Chatbot answers, Agent takes actions using tools |
| Hallucination fix | Better retrieval + low temperature + self-check + say "I don't know" |
