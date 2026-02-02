# Docker-intermediate

# 🐳 Python Flask App with Docker

This project demonstrates how to containerize a simple **Python Flask application** using **Docker**, without requiring Python to be installed on the host machine.

It is designed to run easily on **Docker Playground** or any system with Docker installed.

---

## 📁 Project Structure

```

.
├── Dockerfile
└── app.py

````

---

## 🧩 Application Overview

- **Framework:** Flask
- **Language:** Python 3.6
- **Base Image:** `python:3.6.1-alpine`
- **Port:** 5000

The application exposes a single endpoint (`/`) that returns a simple "Hello World" message.

---

## 📝 app.py

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World from Docker!"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
````
<img width="2880" height="1704" alt="image" src="https://github.com/user-attachments/assets/09c525aa-abfd-40f6-aec2-5972830d5517" />

---

## 🐳 Dockerfile

```dockerfile
FROM python:3.6.1-alpine
RUN pip install --upgrade pip
RUN pip install flask
COPY app.py /app.py
CMD ["python", "/app.py"]
```

---

## 🚀 How to Run (Docker Playground)

### 1️⃣ Start Docker Playground

* Visit: [https://labs.play-with-docker.com](https://labs.play-with-docker.com)
* Login and click **Add New Instance**

---

### 2️⃣ Create the files

```bash
vi app.py
```

Paste the Flask code and save:

```bash
:wq
```

```bash
vi Dockerfile
```

Paste the Dockerfile content and save:

```bash
:wq
```

---

### 3️⃣ Build the Docker image

```bash
docker image build -t python-hello-world .
```

⚠️ The dot (`.`) at the end is required.

---

### 4️⃣ Verify the image

```bash
docker image ls
```

---

### 5️⃣ Run the container

```bash
docker container run -d -p 5000:5000 python-hello-world
```

---

### 6️⃣ Access the application

* Click **Open Port** in Docker Playground
* Enter port **5000**

You should see:

```
Hello World from Docker!
```

---

## 📌 Key Concepts Covered

* Dockerfile instructions (`FROM`, `RUN`, `COPY`, `CMD`)
* Using official Docker images
* Docker image layering and caching
* Running a Python app without Python installed locally

---

## ✅ Notes

* Alpine images are lightweight and secure
* Pinning image versions avoids unexpected updates
* Source code is copied at the end to optimize build caching

---



<img width="2880" height="1531" alt="image" src="https://github.com/user-attachments/assets/9443000d-b40c-415c-a348-1538c2d0c163" />
<img width="2880" height="466" alt="image" src="https://github.com/user-attachments/assets/7c748d4d-e613-4054-b020-0c3eab024780" />

