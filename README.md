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