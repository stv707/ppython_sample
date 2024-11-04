
1. **Set Up the Python Application**: We'll create a simple Flask application that serves a web page.
2. **Test the Application Natively**: We'll run the application locally to ensure it works.
3. **Build a Docker Container**: Finally, we'll create a Docker container for the application.

### Step 1: Set Up the Python Application

1. **Create a new directory for the project**:
   ```bash
   mkdir flask_app
   cd flask_app
   ```

2. **Create a virtual environment (optional but recommended)**:
   ```bash
   python3.10 -m venv venv
   source venv/bin/activate  # On Windows use `venv\Scripts\activate`
   ```

3. **Install Flask**:
   ```bash
   pip install Flask
   ```

4. **Create a simple Flask application**:
   Create a file named `app.py` and add the following code:

   ```python
import os
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_world():
    name = os.getenv('NAME', 'World')  # Default to 'World' if NAME is not set
    return f'Hello, {name}! This is a simple Flask app!'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)

   ```

### Step 2: Test the Application Natively

1. **Run the Flask application**:
   ```bash
   python app.py
   ```

2. **Open a web browser** and navigate to `http://localhost:5000`. You should see the message: "Hello, World! This is a simple Flask app!"

### Step 3: Build a Docker Container

1. **Create a `Dockerfile`** in the same directory as `app.py`:

   ```Dockerfile
   # Use the official Python image from the Docker Hub
   FROM python:3.10-slim

   # Set the working directory in the container
   WORKDIR /app

   # Copy the current directory contents into the container at /app
   COPY . .

   # Install Flask
   RUN pip install Flask

   # Make port 5000 available to the world outside this container
   EXPOSE 5000

   # Define environment variable
   ENV NAME World

   # Run app.py when the container launches
   CMD ["python", "app.py"]
   ```

2. **Build the Docker image**:
   ```bash
   docker build -t flask_app .
   ```

3. **Run the Docker container**:
   ```bash
   docker run -p 5000:5000 flask_app
   ```

4. **Again, curl to this address ** and navigate to `http://localhost:5000`. You should see the same message from the Flask app, now running inside a Docker container.