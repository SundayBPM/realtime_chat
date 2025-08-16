# AI Voice Agents API Documentation

This document outlines the requirements and API endpoints for creating, managing, and monitoring AI Voice Agents. All successful API responses follow the standard response contract defined in Section 5.

---

## 1. Agent Management

### 1.1. Create AI Agent

This endpoint creates a new AI Voice Agent. A `tenant_id` is required for all agent-related operations and is established during creation.

* **Endpoint:** `POST /v1/ai-agents/create`
* **Description:** Before any operations can be performed, an AI Voice Agent must be created with an associated `tenant_id`. This ID is supplied in the request payload.

#### Request Payload
```json
{
  "tenant_id": "1c21d34a-50ac-4326-a5de-b37911e0391a",
  "name": "CS-01"
}
```
> **Note:** The `tenant_id` must be a valid UUID.

#### Backend Mapping
The service maps this request to the ElevenLabs API, combining the `name` and `tenant_id` to create a unique agent name.

```bash
curl -X POST [https://api.elevenlabs.io/v1/convai/agents/create](https://api.elevenlabs.io/v1/convai/agents/create) \
     -H "xi-api-key: <YOUR_API_KEY>" \
     -H "Content-Type: application/json" \
     -d '{
  "conversation_config": {},
  "name": "CS-01/1c21d34a-50ac-4326-a5de-b37911e0391a"
}'
```

#### Database Schema (`ai_agents` table)

| Column    | Type   | Description                               |
| :-------- | :----- | :---------------------------------------- |
| tenant_id | UUID   | The unique identifier for the tenant.     |
| agent_id  | String | The ID returned from the ElevenLabs API.  |
| name      | String | The name assigned to the agent.           |

#### Example Response
The API returns the `agent_id` and the composite `name` after successful creation, wrapped in the standard response structure.

```json
{
    "data": {
        "agent_id": "J3Pbu5gP6NNKBscdCdwB",
        "name": "CS-01/1c21d34a-50ac-4326-a5de-b37911e0391a"
    },
    "version": "1.0"
}
```

---

### 1.2. Get AI Agent Details

This endpoint retrieves the details of a specific AI agent using its `tenant_id`.

* **Endpoint:** `GET /v1/ai-agents/:tenant_id`

#### Workflow
1.  The system checks if the provided `tenant_id` exists in the `ai_agents` database table.
2.  If the `tenant_id` is not found, the API returns a `401 Unauthorized` error.
3.  If found, the system uses the corresponding `agent_id` to fetch detailed information from the ElevenLabs API and returns a `200 OK` response.

#### Backend Query
```sql
SELECT * FROM ai_agents WHERE tenant_id = :tenant_id;
```

#### Fetch from External API
```bash
curl [https://api.elevenlabs.io/v1/convai/agents/:agent_id](https://api.elevenlabs.io/v1/convai/agents/:agent_id) \
     -H "xi-api-key: <YOUR_API_KEY>"
```

#### Example Response
The response contains the full configuration and details of the agent.

```json
{
    "data": {
        "agent_id": "J3Pbu5gP6NNKBscdCdwB",
        "name": "CS-01/1c21d34a-50ac-4326-a5de-b37911e0391a",
        "system_instruction": "You are a helpful customer service assistant.",
        "greeting_message": "Hello!",
        "default_language": "en",
        "supported_languages": [
            "en-US",
            "id-ID"
        ],
        "temperature": 0.7,
        "voice_config": {
            "gender": "female",
            "speaking_speed": 0.5,
            "keywords": [
                "customer service",
                "support",
                "help"
            ]
        },
        "created_at": "2025-08-13T14:46:40.309Z",
        "updated_at": "2025-08-13T14:46:40.309Z"
    },
    "version": "1.0"
}
```

---

## 2. Configuration

### 2.1. Update Voice Configuration

This endpoint allows for updating the agent's voice, language, and conversational settings.

* **Endpoint:** `PUT /v1/ai-agents/:tenant_id/voice-config`

#### Request Payload
The payload contains various settings that map to the external API's configuration.

```json
{
    "system_instruction": "You are a helpful customer service assistant.",
    "greeting_message": "Hello!",
    "default_language": "en",
    "supported_languages": [
        "en-US",
        "id-ID"
    ],
    "temperature": 0.7,
    "voice_config": {
        "gender": "female",
        "speaking_speed": 0.5,
        "keywords": ["customer service", "support", "help"]
    }
}
```
* **Mapping:**
    * `system_instruction` is mapped to the `prompt` field.
    * `greeting_message` is mapped to the `first_message` field.

---

## 3. Knowledge Base

### 3.1. Add to Knowledge Base

This endpoint adds documents or articles to the agent's knowledge base.

* **Endpoint:** `POST /v1/ai-agents/:tenant_id/knowledge-base`

#### Request Payload
The payload is an array of objects, each representing a piece of content.

```json
[
  {
      "id": "3be1b2db-e311-4586-a300-3421087dfd69",
      "title": "My Info",
      "content": "I am the wind that is born from the air.",
      "created_at": "2025-07-16T13:08:12.433Z",
      "updated_at": "2025-07-16T13:08:12.433Z"
  },
  {
      "id": "d0d4a331-feb9-4f14-8f2c-2f701b1bad94",
      "title": "Testing",
      "content": "This is a test.",
      "created_at": "2025-08-13T15:03:53.458Z",
      "updated_at": "2025-08-13T15:03:53.458Z"
  }
]
```

---

## 4. Analytics and History

### 4.1. Get Performance Metrics

This endpoint retrieves performance metrics for the agent over a specified period.

* **Endpoint:** `GET /v1/ai-agents/:tenant_id/performance`

#### Example Response
```json
{
    "data": {
        "total_sessions": 0,
        "avg_session_duration": 180,
        "success_rate": 100,
        "total_messages": 0,
        "user_messages": 0,
        "assistant_messages": 0,
        "languages": [],
        "period": "7d",
        "generated_at": "2025-08-13T15:07:56.806Z"
    },
    "version": "1.0"
}
```

### 4.2. Get Conversation History

This endpoint fetches the conversation history for a given agent.

* **Endpoint:** `GET /v1/ai-agents/:tenant_id/history`

#### Example Response
The response is an array of session objects, wrapped in the standard response structure.

```json
{
    "data": [
        {
            "session_id": "3be1b2db-e311-4586-a300-3421087dfd69",
            "messages": [
                {
                   "user_message": "Hi",
                   "assistant_message": "Hello!"
                },
                {
                   "user_message": "What do you want?",
                   "assistant_message": "What are you asking?"
                }
            ]
        }
    ],
    "version": "1.0"
}
```

---

## 5. Standard Response Contract

This section defines the standard data structure for all successful API responses. The main content is wrapped within the `data` object.

```json
{
    "data": {
        "...": "..."
    },
    "version": "1.0"
}
