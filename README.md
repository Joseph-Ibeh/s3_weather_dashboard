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

[![file structure](https://github.com/Joseph-Ibeh/s3_weather_dashboard/blob/main/screenshots/file%20_structure.png)]                     

### 1. Create Project Structure

Run the following commands to create the necessary directories and files:

```bash
mkdir weather_dashboard_demo
cd weather_dashboard_demo
mkdir src tests data
```
[![mkdir](https://github.com/Joseph-Ibeh/s3_weather_dashboard/blob/main/screenshots/mkdir_cd.png)]

[![src tests data](https://github.com/Joseph-Ibeh/s3_weather_dashboard/blob/main/screenshots/src_tests_data.png)]


### 2. Create Files
Create the required Python files and configuration files:

```
touch src/__init__.py src/weather_dashboard.py
touch requirements.txt README.md .env
```

[![init py](https://github.com/Joseph-Ibeh/s3_weather_dashboard/blob/main/screenshots/touchsrc-init__py.png)]

[![touch requiremnents ](https://github.com/Joseph-Ibeh/s3_weather_dashboard/blob/main/screenshots/touch%20requirement_txt.png)]

### 3.  Initialize Git Repository
Initialize a new Git repository and set up the main branch:

```
git init
git branch -M main
```
[![git init ](https://github.com/Joseph-Ibeh/s3_weather_dashboard/blob/main/screenshots/git_init.png)]   

[![touch requiremnents ](https://github.com/Joseph-Ibeh/s3_weather_dashboard/blob/main/screenshots/git_branch.png)]

### 4. Configure .gitignore
Create a .gitignore file to ignore unnecessary files:

```
echo ".env" >> .gitignore
echo "__pycache__/" >> .gitignore
echo "*.zip" >> .gitignore
```

[![gitignore](https://github.com/Joseph-Ibeh/s3_weather_dashboard/blob/main/screenshots/git_ignore.png)]


### 5. Add Dependencies
Add the required dependencies to requirements.txt:

```
echo "boto3==1.26.137" >> requirements.txt
echo "python-dotenv==1.0.0" >> requirements.txt
echo "requests==2.28.2" >> requirements.txt
```
[![bot3](https://github.com/Joseph-Ibeh/s3_weather_dashboard/blob/main/screenshots/echo_boto_python_requirements.png)]

[![request](https://github.com/Joseph-Ibeh/s3_weather_dashboard/blob/main/screenshots/echo_request_requirements.png)]


### 6. Install Dependencies
Run the following command to install the dependencies from requirements.txt:

```
pip install -r requirements.txt
```
[![dependencies](https://github.com/Joseph-Ibeh/s3_weather_dashboard/blob/main/screenshots/pip_install_requirements.png)]



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
Here is the script that fetches weather data and uploads it to AWS S3:

```
import os
import json
import boto3
import requests
from datetime import datetime
from dotenv import load_dotenv

# Load environment variables
load_dotenv()

class WeatherDashboard:
    def __init__(self):
        self.api_key = os.getenv('OPENWEATHER_API_KEY')
        self.bucket_name = os.getenv('AWS_BUCKET_NAME')
        self.s3_client = boto3.client('s3')

    def create_bucket_if_not_exists(self):
        """Create S3 bucket if it doesn't exist"""
        try:
            self.s3_client.head_bucket(Bucket=self.bucket_name)
            print(f"Bucket {self.bucket_name} exists")
        except:
            print(f"Creating bucket {self.bucket_name}")
        try:
            # Simpler creation for us-east-1
            self.s3_client.create_bucket(Bucket=self.bucket_name)
            print(f"Successfully created bucket {self.bucket_name}")
        except Exception as e:
            print(f"Error creating bucket: {e}")

    def fetch_weather(self, city):
        """Fetch weather data from OpenWeather API"""
        base_url = "http://api.openweathermap.org/data/2.5/weather"
        params = {
            "q": city,
            "appid": self.api_key,
            "units": "imperial"
        }
        
        try:
            response = requests.get(base_url, params=params)
            response.raise_for_status()
            return response.json()
        except requests.exceptions.RequestException as e:
            print(f"Error fetching weather data: {e}")
            return None

    def save_to_s3(self, weather_data, city):
        """Save weather data to S3 bucket"""
        if not weather_data:
            return False
            
        timestamp = datetime.now().strftime('%Y%m%d-%H%M%S')
        file_name = f"weather-data/{city}-{timestamp}.json"
        
        try:
            weather_data['timestamp'] = timestamp
            self.s3_client.put_object(
                Bucket=self.bucket_name,
                Key=file_name,
                Body=json.dumps(weather_data),
                ContentType='application/json'
            )
            print(f"Successfully saved data for {city} to S3")
            return True
        except Exception as e:
            print(f"Error saving to S3: {e}")
            return False

def main():
    dashboard = WeatherDashboard()
    
    # Create bucket if needed
    dashboard.create_bucket_if_not_exists()
    
    cities = ["Philadelphia", "Seattle", "New York"]
    
    for city in cities:
        print(f"\nFetching weather for {city}...")
        weather_data = dashboard.fetch_weather(city)
        if weather_data:
            temp = weather_data['main']['temp']
            feels_like = weather_data['main']['feels_like']
            humidity = weather_data['main']['humidity']
            description = weather_data['weather'][0]['description']
            
            print(f"Temperature: {temp}°F")
            print(f"Feels like: {feels_like}°F")
            print(f"Humidity: {humidity}%")
            print(f"Conditions: {description}")
            
            # Save to S3
            success = dashboard.save_to_s3(weather_data, city)
            if success:
                print(f"Weather data for {city} saved to S3!")
        else:
            print(f"Failed to fetch weather data for {city}")

if __name__ == "__main__":
    main()
```

[![python script in env](https://github.com/Joseph-Ibeh/s3_weather_dashboard/blob/main/screenshots/python_script.png)


### 1. Run the Script
Once the setup is complete, you can run the weather_dashboard.py script to fetch weather data and upload it to the AWS S3 bucket:

```
python src/weather_dashboard.py
```


This will pull the latest weather data from the OpenWeather API and upload it to your AWS S3 bucket.

[![output](https://github.com/Joseph-Ibeh/s3_weather_dashboard/blob/main/screenshots/python_weather_output.png)   



### 2. Verify Your S3 Bucket
Log in to your AWS account and navigate to your S3 bucket to verify that the weather data has been successfully uploaded. If you don't want to accumulate unnecessary costs, be sure to empty and delete the contents of your bucket after verification.

[![s3 otput 1](https://github.com/Joseph-Ibeh/s3_weather_dashboard/blob/main/screenshots/s3_bucket_output_1.png) 
[![s3 output 2](https://github.com/Joseph-Ibeh/s3_weather_dashboard/blob/main/screenshots/s3_output_2.png) 




