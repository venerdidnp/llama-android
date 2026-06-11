# 🤖 AI Chat — Local LLM Chatbot for Android

An **on-device AI chatbot** for Android, powered by [llama.cpp](https://github.com/ggerganov/llama.cpp). Run Large Language Models (LLMs) **completely offline** on your Android device — no internet, no cloud, no API key required.

> Minimum Android: **API 33 (Android 13)**  
> Supported ABIs: `arm64-v8a`, `x86_64`

---

## ✨ Features

- 💬 **Real-time streaming** — tokens appear as they are generated
- 🔒 **100% Offline** — no internet connection required after setup
- 📦 **Any GGUF model** — load any compatible GGUF model from your device storage
- ⚡ **Optimized for Arm** — uses KleidiAI kernels on arm64-v8a for best performance
- 🔄 **Context shifting** — handles long conversations without running out of memory
- 📋 **GGUF metadata viewer** — inspect model info before loading

---

## 📱 Screenshots

> *Load a GGUF model from your storage, then start chatting instantly.*

---

## 🏗 Architecture

```
llama.android/
├── app/          ← Android UI (Kotlin, Views)
│   └── MainActivity, MessageAdapter
└── lib/          ← JNI bridge + Kotlin API
    ├── cpp/
    │   ├── ai_chat.cpp   ← JNI layer wrapping llama.cpp
    │   └── CMakeLists.txt
    └── java/com/arm/aichat/
        ├── AiChat.kt            ← Public entry point
        ├── InferenceEngine.kt   ← Interface
        └── internal/            ← Implementation detail
```

---

## 🚀 Getting Started

### Requirements

| Tool | Version |
|------|---------|
| Android Studio | Hedgehog (2023.1.1) or newer |
| Android NDK | r29 (29.0.13113456) |
| CMake | 3.31.6 |
| JDK | 17 |
| Android device / emulator | API 33+ |

### 1. Clone the repo

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
cd YOUR_REPO_NAME

# Initialize llama.cpp submodule (required for build!)
git submodule update --init --recursive
```

### 2. Open in Android Studio

1. Open **Android Studio**
2. Click **File → Open** and select this folder
3. Wait for Gradle sync to complete
4. Android Studio will automatically detect the NDK

### 3. Download a GGUF model

You need a `.gguf` model file on your Android device. Recommended models for mobile:

| Model | Size | Link |
|-------|------|------|
| Qwen2.5 0.5B (Q4) | ~400 MB | [HuggingFace](https://huggingface.co/Qwen/Qwen2.5-0.5B-Instruct-GGUF) |
| Gemma 2 2B (Q4) | ~1.6 GB | [HuggingFace](https://huggingface.co/google/gemma-2-2b-it-GGUF) |
| Llama 3.2 1B (Q4) | ~700 MB | [HuggingFace](https://huggingface.co/meta-llama/Llama-3.2-1B-Instruct-GGUF) |
| Phi-3.5 Mini (Q4) | ~2.2 GB | [HuggingFace](https://huggingface.co/microsoft/Phi-3.5-mini-instruct-gguf) |

> ⚠️ Make sure your device has enough RAM. A 1B model needs ~1 GB free RAM minimum.

Transfer the `.gguf` file to your phone (via USB, Google Drive, etc.).

### 4. Build & Run

```bash
# Build debug APK
./gradlew assembleDebug

# Or build release APK
./gradlew assembleRelease
```

Or just hit **Run ▶️** in Android Studio.

### 5. Using the App

1. Tap the **FAB (floating action button)** to open file picker
2. Select your `.gguf` model file
3. Wait for the model to load (first time may copy the file to app storage)
4. Type your message and tap **Send**

---

## 🔧 Configuration

Key parameters in [`ai_chat.cpp`](lib/src/main/cpp/ai_chat.cpp):

| Constant | Default | Description |
|----------|---------|-------------|
| `DEFAULT_CONTEXT_SIZE` | `8192` | Max token context window |
| `BATCH_SIZE` | `512` | Token batch processing size |
| `DEFAULT_SAMPLER_TEMP` | `0.3` | Sampling temperature (lower = more focused) |
| `N_THREADS_MAX` | `4` | Max CPU threads for inference |

---

## 🛠 Building llama.cpp (submodule)

This project uses llama.cpp as a **git submodule**. The submodule is located at `llama.cpp/` in the project root.

If you modify llama.cpp or want to update to a newer version:

```bash
# Update submodule to latest commit
git submodule update --remote llama.cpp

# Or pin to a specific version
cd llama.cpp
git checkout b5263   # replace with desired tag/commit
cd ..
git add llama.cpp
git commit -m "chore: update llama.cpp to b5263"
```

---

## 🤝 Contributing

Pull requests are welcome! For major changes, please open an issue first.

---

## 📄 License

This project is licensed under the **MIT License**.  
The bundled [llama.cpp](https://github.com/ggerganov/llama.cpp) library is also MIT-licensed.
