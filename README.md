# Lab 2: Smart Image Analyzer with Durable Functions

A serverless application that automatically analyzes uploaded images using Azure Durable Functions with the Fan-Out/Fan-In pattern.

## Features

- **Blob Storage Trigger**: Automatically processes images when uploaded
- **Parallel Analysis**: Runs 4 analyses simultaneously (colors, objects, text, metadata)
- **Table Storage**: Stores analysis results for retrieval
- **HTTP API**: Query results via REST endpoint

## Prerequisites

- Python 3.11 or 3.12
- Azure Functions Core Tools v4.x
- Azurite (Azure Storage Emulator)
- VS Code with Azure Functions extension (optional)

## Local Setup

### 1. Install Dependencies

```powershell
# Create virtual environment
python -m venv .venv

# Activate virtual environment
.venv\Scripts\activate

# Install packages
python -m pip install -r requirements.txt
```

### 2. Start Azurite

**Option A: VS Code**
- Press `F1` → Type "Azurite: Start"

**Option B: Command Line**
```powershell
azurite --silent --location .azurite --debug .azurite/debug.log
```

### 3. Create Images Container

**Option A: VS Code Azure Extension**
- Click Azure icon → Workspace → Local Emulator → Blob Containers
- Right-click Blob Containers → Create Blob Container → Name: `images`

**Option B: Python Script**
```powershell
python create_container.py
```

### 4. Start Function App

```powershell
# Activate venv
.venv\Scripts\activate

# Start function app
func start
```

The function app will be available at `http://localhost:7071`

## Usage

### Upload an Image

```powershell
python upload_image.py "C:\path\to\your\image.jpg"
```

### View Results

- **All results**: `http://localhost:7071/api/results`
- **Specific result**: `http://localhost:7071/api/results/{id}`

## Project Structure

```
lab2/
├── function_app.py          # Main application (9 functions)
├── requirements.txt          # Python dependencies
├── local.settings.json       # Local configuration
├── host.json                 # Azure Functions host config
├── upload_image.py           # Helper script to upload images
└── create_container.py       # Helper script to create container
```

## Functions

1. `blob_trigger` - Detects image uploads, starts orchestrator
2. `image_analyzer_orchestrator` - Coordinates the workflow
3. `analyze_colors` - Extracts dominant colors
4. `analyze_objects` - Detects objects (mock)
5. `analyze_text` - Performs OCR (mock)
6. `analyze_metadata` - Extracts image metadata
7. `generate_report` - Combines all analyses
8. `store_results` - Saves to Table Storage
9. `get_results` - HTTP endpoint to retrieve results

## Architecture

The application uses the **Fan-Out/Fan-In** pattern:
- 4 analyses run in parallel
- Results are combined into a single report
- Report is stored in Azure Table Storage

## Testing

1. Upload an image using `upload_image.py`
2. Watch the function app logs for parallel execution
3. Query results via HTTP endpoint
4. Verify all 4 analyses completed successfully

## Notes

- Uses Azurite for local development (no Azure account needed)
- Mock implementations for object detection and OCR
- Real implementations for color analysis and metadata extraction

  Youtube :https://youtu.be/xKtQa0BdNJc
