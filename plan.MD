# Resource Integration Guide

This document serves as a comprehensive guide to defining, integrating, and developing plugins for various resource types (**RC**, **RX**, **RD**, **RE**) in your Agent system. The guide includes:

1. Detailed JSON structure for each resource type.
2. Example actions for API calls to handle resource-specific tasks.
3. Plugin definitions for each resource type.
4. A template for integrating plugins into Agents.
5. Development stage categorization (short-term and long-term).

---

## I. Resource Types

### 1. RC (Compute Resources)

#### **Purpose:**
Provide operations on compute resources (CPU, GPU, RAM), including running commands (`execute_cmd`), checking memory (`check_ram`), monitoring CPU (`check_cpu`), or performing calculations (`run_calculation`).

#### **JSON Definition:**
```json
{
  "type": "RC",
  "hardware": {
    "cpu": {
      "cores": "int",
      "usage_percentage": "float",
      "temperature": "float"
    },
    "gpu": {
      "model": "string",
      "usage_percentage": "float",
      "temperature": "float"
    },
    "ram": {
      "total": "int",
      "used": "int",
      "available": "int"
    }
  },
  "api_endpoints": {
    "execute_cmd": "/api/v1/compute/execute_cmd",
    "check_ram": "/api/v1/compute/check_ram",
    "check_cpu": "/api/v1/compute/check_cpu",
    "run_calculation": "/api/v1/compute/run_calculation"
  },
  "data": {
    "type": "json",
    "schema": {
      "task_id": "string",
      "command_output": "string",
      "resource_usage": {
        "cpu": "float",
        "gpu": "float",
        "ram": "float"
      }
    }
  }
}
```

#### **Attributes:**
- **type:** Fixed value "RC".
- **hardware:** Describes compute hardware (CPU, GPU, RAM), including details like cores, usage percentage, and temperature.
- **api_endpoints:** Contains API paths for actions.
  - `execute_cmd`: Executes terminal commands.
  - `check_ram`: Monitors RAM usage.
  - `check_cpu`: Monitors CPU usage and temperature.
  - `run_calculation`: Performs calculations.
- **data:**
  - `type`: Data format (JSON).
  - `schema`: Structure of data returned by the API.

#### **Development Stages:**
- **Short-Term:**
  - Implement `execute_cmd` for running terminal commands.
  - Enable `check_ram` and `check_cpu` to monitor hardware usage.
- **Long-Term:**
  - Develop predictive scaling and optimization for resource usage.
  - Integrate multi-node resource management across distributed systems.

#### **Actions:**
```typescript
const executeCmdAction: Action = {
  name: "executeCmd",
  description: "Execute a terminal command on the compute resource",
  examples: [{ command: "echo Hello World" }],
  handler: async (runtime, message, state) => {
    const { command } = message;
    const response = await runtime.apiCall({
      method: "POST",
      url: runtime.endpoints.execute_cmd,
      data: { command },
    });
    return response;
  },
};

const checkRamAction: Action = {
  name: "checkRam",
  description: "Check current RAM usage on the resource",
  handler: async (runtime, message, state) => {
    const response = await runtime.apiCall({
      method: "GET",
      url: runtime.endpoints.check_ram,
    });
    return response;
  },
};

export const computeResourcesPlugin = {
  name: "computeResourcesPlugin",
  actions: [executeCmdAction, checkRamAction],
  description: "Plugin to manage compute resources",
};
```

---

### 2. RX (Social Resources)

#### **Purpose:**
Interact with social media platforms (Twitter, Discord, Facebook). Support tasks such as posting content, commenting, analyzing sentiment, and scheduling posts.

#### **JSON Definition:**
```json
{
  "type": "RX",
  "platforms": ["Twitter", "Discord", "Facebook"],
  "auth": {
    "id_token": "string",
    "access_token": "string",
    "refresh_token": "string"
  },
  "api_endpoints": {
    "post": "/api/v1/social/post",
    "comment": "/api/v1/social/comment",
    "analyze_sentiment": "/api/v1/social/analyze_sentiment",
    "schedule_post": "/api/v1/social/schedule_post"
  },
  "data": {
    "type": "json",
    "schema": {
      "post_id": "string",
      "content": "string",
      "sentiment_score": "float",
      "timestamp": "string"
    }
  }
}
```

#### **Development Stages:**
- **Short-Term:**
  - Implement `analyze_sentiment` to analyze social media content.
  - Develop `postContent` for posting to platforms.
- **Long-Term:**
  - Add `schedule_post` for future post scheduling.
  - Enable advanced analytics for campaign insights and audience engagement.

#### **Actions:**
```typescript
const analyzeSentimentAction: Action = {
  name: "analyzeSentiment",
  description: "Analyze the sentiment of a given content",
  examples: [{ content: "I love AI so much!" }],
  handler: async (runtime, message, state) => {
    const { content } = message;
    const response = await runtime.apiCall({
      method: "POST",
      url: runtime.endpoints.analyze_sentiment,
      data: { content },
    });
    return response;
  },
};

const postContentAction: Action = {
  name: "postContent",
  description: "Post content to a specific social media platform",
  examples: [{ platform: "Twitter", content: "Hello World!" }],
  handler: async (runtime, message, state) => {
    const { platform, content } = message;
    const response = await runtime.apiCall({
      method: "POST",
      url: runtime.endpoints.post,
      data: { platform, content },
    });
    return response;
  },
};

export const socialResourcesPlugin = {
  name: "socialResourcesPlugin",
  actions: [analyzeSentimentAction, postContentAction],
  description: "Plugin to manage social media resources",
};
```

---

### 3. RD (Data Resources)

#### **Purpose:**
Manage and interact with various data sources (SQL databases, CSV files, Pandas DataFrames). Perform queries, data format conversions, describe data statistics, and more.

#### **JSON Definition:**
```json
{
  "type": "RD",
  "data_sources": {
    "sql": {
      "host": "string",
      "port": "int",
      "database": "string"
    },
    "csv": {
      "file_path": "string",
      "delimiter": "string"
    },
    "pandas": {
      "dataframe_name": "string"
    }
  },
  "api_endpoints": {
    "query": "/api/v1/data/query",
    "convert_csv_to_json": "/api/v1/data/convert_csv_to_json",
    "describe_dataframe": "/api/v1/data/describe_dataframe"
  },
  "data": {
    "type": ["sql", "csv", "pandas"],
    "schema": {
      "columns": ["string"],
      "data_types": ["string"]
    }
  }
}
```

#### **Development Stages:**
- **Short-Term:**
  - Enable `convert_csv_to_json` for CSV-to-JSON conversion.
  - Develop `query_database` for executing SQL queries.
- **Long-Term:**
  - Implement real-time data pipeline creation for ETL processes.
  - Add functionality for distributed data analysis.

#### **Actions:**
```typescript
const convertCsvToJsonAction: Action = {
  name: "convertCsvToJson",
  description: "Convert a CSV file to JSON format",
  examples: [{ file_path: "/path/to/data.csv" }],
  handler: async (runtime, message, state) => {
    const { file_path } = message;
    const response = await runtime.apiCall({
      method: "POST",
      url: runtime.endpoints.convert_csv_to_json,
      data: { file_path },
    });
    return response;
  },
};

const queryDatabaseAction: Action = {
  name: "queryDatabase",
  description: "Execute a SQL query on a database",
  examples: [{ query: "SELECT * FROM tableName" }],
  handler: async (runtime, message, state) => {
    const { query } = message;
    const response = await runtime.apiCall({
      method: "POST",
      url: runtime.endpoints.query,
      data: { query },
    });
    return response;
  },
};

export const dataResourcesPlugin = {
  name: "dataResourcesPlugin",
  actions: [convertCsvToJsonAction, queryDatabaseAction],
  description: "Plugin to manage data resources",
};
```

---

### 4. RE (Execution Resources)

#### **Purpose:**
Deploy and execute scripts within Docker containers or VMs. Check logs, handle container operations, etc.

#### **JSON Definition:**
```json
{
  "type": "RE",
  "runtime": {
    "docker": {
      "image": "string",
      "container_id": "string"
    },
    "vm": {
      "os": "string",
      "version": "string"
    }
  },
  "api_endpoints": {
    "deploy_docker": "/api/v1/exec/deploy_docker",
    "execute_script": "/api/v1/exec/execute_script",
    "check_logs": "/api/v1/exec/check_logs"
  },
  "data": {
    "type": "json",
    "schema": {
      "task_id": "string",
      "script_output": "string",
      "execution_status": "string"
    }
  }
}
```

#### **Development Stages:**
- **Short-Term:**
  - Implement `deploy_docker` for container deployment.
  - Develop `check_logs` to fetch logs from containers or VMs.
- **Long-Term:**
  - Add cluster orchestration with Kubernetes.
  - Develop rollback mechanisms for failed deployments.

#### **Actions:**
```typescript
const deployDockerAction: Action = {
  name: "deployDocker",
  description: "Deploy a Docker container",
  examples: [{ image: "nginx:latest", container_name: "web_server" }],
  handler: async (runtime, message, state) => {
    const { image, container_name } = message;
    const response = await runtime.apiCall({
      method: "POST",
      url: runtime.endpoints.deploy_docker,
      data: { image, container_name },
    });
    return response;
  },
};

const checkLogsAction: Action = {
  name: "checkLogs",
  description: "Fetch logs from a Docker container or VM",
  examples: [{ container_id: "abc123" }],
  handler: async (runtime, message, state) => {
    const { container_id } = message;
    const response = await runtime.apiCall({
      method: "GET",
      url: runtime.endpoints.check_logs,
      params: { container_id },
    });
    return response;
  },
};

export const executionResourcesPlugin = {
  name: "executionResourcesPlugin",
  actions: [deployDockerAction, checkLogsAction],
  description: "Plugin to manage execution resources like Docker containers and VMs",
};
```

---

## II. Agent Configuration Template

#### **Template for Agent JSON Configuration:**
```json
{
  "name": "RivalzAI",
  "plugins": [
    "computeResourcesPlugin",
    "socialResourcesPlugin",
    "dataResourcesPlugin",
    "executionResourcesPlugin"
  ],
  "clients": ["twitter"],
  "modelProvider": "super",
  "settings": {
    "secrets": {},
    "voice": {
      "model": "en_US-male-medium"
    }
  },
  "system": "Be a fun and curious assistant, specialized in AI and blockchain technologies.",
  "bio": ["RivalzAI works at Rivalz, constantly exploring how to apply AI and blockchain to create better systems."],
  "lore": ["Once stayed up three nights debugging a smart contract."],
  "adjectives": ["fun", "curious", "intelligent", "helpful", "enthusiastic"],
  "topics": ["AI", "blockchain", "technology trends", "smart contracts", "data science"]
}
```



---

### Development Stages Summary

#### **RX (Social Resources)**
- **Short-Term:**
  - `analyzeSentiment`: Analyze the sentiment of social media content.
  - `postContent`: Post content to a social media platform.
- **Long-Term:**
  - `schedulePost`: Schedule posts across multiple platforms.
  - `campaignAnalytics`: Provide insights into social media campaigns.

#### **RD (Data Resources)**
- **Short-Term:**
  - `convertCsvToJson`: Convert CSV files to JSON format.
  - `queryDatabase`: Execute SQL queries on a connected database.
- **Long-Term:**
  - `dataPipeline`: Create ETL pipelines for transforming and analyzing data.
  - `realTimeAnalytics`: Enable real-time data streaming and analysis.

#### **RC (Compute Resources)**
- **Short-Term:**
  - `executeCmd`: Execute commands on the system's terminal.
  - `checkRam`: Monitor current RAM usage.
- **Long-Term:**
  - `resourceOptimization`: Predict and optimize resource allocation.
  - `multiNodeManagement`: Manage resources across distributed systems.

#### **RE (Execution Resources)**
- **Short-Term:**
  - `deployDocker`: Deploy a Docker container.
  - `checkLogs`: Fetch logs from Docker containers or VMs.
- **Long-Term:**
  - `orchestrateCluster`: Orchestrate multi-container clusters using Kubernetes.
  - `autoRollback`: Automatically rollback failed deployments.

This guide categorizes the development stages for each action, helping prioritize immediate deliverables and plan future improvements efficiently. Let me know if there are any further adjustments required!

  