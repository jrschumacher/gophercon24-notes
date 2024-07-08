# Gophercon 24 notes

## Gen AI 1

https://hackmd.io/SoeISE6cQRGO8S3qZdXUvQ

https://github.com/dwhitena/go-genai-workshop
https://github.com/dwhitena/go-genai-workshop-build

### History

2012-2017:

```mermaid
graph LR
  
  subgraph train
  A --> B
  AA --> B
  B --> C
  end
  
  subgraph inference
  AC --> C
  AB --> C
  C --> E
  E --> X
  end
  
  A[ex inputs]
  AA[known out]
  AB[model arch]
  AC[new model inputs]
  B[model train code]
  C[ideal params]
  E[model infer code]
  X[output]
```

Bert: 2017-2022

Fine-tuning / transfer learning

```mermaid
graph LR

  subgraph google training
  A-->B
  AA-->B
  AB-->B
  B-->C
  end

  subgraph my code
  C-->F

  FA-->F
  FB-->F

  F-->P
  end

  P-->I

  B[pre-training]
  C[initial params]

  F[fine tune code]
  FA[my ex in]
  FB[my ex out]

  P[params]

  I[infer]
```

The major change is the pre-training and then appending task specific code to the end of the pipeline. The task specific code is often called `embedding` or `feature representation`.

Training on meta tasks was intended to help with further model enhancements however as they trained on the autocomplete models on the vast data of the internet they discovered the emergent behavior of gen ai.

> [!NOTE]
> emergent just means unexpected in this context

2022+: Gen AI

```mermaid

graph LR

  subgraph others //compute heavy//
  A-->PT
  B-->PT
  C-->PT

  PT-->P1
  PT-->P2
  PT-->P3

  P1-->FT
  P2-->FT
  P3-->FT
  end

  subgraph me //func call//
  P-->I
  FT-->I

  I-->O
  end

  P[prompt]
  FT[fine tuning]
  I[infer]
  O[out]
```

LLM generates a list of next tokens with a probability and then passes it as the new prompt. One of the tokens might be the end of sentence token.


```mermaid
flowchart LR

  A[prompt] -- "A gopher is..." --> LLM

  LLM --> T1[1. a\n2. animal\n...\n30k. flea\n]

  T1 --> A
```

Temperature was added to shuffle the ranked words to give some variance on the output.

### Misc

OpenAI initiated the process of using SSEs (server sent events) with responses vs websockets. Returning SSE vs the finished response gives a better UX to users. Using websockets would work, but it would be moving against the trend.

System prompts `client.Roles.System` are used to give context to the LLM,

Context is injected with every prompt. There are ways to reduce this burden, but its not consistent. Remember the greatest burden is the generation.

When the long context window its hard to determine how to determine which aspect of the context caused the issue. If there is a large context window you can use an LLM to summarize and reduce the window size.

### Retrieval Augmented Generation

```mermaid
flowchart LR
  D-->C1
  D-->C2
  D-->CN
  D-->CX

  C1-->DV
  C2-->DV
  CN-->DV
  CX-->DV

  DV-->PT
  PT-->LLM
  LLM-->O

  D[Docs]
  C1[Chunk 1]
  C2[Chunk 2]
  CN[...]
  CX[Chunk N]
  DV[Vector DB]
  PT[Prompt Temp]
  O[Out]
```

DB Vector is a closeness via cosign-sim, l2 distance, etc

Multi-lingual models would theoretically share a similar vector space.

Advanced RAG

- hierarchical search finding most relevant chunk and related context
- elastic search then vector

If the context is 3 pages a summary may not be sufficient and the enrichment should be fine-tuned to the problem.

Bridge-tower model supports embedding images and text

Perf in enterprise:

- how long will it take to vectorize the docs
- how log will it take to search the docs

LLM is judge: use the LLM to rate if we answered the users question.

