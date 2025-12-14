# CSV Concat Agent - Extra Credit Assignment

## Overview

This extra credit assignment focuses on **understanding how AI agents interact with functions**. Students will create a simple CSV concatenation function and experiment with getting an agent to actually call it.

**The Challenge:** Create TWO versions of the same function:
1. **Parameterized version** - Takes folder_path and output_file as parameters
2. **Hardcoded version** - Has folder paths hardcoded inside the function

Then experiment with different questions, system prompts, and docstrings to see which version the agent can successfully call.

## Learning Objectives

Students will learn to:
1. Understand how agents interpret function signatures and docstrings
2. Experiment with natural language prompts to trigger function calls
3. Observe when agents execute functions vs just describe them
4. Learn the importance of clear docstrings and system prompts
5. Document their observations and findings

## Prerequisites

### Required Knowledge
- Python basics (functions, dictionaries, lists)
- Experience with the RAG project (familiarity with APIs and LLMs)
- Basic understanding of API requests

### Required Software
- Python 3.13
- Jupyter Notebook
- Ollama with a model (llama3.2:1b or llama3.2:3b)
- Docker (for containerized setup)

### Required Python Packages

```bash
pip install llama-index
pip install llama-index-llms-ollama
pip install pandas
```

#### Conda Installation

```bash
conda install llama-index
conda install llama-index-llms-ollama
conda install pandas
```

## Setup Instructions

### Option 1: Docker Setup (Recommended)

**Step 1: Update the Dockerfile**

Edit `optional_small_model_dockerfile` and save:

```dockerfile
# Change the base image from:
ENV OLLAMA_MODEL=gemma3:1b-it-qat
# To:
ENV OLLAMA_MODEL=llama3.2:1b
```

After dockerfile edit, create img and container:

```bash
# 🗨️ Remember to be in the same folder of your dockerfile from the terminal
docker build -f optional_small_model_dockerfile -t ollama-llama3_1b-img .
```

```bash
# 🗨️ Remember to include the ip and reference the same img name you just created.
docker run -d --name ollama-llama3_1b-offline -p 127.0.0.1:11434:11434 ollama-llama3_1b-img
```

## The Concat Function

### What the Function Does

The concat function performs the following operations:
1. Reads all CSV files from a folder
2. Combines them into a single DataFrame
3. Saves the combined data to an output file

### Two Versions to Create

**Version 1: Parameterized (Harder for Agent)**
```python
def concat_csv_files(folder_path: str, output_file: str) -> Dict:
    # Takes parameters - agent needs to extract these from the question!
```

**Version 2: Hardcoded (Easier for Agent)**
```python
def concat_csv_files_simple() -> Dict:
    # Hardcoded paths - agent just needs to call it!
    folder_path = "./hogwarts_data"
    output_file = "./merged_data_simple.csv"
```

## Files Provided

### For Students
- **`extra_credit_agent_student.ipynb`** - Student notebook with TODO sections

### What Makes a Good Summary

Your one-page summary should answer:
1. **What happened?** Did the agent call the parameterized function? The hardcoded one?
2. **What worked?** Which questions, prompts, or docstrings got the best results?
3. **What didn't work?** What did you try that failed?
4. **Why do you think this happened?** What's your theory about agent behavior?
5. **Best practices:** Based on your experiments, what would you recommend?

## Recommended Models

This assignment works with small models:

### llama3.2:1b (Fastest) (Recommended)
- ✅ Runs on 4GB RAM systems
- ✅ Fast responses
- ✅ Good for testing and development
- ⚠️ May struggle with complex multi-step reasoning

### llama3.2:3b
- ✅ Handles function calling well
- ✅ Runs on 8GB RAM systems
- ✅ Only requires ~4GB memory
- ✅ Better reasoning capabilities
- ⚠️ May not work/load if hardware is insufficient

## Expected Deliverables

**Submit One file only:**

1. **Completed Jupyter Notebook** (`extra_credit_agent_student.ipynb`)
   - Both functions implemented and working
   - All experiments documented in markdown cells
   - Notes on what you tried and what happened
   - Clear section showing each experiment

**Do NOT submit:**
- Code files outside the notebook

## Common Issues and Solutions

### Issue: File Not Found
**Solution:** Make sure the CSV file path is correct and the file exists in the specified location

### Issue: LLM Not Responding
**Solution:**
- Ensure you have downloaded new model as an img
- Ensure you ran the container with correct commands
- Ensure the container is running

### Issue: Agent Not Calling Function
**Solution:**
- Ensure tool description is clear and mentions "CSV" and "column names"
- Check system prompt includes instructions to use the tool

### Issue: Import Errors
**Solution:**
```bash
pip install --upgrade llama-index llama-index-llms-ollama pandas
```

## Extension Ideas

Students can extend the project by:

1. **Additional CSV Processing Functions**
   - Remove duplicate rows
   - Handle missing values
   - Convert data types
   - Validate column data

2. **Enhanced Features**
   - Batch processing of multiple CSV files
   - Preview changes before saving
   - Backup original files
   - Generate data quality reports

3. **Alternative Applications**
   - Excel file processor agent
   - JSON to CSV converter agent
   - Data validation agent
   - File organization agent

## Resources

### Documentation
- [Pandas Documentation](https://pandas.pydata.org/docs/)
- [LlamaIndex Documentation](https://docs.llamaindex.ai/)
- [Ollama Documentation](https://ollama.ai/docs)
- [Python Regex Tutorial](https://docs.python.org/3/howto/regex.html)

### Example Questions to Try

**For Parameterized Function:**
1. "Combine all CSV files from the hogwarts_data folder into merged_data.csv"
2. "Merge the CSV files in ./hogwarts_data and save as ./merged_data.csv"
3. "Concatenate CSV files from hogwarts_data folder, output: merged_data.csv"
4. "Use concat_csv_files to merge hogwarts_data folder into merged_data.csv"
5. "I need to combine CSV files. Folder: ./hogwarts_data, Output: ./merged_data.csv"

**For Hardcoded Function:**
1. "Combine the CSV files"
2. "Merge the data"
3. "Run the concat function"
4. "Combine all the files"
5. "execute your tool to combine csvs."

**System Prompt Variations to Try:**
1. Basic: "You are a CSV assistant. Use tools when asked."
2. Explicit: "You must CALL the functions, not describe them."
3. Detailed: "Extract parameters from user questions and call functions with those parameters."

### Key Concepts to Emphasize

1. **Function Calling**: How LLMs can invoke functions based on descriptions
2. **Tool Abstraction**: Separating logic (function) from interface (tool)
3. **Agent Decision-Making**: How agents select appropriate tools
4. **API Design**: Well-designed functions make better tools

### Common Student Questions

**Q: Why use an agent instead of just calling the function directly?**
A: The agent can understand natural language requests like "clean my CSV" and automatically determine the correct parameters (file paths), making it more flexible and user-friendly.

**Q: How does the agent know when to use the tool?**
A: The LLM reads the tool description and matches it to the user's intent. Words like "CSV", "clean", "column names" trigger the tool.

**Q: Can I use a different LLM?**
A: Yes! You can use any LLM supported by LlamaIndex (OpenAI, Anthropic, etc.)

**Q: What if the CSV file doesn't exist?**
A: Add error handling in your function to check if the file exists before processing.

## Support

For questions or issues:
1. Check the solution notebook for reference
2. Review LlamaIndex and Pandas documentation
3. Test the function standalone before integrating with agent

## License

This assignment is for educational use.

---

**Version:** 1.0
**Last Updated:** December 2025
**Author:** Moy Patel
