# 🔬 Holo-RAG: Physics-Informed Neural Operator for Phase Retrieval

![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-FF6F00?style=for-the-badge&logo=tensorflow)
![Google Cloud](https://img.shields.io/badge/Google_Cloud-Vertex_AI-4285F4?style=for-the-badge&logo=google-cloud)
![Python](https://img.shields.io/badge/Python-3.11+-3776AB?style=for-the-badge&logo=python)
![Status](https://img.shields.io/badge/Status-Research_Prototype-success?style=for-the-badge)

> **CSCI-89 Capstone Project (Harvard Extension School)** > **Student:** Utah Hans  
> **Advisor:** Dr. Zoran B. Djordjevic

---

## 📖 Executive Summary
**Holo-RAG** is a cloud-native deep learning pipeline that bridges the gap between classical optics and modern Generative AI. It solves the "Phase Retrieval Problem"—one of the hardest inverse problems in physics—by training a **Physics-Informed U-Net** to "see" invisible structures hidden in light waves.

Ideally suited for industrial maintenance, the system not only reconstructs 3D objects from 2D holographic noise but also uses the reconstruction to query technical manuals (RAG), acting as a "Shazam for Mechanics."

---

## 🧐 The "Why": The Phase Problem (ELI5)
Imagine you are trying to figure out what a rock looks like by only staring at the ripples it made in a pond **after** the rock has sunk.

* **The Ripples (Intensity):** This is what a camera sees. It's just a messy pattern of waves.
* **The Rock (Phase):** This is the object's shape and depth. Standard cameras **delete** this information because they are "blind" to the timing of light waves.

**The Solution:**
We built a "Neural Lens." We trained an AI on a **Virtual Optical Bench** (a physics simulator) to look at the messy ripples and hallucinate the rock back into existence with >92% structural accuracy.

---

## ⚙️ Architecture & Methodology

The pipeline runs exclusively on **Google Cloud Platform (Vertex AI)** and consists of three distinct modules:

### 1. The Virtual Optical Laboratory (Physics Engine)
Before we train the AI, we must teach it physics. We built a differentiable simulator that models **Fraunhofer Diffraction**:
* **Input:** Clean 2D image (Spatial Domain).
* **Process:** Fast Fourier Transform (FFT) $\rightarrow$ Shift $\rightarrow$ Absolute Magnitude $\rightarrow$ Poisson/Thermal Noise Injection.
* **Output:** A noisy, unrecognizable "Hologram" (Frequency Domain).

### 2. The Neural Lens (Modified U-Net)
We use a **ResNet-backed U-Net** to reverse the physics.
* **Input:** The noisy hologram (Frequency Domain).
* **Transformation:** The network learns the inverse Fourier mapping non-linearly.
* **Skip Connections:** Essential for holography, allowing high-frequency fringe data to bypass the bottleneck and preserve sub-millimeter details.
* **Loss Function:** We reject standard MSE (Mean Squared Error) because it creates blurry averages. We use **SSIM (Structural Similarity)** Loss to force the network to learn topological edges and shapes.

### 3. Multimodal RAG (Retrieval-Augmented Generation)
Once the image is reconstructed:
1.  **Embedding:** The system calculates a vector fingerprint of the reconstructed part.
2.  **Retrieval:** It queries a Vector Database (Mock) to find the exact maintenance manual for that specific part.

---

## 📊 Key Results

| Metric | Value | Note |
| :--- | :--- | :--- |
| **Training Time** | <50 Epochs | Converges rapidly on GCP T4 GPU |
| **Reconstruction** | **SSIM > 0.92** | High-fidelity structural recovery |
| **Differentiation** | 100% | Successfully distinguishes geometrically similar items (e.g., Dress vs. Trousers) |

> **Note:**
> *"The critical innovation here is the **Zero-Shot** capability. Because the model is trained on the fundamental physics of light (diffraction), it can theoretically reconstruct objects it has never seen before, provided they obey the same optical laws."*

---

## 💻 Installation & Setup

This project is optimized for **Google Cloud Platform (Vertex AI)** using a **T4 GPU**, but it is fully compatible with local machines running NVIDIA GPUs.

### 1. Prerequisites
* **Python:** 3.11+
* **Accelerator:** NVIDIA T4, V100, or A100 (Recommended for training)
* **Environment:** Conda or Virtualenv

### 2. Environment Setup (The "Zero-Garbage" Way)
We strictly follow the course's environment isolation protocols to ensure no dependency conflicts.

```bash
# 1. Clone the Repository
git clone [https://github.com/YourUsername/holo-rag-capstone.git](https://github.com/YourUsername/holo-rag-capstone.git)
cd holo-rag-capstone

# 2. Create a clean Conda Environment (prevents "Dependency Hell")
conda create -n holorag python=3.11
conda activate holorag

# 3. Install Production Dependencies
# We use strict version pinning for reproducibility
pip install tensorflow[and-cuda]>=2.14 matplotlib numpy jupyterlab


## 💻 Installation & Environment Setup

This project is built for **Google Cloud Platform (Vertex AI)** but supports local execution with NVIDIA GPUs.

### 1. Prerequisites
* **Platform:** GCP Vertex AI (Workbench) or Google Colab Pro
* **Accelerator:** NVIDIA T4 GPU (Minimum) or A100 (Recommended)
* **Python:** 3.10+
* **Framework:** TensorFlow 2.14+

### 2. Quick Start (The "No-Headache" Way)
We strictly follow the course's environment isolation protocols.

```bash
# 1. Clone the Repository
git clone [https://github.com/YourUsername/holo-rag-capstone.git](https://github.com/YourUsername/holo-rag-capstone.git)
cd holo-rag-capstone

# 2. Create Isolated Conda Environment
conda create -n holorag python=3.10
conda activate holorag

# 3. Install Dependencies
# Uses strict version pinning for reproducibility
pip install -r requirements.txt

🚀 Usage Guide

Option A: Cloud Training (Vertex AI / Colab) [Recommended]
To reproduce the SSIM > 0.92 results cited in the paper:Upload HoloRAG_Final_Capstone.ipynb to your GCP Vertex AI Workbench.Select the TensorFlow Enterprise 2.11 (GPU) kernel.
Step 1: Run the "Virtual Optical Bench" cells to generate synthetic holograms.
Step 2: Run "Model Training" (Approx. 15 mins on T4 GPU).
Step 3: Execute the "Blind Reconstruction" cell to visualize results.
Option B: Local InferenceTo run the pre-trained model on your local machine:Bash# Launch Jupyter
jupyter lab HoloRAG_Final_Capstone.ipynb

Note: The simulate_fraunhofer_diffraction function requires ~4GB of VRAM. If you encounter OOM (Out of Memory) errors, reduce BATCH_SIZE to 16 in the configuration cell.

📂 Repository StructurePlaintextholo-rag-capstone/
├── 📄 README.md              # Project Documentation & Manifest
├── 📓 HoloRAG_Final.ipynb    # Main Executable (End-to-End Pipeline)
├── 📄 requirements.txt       # Frozen dependency list
├── 📂 paper/
│   └── 📄 Holo-RAG.pdf       # Final Capstone Report (PDF)
└── 📂 assets/
    ├── 📊 training_curves.png # SSIM vs Epochs
    └── 🖼️ model_arch.png     # U-Net Architecture Diagram

