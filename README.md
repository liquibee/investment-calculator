# investment-calculator (Financial Future Planner)

## Table of Contents
- [investment-calculator (Financial Future Planner)](#investment-calculator-financial-future-planner)
  - [Table of Contents](#table-of-contents)
  - [Project Overview](#project-overview)
    - [Scope](#scope)
  - [Features](#features)
  - [Technology Stack](#technology-stack)
  - [Project Structure](#project-structure)
  - [Local Development](#local-development)
    - [Running with Docker (alternative for above)](#running-with-docker-alternative-for-above)
  - [Deployment](#deployment)
  - [CI/CD Pipeline](#cicd-pipeline)
    - [Setting Up the CI/CD Pipeline](#setting-up-the-cicd-pipeline)
- [Contributing](#contributing)
- [License](#license)

## Project Overview

The Financial Future Planner is an AI-assisted, rapidly prototyped web application designed to help users visualize and plan their financial future. This tool demonstrates the power of AI in accelerating web development and prototyping processes.

Created over a weekend as an experiment in AI-powered rapid development, this project showcases how complex financial planning tools can be built quickly and efficiently using modern AI assistants like Anthropic's Sonnet 3.5 and OpenAI's ChatGPT.

### Scope

- Provide a user-friendly interface for inputting financial parameters
- Calculate and display long-term financial projections
- Visualize financial growth through interactive charts
- Offer customizable settings for different currencies and number formats
- Demonstrate the capabilities of AI-assisted rapid prototyping in web development

## Features

- Dynamic financial calculations based on user inputs
- Interactive chart for visualizing financial growth
- Customizable parameters including initial investment, returns, taxes, and withdrawals
- Support for multiple currencies and number formats
- Responsive design for desktop and mobile devices
- Downloadable CSV reports of financial projections
- Detailed FAQ section for user guidance

## Technology Stack

- HTML5
- Vanilla JavaScript
- Tailwind CSS for styling
- Chart.js for data visualization
- Font Awesome for icons
- Cloudflare Pages for hosting and deployment

## Project Structure
```
financial-future-planner/
│
├── site/
│   ├── index.html
│   ├── style.css (if not using Tailwind CDN)
│   ├── script.js
│   └── images/
│       └── logo.png
│
├── public/ (generated for deployment)
│
├── .github/
│   └── workflows/
│       └── deploy.yml
│
├── conf/
│   └── nginx.conf (if using Nginx)
│
└── README.md
```

## Local Development

To run this project locally:

1. Clone the repository:
   
   ``` 
   git clone https://github.com/liquibee/investment-calculator.git 
   ```

2. Navigate to the project directory:
   ```
   cd investment-calculator
   ```

3. Open `site/index.html` in your web browser.

No build process is required as the project uses vanilla HTML, JavaScript, and CSS via CDN.

If you're using Nginx for local development, you can use the following configuration to ensure proper navigation:

```nginx
server {
 listen 80;
 server_name localhost;

 root /path/to/your/investment-calculator/site;
 index index.html;

 location / {
     try_files $uri $uri/ /index.html;
 }
}
```

Replace /path/to/your/investment-calculator/site with the actual path to your project's site directory. This configuration will allow Nginx to properly handle client-side routing, ensuring that all navigation within the app works correctly.

After adding this configuration to your Nginx sites-available directory and enabling it, restart Nginx:

```
sudo nginx -t
sudo systemctl restart nginx
```

Then, hopefully, you can access your project at http://localhost in your web browser.

### Running with Docker (alternative for above)

To run the project using Docker and Nginx:

1. Make sure you have Docker installed on your system.

2. Build and run the Docker container:
```
docker run --name financial-planner -v $(pwd)/site:/usr/share/nginx/html:ro -v $(pwd)/conf/nginx.conf:/etc/nginx/conf.d/default.conf:ro -p 8080:80 -d nginx
```
This command does the following:
- Names the container "financial-planner"
- Mounts the `site` directory to Nginx's HTML directory (read-only)
- Mounts the `conf/nginx.conf` file to Nginx's configuration directory (read-only)
- Maps port 8080 on your host to port 80 in the container
- Runs the container in detached mode

3. Access the application by navigating to `http://localhost:8080` in your web browser.

To stop the container:
```
docker stop financial-planner
```

To delete/remove the container:
```
docker rm financial-planner
```

## Deployment

This project is deployed using Cloudflare Pages. The deployment is automated through a GitHub Actions workflow.

## CI/CD Pipeline

The CI/CD pipeline is set up using GitHub Actions and Cloudflare Pages. Here's an overview of the process:

1. **Trigger**: The workflow is triggered on every push to the repository.

2. **Build**: The contents of the `site/` directory are copied to a `public/` directory.

3. **Deploy**: The contents of the `public/` directory are deployed to Cloudflare Pages.

### Setting Up the CI/CD Pipeline

1. **Cloudflare Setup**:
- Create a Cloudflare account if you don't have one.
- Set up a new project in Cloudflare Pages.
- Note down your Cloudflare Account ID and generate an API token with appropriate permissions.

2. **GitHub Secrets**:
Add the following secrets to your GitHub repository:
- `CLOUDFLARE_API_TOKEN`: Your Cloudflare API token
- `CLOUDFLARE_ACCOUNT_ID`: Your Cloudflare account ID

3. **Workflow File**:
Create a file named `deploy.yml` in the `.github/workflows/` directory of your repository with the following content:

```yaml
name: Deploy Website

on:
  push:

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Build website
        run: |
          cp -r site/ public/

      - name: Publish
        uses: cloudflare/pages-action@1
        with:
          apiToken: ${{ secrets.CLOUDFLARE_API_TOKEN }}
          accountId: ${{ secrets.CLOUDFLARE_ACCOUNT_ID }}
          projectName: invest-calculator
          directory: public 
          gitHubToken: ${{ secrets.GITHUB_TOKEN }}
```

Make sure to replace invest-calculator with your Cloudflare Pages project name if it's different.

Commit and Push: Commit the workflow file and push it to your repository. This will trigger the deployment process.

# Contributing
Contributions to the Financial Future Planner are welcome! Please feel free to submit a Pull Request.

# License

This project is licensed under the Financial Future Planner License. This license allows for personal and educational use but restricts commercial use. For the full license text, see the [LICENSE](LICENSE) file in this repository.

For commercial licensing opportunities, please contact sistemica GmbH at info@sistemica.de.