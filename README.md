# Scalability Experiments
**Table1: TFLOPs comparison between SDP4Bit and DUO4Bit across different model scales on 32 A100 GPUs.** 

DUO consistently achieves comparable end-to-end training speed to SDP4Bit, demonstrating minimal overhead. The 6.7B model enables tensor parallelism, and DUO shows no performance impact. The slight drop in TFLOPs for the 13B and 18B models is attributed to DUO’s communication occasionally delaying pipeline communication, as both involve cross-node data transfer over PCIe.
| **Model Size** | **SDP4Bit** (TFLOPS) | **DUO4Bit** (TFLOPS) |
|:----------------:|:-------------:|:-------------:|
| 1.3B           | 118.000     | 117.350     |
| 2.7B           | 126.075     | 125.475     |
| 6.7B           | 119.350     | 118.450     |
| 13B            | 133.325     | 128.425     |
| 18B            | 111.300     | 105.800     |

**Table2: Model training configuration for scalability experiments.**

*The sequence length is uniformly set to 4096, except for the 18B model, which uses a sequence length of 2048 due to memory constraints.*
| **Model Size** | **Tensor Parallel Size** | **Pipeline Parallel Size** | **Data Parallel Size** | **Grad Accum Step** | **Microbatch Size** | **Sequence Length** |
|:--------------:|:------------------------:|:--------------------------:|:----------------------:|:-------------------:|:-------------------:|:-------------------:|
| 1.3B           | 4                        | 1                          | 8                      | 32                  | 4                   | 4096                |
| 2.7B           | 4                        | 1                          | 8                      | 32                  | 4                   | 4096                |
| 6.7B           | 4                        | 1                          | 8                      | 32                  | 1                   | 4096                |
| 13B            | 4                        | 2                          | 4                      | 32                  | 1                   | 4096                |
| 18B            | 4                        | 2                          | 4                      | 32                  | 1                   | 2048                |

