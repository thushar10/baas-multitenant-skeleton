## Infra Notes (Day 1)

- **Where will this run?**  
  Local Docker today; EKS later

- **What secrets are needed?**  
  None today

- **What does “success” look like?**  
  CI green + a working health check or script output

- **Open questions:**  
  - How will you model tenants (if applicable)?  
  - What storage/index will you use for RAG (FAISS, ES, pgvector)?  
  - How will you test correctness?  
