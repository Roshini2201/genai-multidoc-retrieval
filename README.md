## Design and Implementation of a Multidocument Retrieval Agent Using LlamaIndex

### AIM:
To design and implement a multidocument retrieval agent using LlamaIndex to extract and synthesize information from multiple research articles, and to evaluate its performance by testing it with diverse queries, analyzing its ability to deliver concise, relevant, and accurate responses.

### PROBLEM STATEMENT:
Extracting specific, nuanced information from a collection of dense academic papers is a slow and inefficient manual process. Standard search tools rely on exact keywords and fail to understand the conceptual context of a user's question. This program aims to build an AI agent that can intelligently query multiple documents to synthesize precise answers to complex questions.

### DESIGN STEPS:

#### STEP 1:
Load PDF documents and create specialized search and summary tools for each paper.

#### STEP 2:
Initialize an AI agent with an OpenAI model, giving it access to all the created tools.

#### STEP 3:
Query the agent with a specific question about one paper to get a detailed answer from its content.

### PROGRAM:
```
from helper import get_openai_api_key
OPENAI_API_KEY = get_openai_api_key()
import nest_asyncio
nest_asyncio.apply()
```
```
urls = ["https://openreview.net/pdf?id=tc90LV0yRL",
        "https://openreview.net/pdf?id=DOMP5AgwQz",
        "https://openreview.net/pdf?id=3qFtGLGyO3"
    
]

papers = ["5074_Cybench_A_Framework_for_E.pdf",
          "74_CTIKG_LLM_Powered_Knowledge.pdf",
          "28_Artificial_Intelligence_Bas.pdf"
]
```
```
from utils import get_doc_tools
from pathlib import Path

paper_to_tools_dict = {}
for paper in papers:
    print(f"Gathering info from the paper: {paper}")
    vector_tool, summary_tool = get_doc_tools(paper, Path(paper).stem)
    paper_to_tools_dict[paper] = [vector_tool, summary_tool]
```
```
initial_tools = [t for paper in papers for t in paper_to_tools_dict[paper]]
from llama_index.llms.openai import OpenAI
llm = OpenAI(model="gpt-3.5-turbo")
len(initial_tools)
```
```
from llama_index.core.agent import FunctionCallingAgentWorker
from llama_index.core.agent import AgentRunner

agent_worker = FunctionCallingAgentWorker.from_tools(
    initial_tools, 
    llm=llm, 
    verbose=True
)
agent = AgentRunner(agent_worker)
```
```
response = agent.query(
    "Summarize 5074_Cybench_A_Framework_for_E.pdf"
)
```

### OUTPUT:
<img width="697" height="97" alt="image" src="https://github.com/user-attachments/assets/38744d60-2fb8-456b-905d-7626865a1e39" />
<img width="1242" height="445" alt="image" src="https://github.com/user-attachments/assets/1e9cf23b-a0a5-4456-9209-1093f57794e2" />


### RESULT:
Thus, Design and Implementation of a Multidocument Retrieval Agent Using LlamaIndex has been done successfully.
