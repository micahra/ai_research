
# Definitions

AI output of definitions of various concepts used throughout this documentation project



# File Formats

## SafeTensors

**Safetensors** is a modern, open-source file format developed by Hugging Face for **storing and loading machine learning tensors** safely and efficiently. In the context of Large Language Models (LLMs), which rely on billions of parameters (weights) saved across massive files, safetensors has largely replaced older formats like PyTorch's default `.bin` or `.pt` (which use Python's `pickle` utility).

Here is a breakdown of why safetensors is crucial for LLMs, grouped by its main advantages.

### 1. Security (The "Safe" in Safetensors)
* **No Arbitrary Code Execution:** The biggest flaw of older `.bin` (pickle) files is that they can contain hidden malicious code. When you load a pickle file, that code automatically runs on your computer. 
* **Pure Data Storage:** Safetensors files strictly contain raw tensor data and a basic JSON header describing the data. It is impossible to hide executable code inside them, making it safe to download LLM weights from public repositories like Hugging Face.

### 2. Speed and Efficiency
* **Zero-Copy Loading:** Safetensors maps the file directly from your storage drive into your computer's memory (RAM/VRAM) using a process called `mmap`. The data doesn't need to be copied or duplicated during the load process, which drastically speeds up model startup times.
* **Asynchronous Loading:** It allows a model to start loading new layers into memory while the GPU is still processing previous layers, optimizing the hardware pipeline.

### 3. Better Memory Management
* **Lazy Loading:** With safetensors, an application can open a 100GB LLM file and only load the specific layers or tensors it needs at that exact moment, rather than loading the entire massive file into RAM all at once.
* **Shared Memory:** If you run multiple instances of the same LLM on the same machine, they can share the exact same memory space for the model weights, saving massive amounts of RAM.

---

### Directly Comparing Safetensors vs. PyTorch Pickle (.bin / .pt)

| Feature | Safetensors (`.safetensors`) | PyTorch Pickle (`.bin` / `.pt`) |
| :--- | :--- | :--- |
| **Security** | **Completely Safe** (Data only) | **Insecure** (Can execute arbitrary code) |
| **Loading Speed** | **Extremely Fast** (Zero-copy / `mmap`) | **Slower** (Requires copying data in memory) |
| **RAM Footprint** | **Minimal** (Supports lazy loading) | **High** (Usually copies entire file into RAM first) |
| **Format Structure** | JSON Header + Raw Bytes | Serialized Python Object Tree |


# Tools

## Parsec

**Parsec for Windows** is a proprietary, **high-performance remote desktop application** and desktop-capturing software primarily optimized for **ultra-low latency video streaming, gaming, and real-time creative work**. 

Unlike traditional remote desktop tools (like RDP or VNC) which can be laggy, Parsec is built from the ground up using specialized hardware-accelerated video encoding and decoding techniques. This allows a user on a client device to connect to a host Windows PC over the internet and interact with it as if they were sitting directly in front of it.

---

### Key Capabilities on Windows
Windows serves as the premier platform for Parsec, offering the complete **Host Feature Matrix**:

* **Host and Client Functionality:** Windows PCs can act as both the *Host* (the powerful machine sharing its screen) and the *Client* (the machine viewing and controlling the host).
* **Ultra-High Performance:** Supports streaming at up to **4K resolution at 60 FPS** (and up to 240 FPS max) with near-zero latency (~7ms on local networks).
* **Gaming Optimization:** Features "Immersive Mode," which maps inputs from keyboards, mice, and **gamepad controllers** directly to the host PC. It enables local co-op multiplayer over the internet; only the host needs to own the game.
* **Advanced Visual Accuracy:** Supports **4:4:4 color mode** and H.265 streaming, ensuring high-fidelity color accuracy required by video editors, 3D artists, and animators.
* **Per-User vs. Per-Computer Installation:** Windows installation files ([setup.exe](https://parsec.app)) allow a standard local setup or a system-wide setup that allows connection directly from the **Windows logon screen**.

### How It Works Behind the Scenes
1. **Capture:** Parsec leverages the native Windows **Desktop Duplication API** to capture raw frames straight from the graphics card framebuffer with minimal overhead.
2. **Encode:** It compresses these frames into a highly efficient **H.264 or H.265** video stream using hardware encoding.
3. **Transmit:** The data is pushed through Parsec's proprietary **BUD networking protocol**, establishing an encrypted, peer-to-peer connection directly between the host and client.
