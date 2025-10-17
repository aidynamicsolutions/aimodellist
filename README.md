# aimodellist

A centralized repository for AI model metadata designed to be consumed by iOS applications and other clients.

## Overview

This repository provides a structured JSON API containing curated AI model information, making it easy for applications to dynamically discover and load machine learning models. The models are optimized for iOS performance and include various capabilities from standard inference to advanced reasoning.

## Model Catalog

Currently supports models from the MLX community with different specializations:

### Regular Models
- **Llama-3.2-1B-Instruct-4bit** (0.7 GB) - Default model for general tasks
- **Llama-3.2-3B-Instruct-4bit** (1.8 GB) - Larger general-purpose model

### Reasoning Models
- **DeepSeek-R1-Distill-Qwen-1.5B-4bit** (1.0 GB) - Compact reasoning model
- **DeepSeek-R1-Distill-Qwen-1.5B-8bit** (1.9 GB) - Higher precision reasoning
- **Qwen3-4B-4bit** (2.3 GB) - Medium reasoning model
- **Qwen3-8B-4bit** (4.7 GB) - Large reasoning model

## JSON API Structure

```json
{
  "version": "1.0.0",
  "lastUpdated": "2025-01-17T00:00:00Z",
  "models": [
    {
      "id": "model/identifier",
      "displayName": "human-readable-name",
      "sizeGB": 0.0,
      "isDefault": false,
      "capabilities": ["regular|reasoning"]
    }
  ]
}
```

### Fields Description
- **version**: API schema version for compatibility tracking
- **lastUpdated**: ISO 8601 timestamp of last model list update
- **models**: Array of available AI models
  - **id**: Unique model identifier (usually HuggingFace/MLX path)
  - **displayName**: User-friendly model name for UI display
  - **sizeGB**: Model size in gigabytes for download/storage estimation
  - **isDefault**: Indicates the recommended default model
  - **capabilities**: Array of model capabilities ("regular" or "reasoning")

## Usage in iOS Apps

### Swift Integration Example

```swift
struct ModelInfo: Codable {
    let id: String
    let displayName: String
    let sizeGB: Double
    let isDefault: Bool
    let capabilities: [String]
}

struct ModelCatalog: Codable {
    let version: String
    let lastUpdated: String
    let models: [ModelInfo]
}

// Load and parse the model catalog
guard let url = URL(string: "https://raw.githubusercontent.com/aidynamicsolutions/aimodellist/main/models.json"),
      let data = try? Data(contentsOf: url) else {
    fatalError("Failed to load model catalog")
}

let catalog = try JSONDecoder().decode(ModelCatalog.self, from: data)

// Find default model or filter by capability
let defaultModel = catalog.models.first { $0.isDefault }
let reasoningModels = catalog.models.filter { $0.capabilities.contains("reasoning") }
```

## Integration Guidelines

1. **Cache Locally**: Download and cache the JSON file to reduce network requests
2. **Version Check**: Compare the `version` field to handle API changes gracefully
3. **Size Awareness**: Use `sizeGB` to inform users about download requirements
4. **Capability Filtering**: Filter models based on your app's specific needs
5. **Default Fallback**: Use the `isDefault` flag when no specific model is required

## File Structure

```
aimodellist/
├── README.md           # This documentation
└── models.json         # Model catalog data
```

## Contributing

To add new models or update existing entries:

1. Modify `models.json` with the new model information
2. Update the `lastUpdated` timestamp to current UTC time
3. Increment the `version` if making schema changes
4. Ensure all required fields are populated
5. Test the JSON structure with a validator

## License

This repository provides model metadata only. Individual models are subject to their respective licenses from the MLX community and original creators.

## API Endpoint

When hosted, the model catalog will be available at:
```
https://raw.githubusercontent.com/aidynamicsolutions/aimodellist/main/models.json
```

Use this URL in your applications to dynamically fetch the latest model information.