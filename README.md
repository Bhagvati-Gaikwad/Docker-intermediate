# 🐳 Python Flask App with Docker (Intermediate)

This project demonstrates how to containerize a Python Flask application using Docker, enabling it to run consistently across any environment without requiring Python locally.

It is lightweight, easy to set up, and works seamlessly on Docker Playground or any system with Docker. By leveraging Docker’s layer caching and efficient rebuilds, this workflow is ideal for CI/CD pipelines, allowing rapid deployment of updates with minimal overhead.

## Runnning Python Flask App on Local system
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

## Using Docker Playground

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
### 5️⃣ Deploy a change
Update app.py by replacing the string "Hello World" with "Hello Beautiful World!" in app.py.

Your file should have the following contents:

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello Beautiful World!"


if __name__ == "__main__":
    app.run(host='0.0.0.0')
```    
Now that your application is updated, you need to rebuild your app and push it to the Docker Hub registry.

Rebuild the app by using your Docker Hub username in the build command:
```bash
$  docker image build -t jzaccone/python-hello-world .
Sending build context to Docker daemon  3.072kB
Step 1/4 : FROM python:3.6.1-alpine
 ---> c86415c03c37
Step 2/4 : RUN pip install flask
 ---> Using cache
 ---> ce41f2517c16
Step 3/4 : CMD python app.py
 ---> Using cache
 ---> 0ab91286958b
Step 4/4 : COPY app.py /app.py
 ---> 3e08b2eeace1
Removing intermediate container 23a955e881fc
Successfully built 3e08b2eeace1
Successfully tagged jzaccone/python-hello-world:latest
Notice the "Using cache" for Steps 1 - 3. These layers of the Docker image have already been built, and the docker image build command will use these layers from the cache instead of rebuilding them.
```
```bash
$ docker push jzaccone/python-hello-world
The push refers to a repository [docker.io/jzaccone/python-hello-world]
94525867566e: Pushed 
64d445ecbe93: Layer already exists 
18b27eac38a1: Layer already exists 
3f6f25cd8b1e: Layer already exists 
b7af9d602a0f: Layer already exists 
ed06208397d5: Layer already exists 
5accac14015f: Layer already exists 
latest: digest: sha256:91874e88c14f217b4cab1dd5510da307bf7d9364bd39860c9cc8688573ab1a3a size: 1786
```
<img width="2880" height="1531" alt="image" src="https://github.com/user-attachments/assets/d55c1619-ed25-4264-bb61-b76752c5cd7f" />
<img width="2880" height="613" alt="image" src="https://github.com/user-attachments/assets/ae83012f-fda6-4d3f-9fe8-c064e8266569" />
<img width="2878" height="1459" alt="image" src="https://github.com/user-attachments/assets/dc460d54-3972-4a49-8f23-5d4c45663e2f" />

There is a caching mechanism in place for pushing layers too. Docker Hub already has all but one of the layers from an earlier push, so it only pushes the one layer that has changed.

When you change a layer, every layer built on top of that will have to be rebuilt. Each line in a Dockerfile builds a new layer that is built on the layer created from the lines before it. This is why the order of the lines in your Dockerfile is important. You optimized your Dockerfile so that the layer that is most likely to change (COPY app.py /app.py) is the last line of the Dockerfile. Generally for an application, your code changes at the most frequent rate. This optimization is particularly important for CI/CD processes where you want your automation to run as fast as possible.

## Understand image layers
<img width="2210" height="891" alt="image" src="https://github.com/user-attachments/assets/0506a53d-50e6-4a60-b02d-d63a047fa604" />
## Cleanup
<img width="2210" height="891" alt="image" src="https://github.com/user-attachments/assets/56b57f3d-6167-4361-ba22-b28de4f9ca17" />

## 📌 Key Concepts Covered

* Dockerfile instructions: `FROM`, `RUN`, `COPY`, `CMD`
* Using official Docker images
* Docker image layering and caching
* Running Python apps without Python installed locally
* Pushing images to Docker Hub
* Deploy a change

---

## ✅ Notes

* Use the Dockerfile to create reproducible builds for your application and to integrate your application with Docker into the CI/CD pipeline.
* Docker images can be made available to all of your environments through a central registry. The Docker Hub is one example of a registry, but you can deploy your own registry on servers you control.
* A Docker image contains all the dependencies that it needs to run an application within the image. This is useful because you no longer need to deal with environment drift (version differences) when you rely on dependencies that are installed on every environment you deploy to.
* Docker uses of the union file system and "copy-on-write" to reuse layers of images. This lowers the footprint of storing images and significantly increases the performance of starting containers.
* Image layers are cached by the Docker build and push system. There's no need to rebuild or repush image layers that are already present on a system.
* Each line in a Dockerfile creates a new layer, and because of the layer cache, the lines that change more frequently, for example, adding source code to an image, should be listed near the bottom of the file.










