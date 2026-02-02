# 🐳 Python Flask App with Docker (Intermediate)

This project demonstrates how to containerize a simple **Python Flask application** using **Docker**, without requiring Python to be installed on your host machine.

It is designed to run easily on **Docker Playground** or any system with Docker installed.
<img width="2880" height="1704" alt="image" src="https://github.com/user-attachments/assets/09c525aa-abfd-40f6-aec2-5972830d5517" />
---

## 📁 Project Structure

```
.
├── Dockerfile
└── app.py
```

---

## 🧩 Application Overview

* **Framework:** Flask
* **Language:** Python 3.6
* **Base Image:** `python:3.6.1-alpine`
* **Port:** 5000

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
```

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

## 🚀 Running the App in Docker Playground

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
<img width="2880" height="1531" alt="image" src="https://github.com/user-attachments/assets/9443000d-b40c-415c-a348-1538c2d0c163" />
---

### 6️⃣ Access the application

* Click **Open Port** in Docker Playground
* Enter port **5000**

You should see:

```
Hello World from Docker!
```

<img width="2880" height="466" alt="image" src="https://github.com/user-attachments/assets/7c748d4d-e613-4054-b020-0c3eab024780" />

---

## 📦 Pushing the Image to Docker Hub

### 1️⃣ Login to Docker Hub

```bash
docker login
```

Enter your **Docker Hub username** and **password**.

---

### 2️⃣ Tag your image

Docker Hub requires images to be tagged with the format:

```
[dockerhub-username]/[image-name]
```

Example:

```bash
docker tag python-hello-world yourusername/python-hello-world
```

---

### 3️⃣ Push the image

```bash
docker push yourusername/python-hello-world
```

After a few moments, your image will be available on Docker Hub.
<img width="2880" height="1531" alt="image" src="https://github.com/user-attachments/assets/cc00cc42-b1fa-4d67-8976-d1f6fb4aeefa" />

---

### 4️⃣ Verify on Docker Hub

* Go to [https://hub.docker.com](https://hub.docker.com)
* Navigate to your profile → Repository → `python-hello-world`

<img width="2880" height="1205" alt="image" src="https://github.com/user-attachments/assets/73ed1318-18c9-410d-92ef-628c5320a6e5" />


Now, anyone can pull your image:

```bash
docker pull yourusername/python-hello-world
```

---

## 📌 Key Concepts Covered

* Dockerfile instructions: `FROM`, `RUN`, `COPY`, `CMD`
* Using official Docker images
* Docker image layering and caching
* Running Python apps without Python installed locally
* Pushing images to Docker Hub

---

## ✅ Notes

* Alpine images are lightweight and secure
* Pinning image versions avoids unexpected updates
* Copying source code at the end optimizes build caching
* Docker images bundle all dependencies, avoiding environment drift


````








