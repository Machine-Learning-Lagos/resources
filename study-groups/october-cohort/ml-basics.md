# **📘 10-Week ML Study Group Curriculum (Beginners)**

---

### **Week 1–2: ML Math Foundations (Crash Course)**

**Goal**: Build just enough math to understand how ML algorithms work. Focus on intuition, not heavy proofs.

* **Linear Algebra (Week 1\)**

  * Vectors, matrices, dot product, matrix multiplication

  * Eigenvalues, eigenvectors

  * PCA intuition (dimensionality reduction)  
    **Resource**: [Vectors | Chapter 1, Essence of linear algebra](https://www.youtube.com/watch?v=fNk_zzaMoSs&list=PLZHQObOWTQDPD3MizzM2xVFitgF8hE_ab)  
* **Calculus & Optimisation (Week 2\)**

  * Derivatives & gradients

  * Chain rule → how backprop works

  * Gradient descent (batch, stochastic, mini-batch)

  * Loss functions (MSE, cross-entropy)  
    **Resource**: [The essence of calculus](https://www.youtube.com/watch?v=WUvTyaaNkzM&list=PLZHQObOWTQDMsr9K-rj53DwVRMYO3t5Yr)	

⚡ *By the end of Week 2: Students should be comfortable with the math behind linear regression, gradient descent, and PCA*

---

### **Week 3: Python Libraries for ML**

**Goal**: Get hands-on with tools.

* Pandas, NumPy for data handling

* Matplotlib & Seaborn for visualisation

* Cleaning datasets (missing values, categorical encoding)

* Exploratory Data Analysis (EDA) mini-project  
  **Resources**:  
  * [Pandas Tutorials \- YouTube](https://www.youtube.com/playlist?list=PL-osiE80TeTsWmV9i9c58mdDCSskIFdDS)  
  * [Matplotlib Tutorials \- YouTube](https://www.youtube.com/playlist?list=PL-osiE80TeTvipOqomVEeZ1HRrcEvtZB_)	   
  * [Python NumPy Tutorial for Beginners](https://www.youtube.com/watch?v=QUT1VHiLmmI&t=13s)

📌 *Mini Project*: Analyse a Kaggle dataset (e.g., Titanic or Iris).

---

### **Week 4: Introduction to Machine Learning**

**Goal**: Understand ML fundamentals.Train-test split, cross-validation

* What is ML? (supervised, unsupervised, reinforcement)  
* Evaluation metrics (accuracy, precision, recall, F1, RMSE)  
* Cross-validation  
* Bias-variance tradeoff  
* Linear regression  
* Regularization techniques (L1, L2)  
* Feature importance  
  **Resources**:  
  * [Machine Learning Algorithm | Types Of Machine Learning](https://youtu.be/C3jKfGrNU6c?si=1FHBCCT631RPAZDU)  
  * [Machine Learning Evaluation](https://youtu.be/txcnZAIyyeg?si=sWF4EUwjJOnW9qNR)  
  * [Training and Testing Data Tutorial](https://youtu.be/fwY9Qv96DJY?si=cyvmfjjABsdLv7Rx)  
  * [Cross Validation Fundamentals](https://youtu.be/fSytzGwwBVw?si=aanggdYQcwePurEg)  
  * [Bias and Variance Fundamentals](https://youtu.be/EuBBz3bI-aA?si=0aPWCe7lOEc9FM-c)  
  * [Linear Regression Explained](https://youtu.be/nk2CQITm_eo?si=MnIosFht6jzEBjLX)  
  * [Ridge (L2) Regression](https://youtu.be/Q81RR3yKn30?si=dPBlTf5nHIEmXMrx)  
  * [Lasso (L1) Regression](https://youtu.be/NGf0voTMlcs?si=eahyNtl4LJVRPNev)  
  * [Feature Selection in Machine Learning](https://youtu.be/7tW29jBceRw?si=jviExRBxeCUwkFDP)

📌 *Hands-on*: Predict house prices (regression) or classify Titanic survival (classification).

---

### **Week 5: Classic Supervised ML Algorithms**

**Goal**: Learn tree-based and margin-based models.

* Logistic regression  
* Decision trees  
* Random forests  
* Gradient boosting machines  
* Support Vector Machine (SVM)  
  **Resources:**  
  * [Logistic Regression Series (Parts 1-3)](https://youtu.be/vN5cNN2-HWE?si=t0bU7M3ia5Zd88zy)  
  * [Decision and Classification Trees](https://youtu.be/_L39rN6gz7Y?si=aJSZ116LUzwoxRpl)  
  * [Random Forests Series (Parts 1-2)](https://youtu.be/J4Wdy0Wc_xQ?si=wN0tmXflpFYyy3uM)  
  * [Gradient Boost Series (Parts 1-4)](https://youtu.be/3CC4N4z3GJc?si=ifDcVz8hkY6WvApM)  
  * [Support Vector Machines Series (Parts 1-3)](https://youtu.be/efR1C6CvhmE?si=pYawu509cwkz9A_R)

📌 *Mini Project*: Compare algorithms on the same dataset (Titanic/Iris/Heart Disease).

---

### **Week 6: Unsupervised Learning**

**Goal**: Explore patterns without labels.

* K-means clustering  
* Hierarchical clustering  
* Principal Component Analysis (PCA)  
* t-SNE

  **Resources:**


  * [StatQuest: K-means clustering](https://youtu.be/4b5d3muPQmA?si=8dl6gM86FYy8SCxo)  
  * [StatQuest: Hierarchical Clustering](https://youtu.be/7xHsRkOdVwo?si=0Ez6OzIOWj-QHBXd)  
  * [StatQuest: PCA main ideas](https://youtu.be/HMOI_lkzW08?si=AyWcTzwdAczbJp2E)  
  * [StatQuest: t-SNE Explained](https://youtu.be/NEaUSP4YerM?si=geTWxaRTe0SpL9Ub)

📌 *Mini Project*: Segment customers using a shopping dataset.

---

### 

### **Week 7: Advanced ML Concepts**

**Goal**: Improve models beyond basics.

* Hyperparameter tuning (GridSearchCV, RandomSearch)

* Ensemble methods: Bagging, Boosting, Stacking  
  **Resources:**  
  * [Simple Methods for Hyperparameter Tuning](https://youtu.be/vnXx7t2pnvQ?si=_jAZh0HDZSwDnUv-)  
  * [Ensemble Methods in Machine Learning](https://youtu.be/sN5ZcJLDMaE?si=wFWfX7Uh_ChzhyVL)

📌 *Hands-on*: Tune a RandomForest/XGBoost on a dataset and compare baseline vs tuned.

---

### **Week 8: Intro to Model Deployment**

**Goal**: Learn how ML models are used in real-world apps.

* Save/load models (Pickle/Joblib)

* Deploy with FastAPI or Flask (simple API)

* Streamlit or Gradio for interactive apps  
  **Resources:**  
  * [ML Model Deployment Using Flask](https://youtu.be/J4-2CEV6X_Y?si=N_SQ7iVlhubxscp0)  
  * [Creating APIs with FastAPI](https://youtu.be/5PgqzVG9SCk?si=ScnVb0vBXbJjFIHx)  
  * [FastAPI vs Flask Tutorial](https://youtu.be/Wr1JjhTt1Xg?si=FKtNtKcTJ0SZSlBf)  
  * [Streamlit Deployment Tutorial](https://youtu.be/DqpIeYdwkzA?si=_-hQHrunGQRGfMAn)  
  * [Gradio Crash Course](https://youtu.be/eE7CamOE-PA?si=VI_p-ep9dDCa7BX5)

📌 *Mini Project*: Deploy a simple ML model (Iris or Titanic classifier) with Streamlit/Gradio.

---

### **Week 9: Review & Capstone Preparation**

**Goal**: Consolidate learning and prepare for the final project.

* Group reviews: supervised \+ unsupervised techniques

* Model evaluation recap

* Students brainstorm capstone project ideas (with datasets)

* Capstone proposal presentations

---

### **Week 10: Capstone Project**

**Goal**: Apply everything learned.

* Each team selects a dataset (Kaggle, UCI, OpenML)

* Must include:

  * Data cleaning \+ EDA

  * At least 2 supervised models compared

  * One unsupervised technique (e.g., clustering/PCA)

  * Hyperparameter tuning

  * Deployment via Streamlit/Gradio

🎓 *Final Day*: Capstone presentations \+ feedback.

