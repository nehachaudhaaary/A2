# A2
# Step 1: Clone the repository and set up the project
!git clone https://github.com/Omniplex-ai/omniplex.git
!cd omniplex && pip install -r requirements.txt

# Step 2: Enhance UI
# Assuming the file to modify is `templates/login.html`
ui_code = """
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login - Omniplex</title>
    <link rel="stylesheet" href="/static/style.css">
</head>
<body>
    <div class="login-container">
        <form action="/login" method="POST" class="login-form">
            <h2>Welcome Back</h2>
            <p>Please login to your account</p>
            <input type="text" name="username" placeholder="Username" required>
            <input type="password" name="password" placeholder="Password" required>
            <button type="submit">Login</button>
        </form>
    </div>
</body>
</html>
"""
with open('omniplex/templates/login.html', 'w') as f:
    f.write(ui_code)

# Step 3: Integrate a Public API from RapidAPI
import requests
from flask import Flask, request, render_template

app = Flask(__name__)

@app.route('/api/joke', methods=['GET'])
def get_joke():
    url = "https://jokeapi-v2.p.rapidapi.com/joke/Any"
    headers = {
        "X-RapidAPI-Key": "your-rapidapi-key",
        "X-RapidAPI-Host": "jokeapi-v2.p.rapidapi.com"
    }
    response = requests.get(url, headers=headers)
    if response.status_code == 200:
        return response.json()
    return {"error": "Unable to fetch joke"}

# Step 4: Update routes to display API data
@app.route('/jokes')
def show_jokes():
    joke_data = get_joke()
    return render_template('jokes.html', joke=joke_data.get('joke', 'No joke available'))

# Step 5: Deploy on Azure
# Dockerfile
dockerfile_content = """
# Use Python base image
FROM python:3.8-slim

# Set working directory
WORKDIR /app

# Install dependencies
COPY requirements.txt .
RUN pip install -r requirements.txt

# Copy project files
COPY . .

# Set the command to run the Flask app
CMD ["python", "app.py"]
"""
with open('omniplex/Dockerfile', 'w') as f:
    f.write(dockerfile_content)

# Azure deployment script (CLI-based example)
azure_deployment_script = """
az webapp up --name omniplex-enhanced --resource-group your-resource-group --plan your-app-service-plan --runtime "PYTHON|3.8"
"""

# Step 6: Claude.ai Agent (Bonus Task)
claude_agent_code = """
from selenium import webdriver
from selenium.webdriver.common.by import By

# Configure Selenium WebDriver
driver = webdriver.Chrome()

# Navigate to the deployed website
driver.get("https://your-azure-deployed-url.com")

# Simulate interactions
driver.find_element(By.ID, 'login-username').send_keys('testuser')
driver.find_element(By.ID, 'login-password').send_keys('password123')
driver.find_element(By.ID, 'login-button').click()

# Navigate to joke page
driver.get("https://your-azure-deployed-url.com/jokes")

# Print joke to console
print(driver.find_element(By.CLASS_NAME, 'joke-text').text)

driver.quit()
"""

# Save agent script
with open('claude_agent.py', 'w') as f:
    f.write(claude_agent_code)

# Save deployment notes
readme_content = """
# Omniplex Enhancement Project

## Setup Instructions
1. Clone the repository.
2. Run `pip install -r requirements.txt`.
3. Modify `.env` with your API keys.
4. Deploy using Docker and Azure CLI.

## Features
1. Enhanced login page.
2. Joke API integration.
3. Deployed on Azure.

## Demo
Run `claude_agent.py` for agent navigation.
"""
with open('omniplex/README.md', 'w') as f:
    f.write(readme_content)
