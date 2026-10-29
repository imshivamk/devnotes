
# Complete Roadmap / Guide for AI & ML (Beginner to Advanced – Free Resources)

This document presents a complete, structured, and end‑to‑end roadmap for learning Artificial Intelligence, Machine Learning, Deep Learning, Generative AI, and Agentic Systems using free resources only.  
It is designed to be progressive, non‑overlapping, and industry‑aligned, moving from fundamentals to autonomous AI system design..

---

## Phase‑0: Prerequisites

Goal: Build programming and tooling fundamentals required before entering AI/ML.

### 0.1 Complete Python for AI/ML

This covers Python basics and all essential data‑science libraries. - Python for Data Science – Course for Beginners (Learn Python, Pandas, NumPy, Matplotlib) – freeCodeCamp - Python Object Oriented Programming (OOP) – Full Course for Beginners 

### 0.2 Git Basics

Basic version control knowledge is required for all ML and AI work. - Git & GitHub Crash Course for Beginners [2026]

### 0.3 Linux Commands (Optional)

Useful for development environments and servers. - [The 50 Most Popular Linux & Terminal Commands – Full Course for Beginners](https://www.youtube.com/watch?v=mAFoROnOfHs)

---

## Phase‑1: Mathematical Foundations   

Goal: Understand why ML and DL models work, not just how to use them.

### Resources

- [Essence of Linear Algebra](https://youtube.com/playlist?list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab&si=QdnZlBYMl72HATM2)
    
- [Essence of Calculus](https://youtube.com/playlist?list=PLZHQObOWTQDMsr9K-rj53DwVRMYO3t5Yr&si=8WEiUTJD5y8fx3nT)[https://youtube.com/playlist?list=PLlpUUtQ9RrF76jvALwrTp0oOGfk0EGC3s&si=e_2proSVah2LfZ5e](https://youtube.com/playlist?list=PLlpUUtQ9RrF76jvALwrTp0oOGfk0EGC3s&si=e_2proSVah2LfZ5e)
    
- [Statistics Fundamentals](https://youtube.com/playlist?list=PLblh5JKOoLUK0FLuzwntyYI10UQFUhsY9&si=1n6V1EFdaaPOg6hG)
    
- [Math for Machine Learning: Probability, Calculus & Linear Algebra | Essential for Data Science, AI & ML](https://youtube.com/playlist?list=PLlpUUtQ9RrF76jvALwrTp0oOGfk0EGC3s&si=e_2proSVah2LfZ5e)
    

Some topics may require individual follow‑up videos for clarity.

---

### 1.1 Linear Algebra (Core of ML)  

Scalars, vectors, matrices, tensors (basic idea), dimensions and shape; vector operations (addition, subtraction, scalar multiplication, dot product, cross product – intuition, vector norm, unit vectors); matrix operations (addition, multiplication – very important, transpose, inverse – intuition, identity and diagonal matrices); geometric interpretation (vectors as points and directions, matrices as linear transformations: rotation, scaling, projection, basis and change of basis); systems of linear equations (matrix form, solving intuition, rank); eigen concepts (eigenvalues, eigenvectors, why they matter, PCA intuition); similarity and distance (Euclidean distance, cosine similarity, projections).

Used in: Neural Networks, Linear Regression, PCA, Embeddings, Attention, Transformers.

---

### 1.2 Probability & Statistics (ML Reasoning)

Probability basics (sample space, events, rules, conditional probability, independence); random variables (discrete, continuous), PMF and PDF; distributions (Bernoulli, Binomial, Normal); expectation, variance, standard deviation, law of expectation (intuition); Bayes theorem (prior, likelihood, posterior, evidence, Naive Bayes intuition); statistics essentials (mean, median, mode, variance, covariance, Pearson correlation, correlation vs causation); sampling and estimation (population vs sample, sampling bias); bias–variance tradeoff (overfitting, underfitting, model complexity, train vs test error).

Used in: Naive Bayes, Classification, Uncertainty Estimation, Model Evaluation.

---

### 1.3 Calculus (Optimization & Learning)

Functions (linear, non‑linear, multivariable); limits (intuition); derivatives (slope, rate of change); partial derivatives; gradients and direction of steepest descent; chain rule and its role in neural networks; optimization (loss functions, convex vs non‑convex, local vs global minima); gradient descent (learning rate, convergence, batch vs stochastic).

Used in: Gradient Descent, Backpropagation, Neural Network Training.

---

## Phase‑2: Core Machine Learning (Foundation Models)

Goal: Learn classical ML using feature‑based models and structured data.

  

### Resources

- [Machine Learning with Python and Scikit‑Learn – Full Course](https://youtu.be/hDKCxebp88A?si=pCsJeZsAcZ8qi_gZ)
    
- [Machine Learning Foundations Course – Regression Analysis](https://youtu.be/2IZKK3pJQjg?si=UXgbRjD8l7Y1wo3Q)
    
- [Stanford CS229: Machine Learning (Andrew Ng)](https://youtube.com/playlist?list=PLoROMvodv4rMiGQp3WXShtMGgzqpfVfbU&si=GUzJ9-JjK8R1ePvm)
    
- [Machine Learning with StatQuest](https://youtube.com/playlist?list=PLblh5JKOoLUICTaGLRoHQDuF_7q2GfuJF&si=ert3_AkRjwQ1PRHf)
    
- [AI Foundations Course – Python, Machine Learning, Deep Learning, Data Science](https://youtu.be/0oyDqO8PjIg?si=Jh29M99jfrb4Sby_)
    

By the end[AI Foundations Course – Python, Machine Learning, Deep Learning, Data Science](https://youtu.be/0oyDqO8PjIg?si=Jh29M99jfrb4Sby_) of this phase, all listed topics should be understood. Any gaps can be filled with individual videos.

---

### 2.1 Introduction to Machine Learning

ML vs AI vs Deep Learning; supervised, unsupervised, semi‑supervised (conceptual); regression, classification, clustering; ML workflow (problem definition, data collection, feature engineering, training, evaluation, deployment – conceptual); training vs inference.

---

### 2.2 Data Understanding & Preprocessing

Structured data; numerical vs categorical data; missing values; outlier handling; categorical encoding (label, one‑hot); feature scaling (normalization, standardization); feature selection vs extraction; data leakage (intro).

Note: Models learn from features, not raw data.

---

### 2.3 Supervised Learning – Regression

Linear, multiple and polynomial regression; hypothesis functions; loss functions (MSE); gradient descent; underfitting vs overfitting; regularization (L1/L2 – intuition); metrics (MAE, MSE, RMSE, R²).

Used in: Price prediction, forecasting, trend estimation.

---

### 2.4 Supervised Learning – Classification

Logistic regression, KNN, decision trees, random forest (intuition); decision boundaries; probability thresholds; class imbalance; metrics (confusion matrix, accuracy, precision, recall, F1, ROC‑AUC).

Used in: Spam detection, fraud detection, medical diagnosis.

---

### 2.5 Unsupervised Learning

Clustering (K‑Means, hierarchical – intuition, DBSCAN – intuition); dimensionality reduction using PCA.

Used in: Segmentation, anomaly detection, visualization.

---

### 2.6  Model Selection & Validation

Train/validation/test split; cross‑validation; hyperparameter tuning (grid search); applied bias–variance tradeoff.

---

### 2.7 ML Pipelines & Best Practices

End‑to‑end pipelines; preprocessing pipelines; avoiding data leakage; reproducibility; experiment tracking (conceptual).

---

### 2.8 ML Libraries (Hands‑On)

NumPy, Pandas, Scikit‑learn (fit/predict, preprocessing, pipelines, metrics).

Note: Scikit-learn ‑learn is the backbone of Phase‑2.

---

### 2.9 Ethics & Limitations of ML

Bias in data; assumptions; interpretability limits; correlation vs causation.

---

### 2.10 Phase‑2 Projects (Mandatory)

House price prediction; student performance prediction; spam classifier; credit risk classifier; customer segmentation using K‑Means.

---

## Phase‑3: Deep Learning & Advanced ML

Goal: Build and understand neural‑network‑based systems end‑to‑end.

### Resources

- [Neural Networks by 3Blue1Brown](https://youtube.com/playlist?list=PLZHQObOWTQDNU6R1_67000Dx_ZCJB-3pi&si=MEIuFlXgvJwkvuPK)
    
- [Neural Networks: Zero to Hero – Andrej Karpathy](https://youtube.com/playlist?list=PLAqhIrjkxbuWI23v9cThsA9GvCAUhRvKZ&si=FlBjT7U_Q50Cmfh8)
    
- [PyTorch for Deep Learning & Machine Learning – Full Course](https://youtu.be/V_xro1bcAuA?si=8kNii-DpHvmjpVtY)
    
- [100 Days of Deep Learning by CampusX](https://youtube.com/playlist?list=PLKnIA16_RmvYuZauWaPlRTC54KxSNLtNn&si=w8sU1P_y0jBeIROd)
    
- [MIT 6.S191: Introduction to Deep Learning](https://youtube.com/playlist?list=PLtBw6njQRU-rwp5__7C0oIVt26ZgjG9NI&si=6bc5VuBu531O2eg-)
    
- [Deep Learning – Andrew Ng](https://learn.deeplearning.ai/specializations/deep-learning)
    

---

### Phase‑3 Topics

Mathematical foundations for deep learning; neural network fundamentals; backpropagation; training deep networks; PyTorch frameworks; feedforward networks; CNNs; RNNs, LSTM, GRU; attention and transformers; embeddings; optimization and debugging; advanced ML concepts; ethics and limitations.

---

### Phase‑3 Projects (Mandatory)

Tabular neural network; CNN image classifier; sequence or time‑series model; introductory transformer text classifier.

---

## Phase‑4: LLMs, NLP & Generative AI

Goal: Use transformer‑based LLMs to build real‑world AI applications.

### Resources

- [Intro to Large Language Models – Andrej Karpathy](https://youtu.be/zjkBMFhNj_g?si=OxEFnAGuIti83IbF)
    
- [Create a Large Language Model from Scratch with Python – Tutorial](https://youtu.be/UU1WVnMk4E8?si=BsS6hieIVrk0rsdl)
    
- [Building LLMs from scratch by Vizuara](https://youtube.com/playlist?list=PLPTV0NXA_ZSgsLAr8YCgCwhPIJNNtexWu&si=Syh8sdOSDunZW-6H) 
    
- [Large Language Models (LLMs)](https://youtube.com/playlist?list=PLoROMvodv4rObv1FMizXqumgVVdzX4_05&si=xr-4e8ejrBQm-lox)
    
- [Natural Language Processing(NLP)](https://youtube.com/playlist?list=PLKnIA16_RmvZo7fp5kkIth6nRTeQQsjfX&si=gdwTxbDtkqKp7Lad)
    
- [Stanford CS224N Natural Language Processing with Deep Learning I Spring 2024 I Professor Christopher Manning](https://youtube.com/playlist?list=PLoROMvodv4rOaMFbaqxPDoLWjDaRAdP9D&si=wlO1gTQKIqKtps1w)
    
- [Generative AI using LangChain – CampusX](https://youtube.com/playlist?list=PLKnIA16_RmvaTbihpo4MtzVm4XOQa0ER0&si=Opc3SDsjyzNnNAOT)
    
- [Fine‑Tuning LLM Models – Generative AI Course](https://youtu.be/iOdFUJiB0Zc?si=khYYafostSUk5N8u)
    
- [Complete LangChain GenAI Crash Course (End‑to‑End Projects)](https://youtu.be/aWKrL4z5H6w?si=B88bYoKbXdSSkJ4R)
    

---

### Phase‑4 Topics

Generative AI foundations; LLM fundamentals; prompt engineering; embeddings and semantic search; Retrieval‑Augmented Generation (RAG); fine‑tuning (high‑level); LLM APIs and controls; building LLM‑powered applications; safety and limitations.

---

### Phase‑4 Projects (Mandatory)

Chatbot using LLM APIs; document Q&A with RAG; semantic search system; domain‑specific AI assistant.

---

## Phase‑5: Agentic Systems & AI System Design

Goal: Design autonomous, goal‑driven AI systems with tools, memory, orchestration, and safety controls.

### Resources

- [agentic AI by ng andrew](https://learn.deeplearning.ai/courses/agentic-ai/)
    
-  [AI Agents Course by hugging-face](https://huggingface.co/learn/agents-course/unit0/introduction)
    
- [AI Agents for Beginners](https://youtube.com/playlist?list=PLlrxD0HtieHgKcRjd5-8DT9TbwdlDO-OC&si=vrQPBvBHbBTm7Zhp) 🡪 [microsoft/ai-agent](https://github.com/microsoft/ai-agents-for-beginners)
    
- [Building AI Agents from Scratch | Full Course](https://youtu.be/1OLrT3dEzhA?si=cj3hh3qzVQ8E2fAg)
    
- [Agent Design Patterns](https://youtube.com/playlist?list=PLacQJwuclt_sK_pUPzBpfeWyiL1QOSMRQ&si=mRDO4TdfipnBtcNO)
    
- [Guide to Agentic AI – Build a Python Coding Agent with Gemini](https://youtu.be/YtHdaXuOAks?si=bo6eZAa63EaY1TOd)
    
- [LangGraph Complete Course for Beginners – Complex AI Agents with Python](https://youtu.be/jGg_1h0qzaM?si=hkgKEa8n3nVECdOp)
    
- [n8n Tutorial – Zero to Hero Course](https://youtu.be/UIf-SlmMays?si=tm6mqraml82m-wCI)
    
- [AI Agent Tutorials by Nate Herk | AI Automation](https://youtube.com/playlist?list=PLvQWpZ46MVvgUUUBxnqLAu-JzA-6QA1o2&si=K9tMnNAwawq6vErZ)
    
- [n8n Tips & Tricks](https://youtube.com/playlist?list=PLvQWpZ46MVvgS_b2ipWcDGLgv5EHsgebD&si=vQof7w-K8K9EWv4g)
    
- [N8N For Everyone (AI Agents, Workflows, & More)](https://youtube.com/playlist?list=PLSbQllRagIuQZ8BrXay_s9xZlGLzAlHFJ&si=mtX_wQHz2lYlbOD1)
    
- [Agentic IDEs and AI Code Monkeys](https://youtube.com/playlist?list=PLbzjzOKeYPCpPSAD9qvIDBQ-TGx-y6hjV&si=rA8HCcBA2nZZYY6l)
    
    ---
    

### Phase‑5 Topics

From LLMs to agents; agent architecture; planning and reasoning strategies; tool and action execution; memory systems; multi‑agent systems; orchestration and workflow control; agentic RAG; evaluation and guardrails; enterprise AI system design.

---

### Phase‑5 Projects (Mandatory)

Autonomous research agent; multi‑tool assistant; multi‑agent task delegation system; agentic workflow with human approval; enterprise AI automation..

---

This roadmap represents a complete learning path from beginner to advanced agentic AI systems using free resources.

Reinforcement Learning (RL) and Robotics are intentionally not included in this roadmap, as they represent specialized branches of AI rather than core requirements for modern ML, LLM, and agentic system development. RL focuses on sequential decision-making through reward optimization and is commonly used in robotics, control systems, game AI, and autonomous navigation. Robotics further combines RL with control theory, simulation, perception, and real-world hardware constraints. While these areas are valuable and closely related to agentic AI, they are best pursued after completing this roadmap, depending on specific interests or career goals. Understanding their existence and high-level purpose is sufficient at this stage.

  
  
  
