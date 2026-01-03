# MixLoRA: Parameter-Efficient Style Mixing in Diffusion Models

This repository contains an implementation of **MixLoRA**, a technique for composing multiple LoRA adapters in diffusion models to enable **controllable style mixing at inference time**.

The project demonstrates how independently trained LoRA adapters (e.g., Monet, Van Gogh, Ghibli) can be linearly combined within **Stable Diffusion** to generate hybrid artistic styles **without retraining the base model**.

---

## Project Motivation

Fine-tuning large diffusion models for every new style is computationally expensive.  
LoRA enables parameter-efficient fine-tuning, but standard LoRA adapters are limited to a **single style or task**.

**MixLoRA addresses this limitation by enabling:**
- Reusable LoRA adapters  
- Non-destructive style composition  
- Smooth interpolation between multiple styles using weights  

This project implements the MixLoRA idea end-to-end for image generation.

---

## Key Concepts

- Stable Diffusion as the frozen base model  
- LoRA adapters trained independently per style  
- MixLoRA inference via weighted summation of LoRA updates  
- No retraining required for new style combinations  

---

## High-Level Pipeline

1. Load a pretrained Stable Diffusion model  
2. Inject LoRA layers into attention modules  
3. Train one LoRA adapter per artistic style  
4. Save adapters independently  
5. Load multiple adapters during inference  
6. Apply weighted composition (MixLoRA)  
7. Generate mixed-style images  

```mermaid
flowchart LR
    A[Text Prompt] --> B[Text Encoder Frozen]
    B --> C[Stable Diffusion U-Net Frozen]

    subgraph Adapters[LoRA Adapters]
        L1[Monet Adapter]
        L2[Van Gogh Adapter]
        L3[Ghibli Adapter]
    end

    L1 --> M[MixLoRA Mixing]
    L2 --> M
    L3 --> M

    M --> C
    C --> E[Generated Image]
