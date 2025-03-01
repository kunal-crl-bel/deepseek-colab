# Running DeepSeek 7B on Google Colab

This guide will help you set up and run the DeepSeek 7B model on Google Colab using Ollama. It provides step-by-step instructions, system requirements, and troubleshooting tips to ensure a smooth experience.

## System Requirements

DeepSeek 7B requires a high-performance GPU with sufficient VRAM to run efficiently. Below is a table with recommended system configurations:

| Model          | VRAM Required | GPU Recommended | Speed (ms) per Prompt Token |
|---------------|--------------|----------------|--------------------------|
| DeepSeek 7B   | 16GB+        | A100/RTX 3090  | ~30-50ms                 |
| DeepSeek 6B   | 12GB+        | RTX 3080       | ~25-40ms                 |
| DeepSeek 3B   | 8GB+         | RTX 3060       | ~15-30ms                 |

> **Note:** Performance may vary based on available VRAM, batch size, and prompt complexity. Google Colab Pro+ offers A100 GPUs, which are suitable for running DeepSeek 7B.

## Prerequisites

Before running the DeepSeek 7B model, ensure you have:
- A Google account to use Google Colab.
- A Google Colab Pro or Pro+ subscription for access to high-end GPUs (optional but recommended).
- A basic understanding of Python and shell commands.

## Steps to Run DeepSeek 7B on Colab

### 1. **Enable GPU:**
   - Open Google Colab.
   - Go to `Runtime` > `Change runtime type`.
   - Set `Hardware accelerator` to `GPU`.
   - Click `Save`.

### 2. **Install Required Dependencies:**
   Run the following commands in a Colab code cell to install the necessary dependencies:
   ```sh
   !pip install libtmux
   !curl -fsSL https://ollama.ai/install.sh | sh
   ```

### 3. **Start Ollama Server in a Persistent tmux Session:**
   Use `libtmux` to keep `ollama serve` running in the background:
   ```python
   import libtmux

   server = libtmux.Server()
   session = server.new_session(session_name="ollama_session", kill_session=True)
   session.attached_pane.send_keys("ollama serve", enter=True)

   print("Ollama is now running persistently in a background session!")
   ```

### 4. **Download and Run DeepSeek 7B Model:**
   Fetch and run the DeepSeek 7B model using the following commands:
   ```sh
   ollama pull deepseek/7b
   ollama run deepseek/7b
   ```

## Troubleshooting

### **1. Google Colab Memory Errors**
- If you get an `Out of Memory` error, try reducing the model size by using `deepseek/6b` or `deepseek/3b`.
- Ensure you are using a high-end GPU by running:
  ```sh
  !nvidia-smi
  ```
  If the GPU is insufficient, consider switching to a Colab Pro+ plan.

### **2. Ollama Not Starting Properly**
- If `ollama serve` does not start, restart the Colab runtime and run the setup steps again.
- Check if Ollama is running with:
  ```sh
  ps aux | grep ollama
  ```
  If no process is found, rerun the `libtmux` script.

### **3. Slow Inference Speed**
- Reduce the prompt size and batch size to improve response time.
- Use a GPU with higher VRAM for better performance.

## Notes
- The `libtmux` library is used to keep `ollama serve` running persistently.
- Make sure your Colab runtime is not idle to prevent the session from disconnecting.
- You can use `screen` or `nohup` instead of `tmux` if needed.

Now, you're ready to run DeepSeek 7B on Google Colab! 🚀

