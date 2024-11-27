Running a Jupyter Notebook with a "Hello World" Python script via a development container using a Dockerfile involves several steps. Below is a comprehensive guide to setting it up.

---

### 1. **Create the Project Directory**

Create a directory for your project and navigate into it:

```bash
mkdir jupyter-docker
cd jupyter-docker
```

---

### 2. **Write the `Dockerfile`**

Create a `Dockerfile` in the directory with the following content:

```dockerfile
# Use an official Python base image
FROM python:3.10-slim

# Set the working directory
WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Jupyter Notebook
RUN pip install --no-cache-dir jupyter

# Expose the port Jupyter runs on
EXPOSE 8888

# Set the command to start Jupyter
CMD ["jupyter", "notebook", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root"]
```

---

### 3. **Build and Run the Docker Container**

1. Build the Docker image:
   ```bash
   docker build -t jupyter-docker .
   ```

2. Run the container:
   ```bash
   docker run -p 8888:8888 -v $(pwd):/app jupyter-docker
   ```

3. Copy the link with the token displayed in the terminal (it looks like `http://127.0.0.1:8888/?token=...`).

4. Open the link in your browser to access Jupyter Notebook.

---

### 4. **Create and Run the "Hello World" Notebook**

1. In Jupyter, create a new notebook (Python 3).
2. Add the following code in a cell:
   ```python
   print("Hello, World!")
   ```
3. Run the cell to see the output.

---

### 5. **Optional: Use a `devcontainer.json` for VS Code**

If you're using Visual Studio Code and want to use a Dev Container, add a `.devcontainer` directory with a `devcontainer.json` file:

```json
{
    "name": "Jupyter Notebook Dev Container",
    "build": {
        "dockerfile": "Dockerfile"
    },
    "forwardPorts": [8888],
    "mounts": [
        "source=${localWorkspaceFolder},target=/app,type=bind"
    ]
}
```

Now, open the folder in VS Code, and when prompted, reopen it in the container.

---

### 6. **Verify the Setup**

- Open `localhost:8888` in your browser.
- Ensure your notebook runs the "Hello, World!" script successfully.

This setup provides a clean, containerized environment for running Jupyter Notebook.