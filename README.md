# 🚀 Deploying ML Models with FastAPI, Docker & Kubernetes  
*By: Enzo KARA*  

<div align="center">
<img src="https://i.ibb.co/XLZTbBG/fastapi-ml-deployment.png" width="60%"/><br>
<sup>Figure adapted from Chansung Park</sup>
</div>

---

## 🧠 Overview  
This project demonstrates how to **deploy a machine learning model as a REST API** using **FastAPI**, **Docker**, and **Kubernetes (K8s)**.  
It walks through the **entire production workflow** — from model optimization to automated deployment in the cloud via **GitHub Actions** and **Google Kubernetes Engine (GKE)**.  

While the example uses an **image classification model** (ONNX format), the same structure can be applied to **any ML model** — NLP, audio, or tabular.  

---

## ⚙️ Architecture  

```mermaid
graph TD;
    A[User Request (image)] --> B[FastAPI App]
    B --> C[ONNX Runtime Inference]
    C --> D[JSON Response]
    B --> E[Docker Container]
    E --> F[Kubernetes Cluster (GKE)]
    F --> G[Auto-Scaling + LoadBalancer]
```

---

## 🧩 Key Features  

✅ FastAPI REST service for real-time predictions  
✅ Model optimized to ONNX for CPU efficiency  
✅ Dockerized container for reproducibility  
✅ Kubernetes deployment for scalability and resilience  
✅ Automated CI/CD via GitHub Actions  
✅ Load-testing with Locust for performance evaluation  

---

## 📂 Project Structure  

```
ml-deployment-k8s/
│
├── api/
│   ├── main.py                # FastAPI app (prediction endpoint)
│   ├── model/
│   │   └── model.onnx         # Optimized model file
│   └── utils/
│       └── inference.py       # Inference helper functions
│
├── notebooks/
│   └── TF_to_ONNX.ipynb       # Converts TensorFlow model → ONNX
│
├── k8s/
│   ├── deployment.yaml        # Pod + ReplicaSet definition
│   ├── service.yaml           # Exposes the API externally
│   └── ingress.yaml           # (optional) domain routing
│
├── locust/                    # Load-testing setup
│   └── locustfile.py
│
├── Dockerfile
├── requirements.txt
├── .github/workflows/
│   └── deployment.yaml        # CI/CD pipeline for GKE
└── README.md
```

---

## 🤪 Running Locally  

### 1️⃣ Clone the repository
```bash
git clone https://github.com/EnzoKara/ml-deployment-k8s.git
cd ml-deployment-k8s
```

### 2️⃣ Create environment and install dependencies
```bash
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### 3️⃣ Run the API locally
```bash
uvicorn api.main:app --reload
```

Access interactive docs 👉 [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)

---

## 🐳 Running with Docker  

```bash
docker build -t ml-deployment-k8s .
docker run -p 8000:8000 ml-deployment-k8s
```

---

## ☸️ Deploying on Kubernetes  

### 1️⃣ Apply the deployment and service files
```bash
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
```

### 2️⃣ Check deployment status
```bash
kubectl get pods
kubectl get services
```

If the service type is `LoadBalancer`, note the **EXTERNAL-IP** and test your endpoint.

---

## 🧠 Example API Request  

```bash
curl -X POST -F image_file=@cat.jpg \
     -F with_resize=True \
     -F with_post_process=True \
     http://<EXTERNAL-IP>:80/predict/image
```

Response:
```json
{
  "Label": "tabby",
  "Score": 0.538
}
```

---

## 🔁 CI/CD Workflow  

Each time you push changes to the `main` branch:
1. GitHub Actions builds the Docker image  
2. Pushes it to **Google Container Registry (GCR)**  
3. Deploys it to **Google Kubernetes Engine (GKE)**  
4. Updates the API endpoint automatically  

---

## 📊 Load Testing  

The project includes a **Locust** setup to simulate concurrent requests and measure:
- Latency  
- Throughput  
- Scalability across nodes  

**Result Summary:**  
> Best configuration = 8 nodes (2 vCPUs, 4 GB RAM each) for optimal balance between speed and cost.

---

## 🧮 Tech Stack  

| Category | Tools |
|-----------|-------|
| API | FastAPI, Uvicorn, Pydantic |
| ML Runtime | ONNX Runtime |
| Containerization | Docker |
| Orchestration | Kubernetes (GKE) |
| CI/CD | GitHub Actions |
| Testing | Locust |
| Cloud | Google Cloud Platform (GCR + GKE) |

---

## 🦭 Why this project matters  

This project bridges the gap between **ML research and production**.  
It shows that you can:
- Build, containerize, and deploy ML models reliably  
- Automate deployment pipelines  
- Scale and monitor workloads on Kubernetes  

It’s a solid foundation for **MLOps**, **AI DevOps**, and **cloud-ready ML engineering**.  

---

## 🏆 Acknowledgements  
- [ML-GDE Program](https://developers.google.com/programs/experts/) for GCP support  
- [Chansung Park](https://github.com/deep-diver) for the original project structure inspiration  
- [Hannes Hapke](https://www.linkedin.com/in/hanneshapke) for insights on load-testing  
