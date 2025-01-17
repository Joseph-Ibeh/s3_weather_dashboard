# Weather Dashboard Project

The **Weather Dashboard** is a Python-based project that pulls weather data from the OpenWeather API and uploads it to an AWS S3 bucket. It is designed to provide a simple interface for displaying weather information for different cities and saving the results to the cloud.

This project leverages AWS S3 to store weather data, making it highly scalable, and uses Python to interact with the OpenWeather API.



## Table of Contents

- **Prerequisites**
- **Project Overview**
- **Core Features**
- **Tools Used**
- **Project Setup**
- **Environment Setup**
- **Running the Project**


## Prerequisites

Before starting this project, ensure you have the following:

1. **Python 3.x**: Install Python from the [official website](https://www.python.org/downloads/).
2. **AWS Account**: Create an AWS account to access AWS S3.
3. **OpenWeather API Key**: Obtain an API key from the [OpenWeather website](https://openweathermap.org/).
4. **AWS CLI Installed**: Download and install the AWS CLI tool from the [official website](https://aws.amazon.com/cli/).
5. **Basic Python Knowledge**: Familiarity with Python scripting, working with APIs, and environment variables.
6. **Text Editor or IDE**: Use a development environment like VS Code, PyCharm, or any text editor of your choice.
7. **Git Installed**: Ensure Git is installed for version control. Download it from the [Git website](https://git-scm.com/).


## Project Overview

This weather dashboard project fetches weather information for a given location using the **OpenWeather API**. The data is then uploaded to an **AWS S3 bucket** where it can be accessed remotely. The system is designed to be flexible, allowing users to specify different cities and view weather updates in real-time.

### Core Features

- Fetches weather data from OpenWeather API.
- Uploads weather data to an AWS S3 bucket.
- Allows you to configure API keys and AWS credentials securely via environment variables.



## Tools Used

This project utilizes several tools and libraries to build the dashboard:

- **Python 3.x**: The main programming language for the project.
- **boto3**: AWS SDK for Python used to interact with AWS S3.
- **python-dotenv**: A library to read environment variables from a `.env` file to keep sensitive data safe.
- **requests**: A simple HTTP library for making API requests to OpenWeather.
- **AWS CLI**: Command-line tool to interact with AWS services, such as configuring access keys and managing S3 buckets.



## Project Setup

Follow these steps to set up the project on your local machine.

### 1. Create Project Structure

Run the following commands to create the necessary directories and files:

```bash
mkdir weather_dashboard_demo
cd weather_dashboard_demo
mkdir src tests data
```

### 2. Create Files
Create the required Python files and configuration files:

```
touch src/__init__.py src/weather_dashboard.py
touch requirements.txt README.md .env
```

### 3.  Initialize Git Repository
Initialize a new Git repository and set up the main branch:

```
git init
git branch -M main
```

### 4. Configure .gitignore
Create a .gitignore file to ignore unnecessary files:

```
echo ".env" >> .gitignore
echo "__pycache__/" >> .gitignore
echo "*.zip" >> .gitignore
```

### 5. Add Dependencies
Add the required dependencies to requirements.txt:

```
echo "boto3==1.26.137" >> requirements.txt
echo "python-dotenv==1.0.0" >> requirements.txt
echo "requests==2.28.2" >> requirements.txt
```

### 6. Install Dependencies
Run the following command to install the dependencies from requirements.txt:

```
pip install -r requirements.txt
```

## Environment Setup
### 1. AWS CLI Configuration
Before interacting with AWS services like S3, you need to configure your AWS CLI with your access keys. This allows the Python script to communicate with AWS securely.
To configure AWS CLI, run the following command:

Run:

```
aws configure

```

This will prompt you to enter your AWS credentials. You'll need your AWS Access Key ID and AWS Secret Access Key, which you can get from your AWS Management Console under IAM > Users > Your User > Security Credentials.

You will also be prompted for the default region and default output format:

- **AWS Access Key ID:** Your unique key that identifies your AWS account.
- **AWS Secret Access Key:** A secret key that corresponds to your AWS Access Key ID.
- **Default region name:** This is typically something like us-east-1, us-west-2, etc. You can check the AWS S3 bucket region to match this.
- **Default output format:** Choose json for the output format, or leave it as the default (None).

### Example:

```
$ aws configure
AWS Access Key ID [None]: your-access-key-id
AWS Secret Access Key [None]: your-secret-access-key
Default region name [None]: us-east-1
Default output format [None]: json
```


Check if AWS CLI is installed correctly by running:

```
aws --version
```

### 2. Configure .env
Create a .env file to store sensitive information like your API keys and AWS bucket name. Add the following entries:

```
echo "OPENWEATHER_API_KEY=your key here" >> .env
echo "AWS_BUCKET_NAME=weather-dashboard-joseph" >> .env
```

Make sure to replace your key here with your actual OpenWeather API key and weather-dashboard-joseph with your S3 bucket name.

## Running the Project

### 1. Run the Script
Once the setup is complete, you can run the weather_dashboard.py script to fetch weather data and upload it to the AWS S3 bucket:

```
python src/weather_dashboard.py
```

This will pull the latest weather data from the OpenWeather API and upload it to your AWS S3 bucket.

### 2. Verify Your S3 Bucket
Log in to your AWS account and navigate to your S3 bucket to verify that the weather data has been successfully uploaded. If you don't want to accumulate unnecessary costs, be sure to delete the contents of your bucket after verification.




