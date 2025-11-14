# 📝 Instructions to download small model

1. Run in terminal.
    ```bash
    docker build -f optional_small_model_dockerfile -t ollama-gemma-img .
    ```

2.  Run in terminal.
    ```bash
    docker run -d --name ollama-gemma-offline -p 127.0.0.1:11434:11434 ollama-gemma-img
    ```

3. Change Model Name from "mistral:7b" to "gemma3:1b-it-qat" in [chat_interface](chat_interface.ipynb).