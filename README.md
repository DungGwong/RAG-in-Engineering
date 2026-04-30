# RAG-in-Engineering
This repository contains the implementation of a Cross-Lingual Multi-Agent Retrieval-Augmented Generation (RAG) framework designed for solving complex calculation problems in electrical engineering. The project addresses the specific challenges LLMs face with domain-specific formulas, such as hallucinations and logical drift in multi-step reasoning.

Project Overview

Large Language Models often struggle with engineering tasks because they fail to accurately recall physical formulas or maintain logical consistency across derivations. This project introduces a "Plan-Execute-Review" architecture that mimics human problem-solving to ensure reliability.

The system decomposes complex problems into sub-tasks, performs targeted retrieval for each step using a dual-stage search (Embedding + Re-ranking), and employs a Critic agent for self-verification.

Key Results

The framework was evaluated on a dataset of 300 electrical engineering problems. The full configuration (Standard) significantly outperformed all baselines:

Configuration

	

Recall (%)

	

Precision (%)

	

F1 Score




Monolingual Chinese (Chi)

	

60.87

	

55.27

	

0.5567




No Agent Loop (noAgent)

	

59.00

	

47.49

	

0.5019




No GPT Re-rank (noGPT)

	

55.07

	

46.74

	

0.4735




Full Framework (Standard)​

	

72.96​

	

60.49​

	

0.6356​

Component Analysis

Semantic Re-ranking:​ The most critical component, improving the F1 score by 0.1621​ over vector-only retrieval. It effectively filters formulas that look similar but have different applicability conditions.

Agent Loop:​ Provided a 0.1337​ F1 improvement through error recovery and self-correction.

Cross-Lingual Processing:​ Using English as an intermediate reasoning language yielded a 0.0789​ F1 boost compared to the monolingual Chinese setting.

Repository Structure

The repository is organized into datasets and experimental notebooks used for ablation studies.

Datasets

EE_Question/: Contains the source data for the engineering problems.

jsons_EE_Chi/: Structured JSON data for Chinese-language engineering problems.

jsons_EE_Eng/: Structured JSON data for English-language engineering problems (used for intermediate reasoning).

Experimental Notebooks (Ablation Studies)

These four notebooks correspond to the experimental configurations described in the paper. They are used to reproduce the results in the ablation study table above.

Multi_Search_Eng_Standard.ipynb

Description:​ The complete framework. Includes Chinese-to-English translation, problem decomposition, GPT-4o based semantic re-ranking, and the multi-agent self-correction loop.

Multi_Search_Eng_noAgent.ipynb

Description:​ Ablation study removing the agent loop. This runs the pipeline only once (Plan →Retrieve →Solve →Output) without self-verification or re-retrieval.

Multi_Search_Eng_noGPT.ipynb

Description:​ Ablation study removing the LLM-based re-ranker. After retrieving the top-3 candidates by embedding similarity, it directly selects the top-1 result without semantic refinement.

Multi_Search_Chi.ipynb

Description:​ Comparison baseline running entirely in Chinese. Uses DeepSeek-V3 to ensure a fair comparison against the English-centric configurations.

Methodology Summary

Input Processing:​ Translates the Chinese query into English to leverage superior LLM reasoning capabilities.

Planning:​ Decomposes the problem into a sequence of dependent sub-problems using Chain-of-Thought (CoT).

Retrieval:​ Performs granular, task-oriented search using a structured formula knowledge base (555 entries).

Solving:​ Executes calculations constrained strictly by the retrieved context.

Critic:​ Reviews the solution for unit consistency and logical errors, triggering a self-correction loop if necessary.
