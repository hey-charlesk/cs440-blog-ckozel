---
layout: post
title: "Distrain Dis Data"
date: 2026-10-29
paper_title: "DistTrain: Addressing Model and Data Heterogeneity with Disaggregated Training for Multimodal Large Language Models"
paper_authors: "Z. Zhang, Y. Zhong, Y. Jiang, H. Hu, J. Sun, Z. Ge, Y. Zhu, D. Jiang, X. Jin (Peking University / StepFun)"
paper_venue: "SIGCOMM 2025"
paper_url: "https://doi.org/10.1145/3718958.3750472"
week: 6
tags: [ai, training, data-center, disaggregation]
---

## Key Idea

Multimodal LLMs are really just three models stacked together in a trenchcoat. An image to text encoder, a backing
LLM, and a modal output generator. Today these are commonly trained together using a tool such as Megatron-LM.
However it treats the three models the same during training, ignoring heterogeneity in training resulting in efficiency.
The encoder/generator are relatively small compared to a 70B-parameter LLM leaving idle GPU time while training all
three models together. Also the modality of the training data differs per item, with some having many images while
others have one or none. In testing this 'monolithic' architecture their Model FLOPs Utilization (MFU) as low as 20%.

DistTrain addressed this by 'disaggregating' both the model-training and data-preprocessing. Each of the three modules 
becomes its own "parallelism unit" with its own GPU count and data/tensor parallelism levels. A communication broker
bridges training pipeline traffic between the mismatched modules. Data preprocessing is moved from the GPU to CPU, 
which sees the whole batch of data. Then strategically organizes the data across groups so no one gets overwhelmed 
with all the large samples while others idle. This resulted in ~ 52% MFU on a 72B multimodal model being 1.3x faster, 
and over 2x faster on smaller models.


## Critique

This does not seem very network focused, more LLM training specific. However the control architectures load balancing
and parallelization of heterogeneous timed computations is similar to big-head/long-tail issues in networks. The authors
only tested this with image models, which are the most common multimodal models, but for a system designed to assist
any kind of multimodal model, testing it with others would prove its broader usefulness. 

## Connections

Like week 5's [RDMA Over Lossy Fabric](/cs440-blog-ckozel/2026/10/27/rdma-over-lossy-fabric/) this is optimizing AI
training workloads. However its not doing so on the network infrastructure level, but on the training scheduler level.

[Raha](/cs440-blog-ckozel/2026/10/06/Raha-Fault-Tolerance/) also used the trick of breaking the problem down into convex 
components that can be solved easily.
