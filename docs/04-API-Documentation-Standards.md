# API Documentation Standards (OpenAPI)

## 1. Introduction

**Project Title:** Interactive UFO/UAP Sightings Explorer & AI Analyst

This document defines the standards and conventions for creating the OpenAPI 3.0 specification for the backend API of the Interactive UFO/UAP Sightings Explorer. A well-defined OpenAPI specification is crucial as it serves as the contract between the Python backend (hosted on Google Cloud Run) and the Davia platform, which uses this specification to generate the frontend UI (hosted on Vercel).

Adherence to these standards will ensure clarity, consistency, and successful integration with the Davia ecosystem.

## 2. General Principles

*   **Clarity and Precision:** API paths, parameters, and schemas must be clearly and unambiguously defined.
*   **Consistency:** Use consistent naming conventions and data structures throughout the API.
*   **Completeness:** All API endpoints, request/response bodies, and authentication methods (if any, though not planned for this simple demo) must be documented.
*   **Davia Compatibility:** The OpenAPI specification must be structured in a way that Davia can interpret to generate the desired frontend components and interactions. This means focusing on how data inputs (for filtering, queries) and outputs (for display, visualizations) are represented.
*   **RESTful Design:** Follow REST principles for API design where applicable.

## 3. OpenAPI Specification Structure

The API will be documented in a single OpenAPI 3.0 JSON or YAML file. FastAPI can automatically generate this.

### 3.1. `info` Object
*   **`title`**: "Interactive UFO/UAP Sightings API"
*   **`version`**: e.g., "1.0.0"
*   **`description`**: A brief description of the API, e.g., "API for exploring and analyzing UFO/UAP sightings data. Powers the Davia-generated frontend."

### 3.2. `servers` Object
*   Define the server URL for the deployed backend on Google Cloud Run.
    ```json
    "servers": [
      {
        "url": "https://your-gcr-service-url.a.run.app/api/v1", // Example URL
        "description": "Production Server (Google Cloud Run)"
      }
    ]
    ```
    *(Note: A base path like `/api/v1` is good practice)*

### 3.3. `paths` Object
*   Each endpoint will be a key under `paths`.
*   Use descriptive, noun-based resource paths (e.g., `/sightings`, `/summary-metrics`, `/ai-query`).
*   HTTP methods (GET, POST) will be clearly defined for each path.

### 3.4. `components` Object
*   **`schemas`**: Define reusable data models for request and response bodies using Pydantic models in FastAPI, which translate to JSON Schema.
    *   Example: `SightingData`, `FilterCriteria`, `AIQueryRequest`, `AIQueryResponse`, `SummaryMetrics`.
    *   Clearly define data types, required fields, and example values.
*   **`parameters`**: Define reusable parameters (e.g., query parameters for filtering) if they are used across multiple endpoints.

## 4. Naming Conventions

*   **Paths:** Lowercase, hyphen-separated (kebab-case) for multi-word path segments (e.g., `/summary-metrics`).
*   **Query Parameters:** Lowercase, underscore-separated (snake_case) (e.g., `start_date`, `ufo_shape`).
*   **JSON Properties (Request/Response Bodies):** Lowercase, underscore-separated (snake_case), consistent with Python/Pydantic conventions.
*   **Schema Names (in `components.schemas`):** PascalCase (e.g., `SightingRecord`, `FilterOptions`).

## 5. Endpoint Design Guidelines

### 5.1. Data Exploration Endpoints
*   **`/sightings` (GET):**
    *   **Purpose:** Retrieve a list of sightings, potentially paginated and filtered.
    *   **Query Parameters:**
        *   `limit` (integer, default: 100): Max number of records to return.
        *   `offset` (integer, default: 0): Offset for pagination.
        *   `start_date` (string, format: date): Filter by start date.
        *   `end_date` (string, format: date): Filter by end date.
        *   `shape` (string): Filter by UFO shape.
        *   `state` (string): Filter by state.
        *   `country` (string): Filter by country.
        *   `sort_by` (string, e.g., "date_time"): Field to sort by.
        *   `sort_order` (string, enum: ["asc", "desc"], default: "desc"): Sort order.
    *   **Response:** A JSON object containing a list of sighting records and pagination details. The structure of sighting records should be well-defined in `components.schemas`.

*   **`/summary-metrics` (GET):**
    *   **Purpose:** Retrieve key summary statistics based on current filters.
    *   **Query Parameters:** Same as `/sightings` to apply consistent filtering.
    *   **Response:** A JSON object with metrics like `total_sightings`, `most_common_shape`, `busiest_month`.

*   **`/filter-options` (GET):**
    *   **Purpose:** Retrieve available distinct values for filter dropdowns (e.g., list of all unique shapes, states, countries present in the dataset).
    *   **Response:** A JSON object with arrays for `shapes`, `states`, `countries`.

### 5.2. AI Assistant Endpoint
*   **`/ai-query` (POST):**
    *   **Purpose:** Submit a natural language query to the AI assistant.
    *   **Request Body:**
        ```json
        {
          "query_text": "string"
          // Potentially include current filter context if AI needs it
        }
        ```
    *   **Response Body:**
        ```json
        {
          "answer_text": "string", // Textual answer
          "updated_filters": { /* object with filter values if query implies filter change */ },
          "visualization_hint": "string" // Optional: hint for frontend if a specific viz is suggested
        }
        ```

## 6. Schema Definitions (`components.schemas`)

*   Define clear schemas for all data structures.
*   **`SightingRecord`**:
    *   `date_time` (string, format: date-time)
    *   `city` (string)
    *   `state` (string)
    *   `country` (string)
    *   `shape` (string)
    *   `duration_seconds` (number)
    *   `comments` (string)
    *   `latitude` (number)
    *   `longitude` (number)
*   **`FilterCriteria` (for request bodies if complex, or use query params):**
    *   Mirrors the query parameters for `/sightings`.
*   Other schemas as needed for responses.

## 7. Descriptions and Examples

*   All paths, operations, parameters, and schemas should have clear `summary` and `description` fields.
*   Provide `example` values for request/response bodies and parameters to aid understanding and testing. This is particularly helpful for Davia's platform.

## 8. Versioning

*   API versioning will be done via a base path in the URL (e.g., `/api/v1`).

## 9. Error Handling

*   Define standard error response schemas.
*   Use appropriate HTTP status codes (e.g., 400 for bad requests, 404 for not found, 500 for server errors).
    ```json
    // Example Error Schema
    {
      "type": "object",
      "properties": {
        "error_code": { "type": "string" },
        "message": { "type": "string" },
        "details": { "type": "object", "additionalProperties": true }
      }
    }
    ```

## 10. Tooling

*   FastAPI will be used to auto-generate the `openapi.json` file from Python code and Pydantic models.
*   The generated `openapi.json` will be the source of truth for the API contract.
*   Tools like Swagger Editor or Stoplight Elements can be used to visualize and validate the OpenAPI specification during development.

By following these standards, the project will produce a high-quality OpenAPI specification that facilitates seamless integration with the Davia platform and promotes a well-structured backend API.
