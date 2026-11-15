# GitHub Actions CI/CD – ASP.NET Core 10 on IIS

## Overview

This project demonstrates an end-to-end CI/CD pipeline using GitHub Actions to build, publish, and deploy an ASP.NET Core 10 web application to an on-premise IIS server.

The solution combines GitHub-hosted runners for application compilation with a self-hosted Windows runner for deployment to local infrastructure.

## Architecture

```text
                  GitHub Repository
                         |
                         v
                  GitHub Actions
                         |
                 +-------+-------+
                 |               |
                 v               |
          Ubuntu Runner          |
                 |               |
          Restore / Build        |
                 |               |
              Publish            |
                 |               |
           Upload Artifact      |
                 |               |
                 v               |
          GitHub Artifact        |
                 |               |
                 v               |
          Windows 11 Runner <----+
                 |
          Download Artifact
                 |
          Stop IIS App Pool
                 |
          Deploy Application
                 |
          Start IIS App Pool
                 |
          HTTP Health Check
                 |
                 v
             IIS Server
          ASP.NET Core 10
```

## Technologies

- ASP.NET Core 10
- C#
- GitHub Actions
- GitHub-hosted runners
- Self-hosted Windows runner
- Windows 11
- Internet Information Services (IIS)
- PowerShell
- Robocopy
- Git

## CI Pipeline – Build and Publish

The build job runs on an Ubuntu GitHub-hosted runner.

Main tasks:

1. Checkout source code.
2. Configure .NET 10 SDK.
3. Restore NuGet dependencies.
4. Build the application in Release configuration.
5. Publish the application.
6. Upload the published files as a GitHub Actions artifact.

Artifact name: `webapp-publish`

## CD Pipeline – IIS Deployment

The deployment job runs on a self-hosted Windows 11 runner.

Main tasks:

1. Download the build artifact.
2. Prepare the deployment directory.
3. Stop the IIS Application Pool.
4. Copy published files using Robocopy.
5. Start the IIS Application Pool.
6. Perform an HTTP health check.

## IIS Configuration

| Parameter | Value |
|---|---|
| Application | DevOpsLab |
| Framework | ASP.NET Core 10 |
| Application Pool | DevOpsLab |
| Deployment path | C:\inetpub\wwwroot\DevOpsLab |
| HTTP binding | localhost:8085 |

## Deployment Validation

After deployment, the pipeline performs an HTTP request to verify application availability.

Expected response:

```text
HTTP 200 OK
```

Application URL:

`http://localhost:8085`

## Repository Structure

```text
github-actions-iis-cicd/
│
├── .github/
│   └── workflows/
│       ├── build.yml
│       └── test-runner.yml
│
├── src/
│   └── WebApp/
│
└── README.md
```

## Project Scope

This is a DevOps laboratory project focused on CI/CD automation and on-premise application deployment.

Future improvements:

- QA and Production deployment environments.
- GitHub Environments and deployment approvals.
- Automated backup and rollback.
- Automated application tests.
- SQL Server deployment and database migrations.
- Improved runner security and permissions.

## Author

DevOps CI/CD Laboratory – GitHub Actions and IIS.