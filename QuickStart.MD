# Example & Quick Start 
This section provides step-by-step instructions to quickly run rAgent. Whether in a simple console-based interaction or a web-based UI using Chainlit, these examples will help you deploy and test your agent effortlessly.

## Installation
Before running **rAgent**, it’s highly recommended to set up a virtual environment to isolate dependencies.

### Method 1: Using python-env

**Step 1: Create and Activate a Virtual Environment**
Run the following commands in your terminal
```console
# Create a virtual environment
python -m venv venv

# Activate the virtual environment (MacOS/Linux)
source venv/bin/activate
# Activate the virtual environment (Windows)
venv\Scripts\activate
```
**Step 2: Clone the repository**
```Console
git clone https://github.com/Rivalz-ai/Ragent-python.git

cd Ragent-python
```
**Step 3: Install Dependencies**

Direct to folder cookbook, then install requirement
```console
pip install -r requirement.txt
```
Now you’re ready to run the agent!

### Method 2: Using conda
**Step 1: Create conda environment**
Run the following commands in your terminal

```console
conda create -n ragent python=3.11

conda activate ragent
```
**Step 2: Clone the repository**
```Console
git clone https://github.com/Rivalz-ai/Ragent-python.git

cd Ragent-python
```
**Step 3: Install Dependencies**

Direct to folder cookbook, then install requirement
```console
pip install -r requirement.txt
```
Now you’re ready to run the agent!

### Method 3: Using uv (Recommended)
**Step 1: Install uv (A fast Python package installer and resolver)**
```console
curl -LsSf https://astral.sh/uv/install.sh | sh
```
**Step 2: Clone the repository**
```Console
git clone https://github.com/Rivalz-ai/Ragent-python.git

cd Ragent-python
```
**Step 3: Create a new virtual environment and activate it**
```console
uv venv
source .venv/bin/activate  # On Unix/macOS
# Or on Windows:
# .venv\Scripts\activate
```
**Step 4: Install dependencies**
```console
uv pip install -r requirements.txt
```
## Configuration
**1. Create your .env file (you can copy from the example)**
```console
cp .env-example .env
```
**2. Edit .env file to add your API keys and customize settings**

**2.1 You can only setup openAI key**
```console
OPENAI_API_KEY="your-api-key"
```

**2.2 If you using another LLM model. If you use other models based on Openai api, please only setup these things**

```console
# For other API KEYS
custom_base_url="url-custom"
deep_infra_api_key="api-key"
deep_infra_model="model-name"
```

## Run code
### Method 1: Very Quick
One line for run OpenManus:
```console
chainlit run run_chainlit.py
```
### Method 2: Modify Sample by create your Agent

#### Run RX agent with chainlit example
Create a Python script (run_rx_chainlit.py) with the following code:
```python
import uuid
import chainlit as cl
import os
from dotenv import load_dotenv

# Import RXAgent
from r_agent.rx_agent import RXAgent, RXAgentOptions
from multi_agent_orchestrator.types import ConversationMessage

# Load environment variables
load_dotenv()

# Create RXAgent
def create_X_agent():
    options = RXAgentOptions(
        name="X Agent",
        description="Handles Twitter/X interactions such as posting tweets.",
        api_key=os.getenv("API_KEY"),
        model="gpt-4o",
        base_url="https://api.openai.com",
        xaccesstoken=os.getenv("TWITTER_ACCESS_TOKEN"),
        inference_config={
            "maxTokens": 500,
            "temperature": 0.5,
            "topP": 0.8,
            "stopSequences": []
        },
        tool_config={
            "tool": "Xtools",
            "toolMaxRecursions": 5
        }
    )
    return RXAgent(options)

agent = create_X_agent()

@cl.on_chat_start
async def start():
    cl.user_session.set("user_id", str(uuid.uuid4()))
    cl.user_session.set("session_id", str(uuid.uuid4()))

@cl.on_message
async def handle_message(message: cl.Message):
    user_id = cl.user_session.get("user_id")
    session_id = cl.user_session.get("session_id")

    msg = cl.Message(content="")
    await msg.send()
    cl.user_session.set("current_msg", msg)

    response = await agent.process_request(
        input_text=message.content,
        user_id=user_id,
        session_id=session_id,
        chat_history=[]
    )

    if isinstance(response, ConversationMessage):
        await msg.stream_token(response.content[0]["text"])

    await msg.update()

# Run Chainlit server
if __name__ == "__main__":
    cl.run()
```
***Run the Chainlit Server***
```console

chainlit run run_chainlit.py
```
A web interface will open, allowing you to chat with RXAgent in real-time.