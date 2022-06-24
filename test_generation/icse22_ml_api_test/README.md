# ICSE'22 Automated Testing of Software that Uses Machine Learning APIs

### Authors: Chengcheng Wan, Shicheng Liu, Sophie Xie, Yifan Liu, Henry Hoffmann, Michael Maire, Shan Lu
### Link: https://people.cs.uchicago.edu/~cwan/paper/ml_api_test.pdf

## Abstract
This study aims to solve the problem of testing machine learning APIs. It points out a few open challenges in testing ML-software. They proposed the tool Keeper which is designed to test ML software to tackle the problems. Keeper intergrates pseudp-inverse functions with symbolic execution to reach the sparse program-relevant input space. The design is elegant.

## Background
### ML Cloud APIs
- Three main categories of ML tasks: vision, language and speech.
- Input contains not only content (image/text/audio), but also configuration parameters.
- Output may include multiple records. A record: a key result + a auxiliary result field
### ML software
- ML APIs could be loosely or closely connected with the remaining part of the software. This paper focus on the later kind.

## Test Input Generation
- Two major components: test-input generation + test-output processing.
- Input generation: use DSE to solve path constraints.
### Identify relevant ML outputs
- Gather all API outputs as part of the path constraints.
- Resolve constraints for each branch sub-condition instead of the whole branch.
### Identify ML API inputs
Pseudo-inverse function f' of API f.
- Not analytical inversion of f, ideally independent.
- Semantic inverse of f.
- Produce multiple output for each input it takes in.
#### Search-based pseudo inversion
Including **vision** and **language** tasks.
- Great semantic inversion
- Not analytical inversion
- A large number of high-quality ranked test inputs
#### Synthesis-based pseudo inversion
Including **text-detection**, **entity-detection** and **speech-recognition** tasks
#### ML benchmarks for pseudo inversion
The last resort when there's no better solutions. For example, the sentiment detection API.
- The results are two floating-point numbers, score and magnitude.
- No search engine or synthesizer can generate text with positive/negative emotion.
### Putting everything together
1. Symbolic execution generates a set of inputs aiming to cover all branches.