# Resource and Budget Plan
## Project Title: Interactive UFO/UAP Sightings Explorer & AI Analyst

This document outlines the resource requirements and budget considerations for the Interactive UFO/UAP Sightings Explorer project. A primary goal of this project is to leverage free-tier services and open-source tools to minimize costs, aligning with the resourcefulness aspect often valued in data science roles and startup environments.

---

## 1. Human Resources

*   **Developer:** 1 (The applicant)
    *   **Role:** Full-stack development (data preprocessing, backend API, Davia SDK integration, deployment, testing, documentation).
    *   **Time Commitment:** As per the project schedule (estimated 7 - 10.5 work sessions). This is a time investment by the applicant.

## 2. Software & Tools

| Resource Category         | Tool/Software                                     | Cost      | Justification / Notes                                                                 |
|---------------------------|---------------------------------------------------|-----------|---------------------------------------------------------------------------------------|
| **Development Environment** |                                                   |           |                                                                                       |
|                           | Python 3.9+                                       | Free      | Core programming language.                                                            |
|                           | FastAPI                                           | Free      | Open-source web framework for Python backend.                                         |
|                           | Pandas, PyArrow                                   | Free      | Open-source libraries for data manipulation and Parquet file handling.                |
|                           | Uvicorn/Gunicorn                                  | Free      | Open-source ASGI server.                                                              |
|                           | Davia Python SDK                                  | Free      | Provided by Davia AI for the application. Assumed to be free for this purpose.        |
|                           | Visual Studio Code (or preferred IDE)             | Free      | Development environment.                                                              |
|                           | Docker Desktop                                    | Free      | For local containerization and testing. (Free for small businesses/personal use)      |
|                           | Git                                               | Free      | Open-source version control.                                                          |
| **Data Source**           |                                                   |           |                                                                                       |
|                           | NUFORC UFO Sightings Dataset (e.g., from Kaggle)  | Free      | Publicly available dataset.                                                           |
| **Cloud Services**        |                                                   |           |                                                                                       |
|                           | Google Cloud Platform (GCP)                       |           |                                                                                       |
|                           |   - Google Cloud Run                              | Free Tier*| Hosting for the containerized Python backend. Free tier typically includes generous monthly allowances for vCPU-time, memory-time, requests, and network egress. |
|                           |   - Google Container Registry / Artifact Registry | Free Tier*| Storing Docker images. Free tier for storage and data transfer usually available.     |
|                           |   - Google Cloud Logging/Monitoring               | Free Tier*| Basic logging and monitoring for Cloud Run services.                                  |
|                           | Vercel                                            | Free Tier | Hosting for the Davia-generated frontend. Hobby plan is free.                       |
| **Version Control Hosting** |                                                   |           |                                                                                       |
|                           | GitHub (or similar like GitLab, Bitbucket)        | Free      | Hosting the project's Git repository. Public repositories are free.                   |

**\*Note on Free Tiers:**
*   Google Cloud Platform offers an "Always Free" tier for many services, including Cloud Run and Artifact Registry, up to certain usage limits. It's crucial to stay within these limits to avoid charges. This typically involves:
    *   Configuring Cloud Run with `min-instances: 0` to scale to zero.
    *   Monitoring usage through the GCP console.
*   Vercel's "Hobby" plan is free and suitable for personal projects and demos.

## 3. Budget

*   **Software Costs:** $0 (All planned tools are open-source or have suitable free tiers for this project's scope).
*   **Cloud Hosting Costs:** $0 (Aiming to stay entirely within the free tiers of GCP and Vercel).
    *   **Contingency:** In the unlikely event that free tier limits are unexpectedly hit (e.g., due to extensive testing or an error in configuration), the cost would be minimal for a short-duration project. The developer would monitor this and shut down services if necessary.
*   **Data Acquisition Costs:** $0 (Public dataset).
*   **Human Resource Costs:** Not applicable in monetary terms for a job application project (time investment by the applicant).

## 4. Resource Management

*   **Time:** Managed according to the `UFO-UAP-Project-Schedule.md`.
*   **Code & Documentation:** Managed via Git and hosted on a platform like GitHub.
*   **Cloud Resources:** Managed via the respective GCP and Vercel dashboards/CLIs.
*   **Dataset:** The `nuforc_data.parquet` file will be relatively small and bundled with the backend deployment. No significant external data storage costs are anticipated.

## 5. Assumptions

*   The Davia Python SDK is provided free of charge for development and evaluation purposes as part of the job application.
*   The scale of the application (a single-user demo for evaluation) will comfortably fit within the free tier limits of GCP and Vercel.
*   No specialized paid software or services are required to meet the project objectives.

This resource and budget plan confirms the project's feasibility with minimal to no direct financial cost, relying on the applicant's time and the availability of free-tier cloud services and open-source software.
