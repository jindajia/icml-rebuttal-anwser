**Table1: TFLOPs comparison between SDP4Bit and DUO4Bit across different model scales on 32 A100 GPUs.**  
*The sequence length is uniformly set to 4096, except for the 18B model, which uses a sequence length of 2048 due to memory constraints.*

| **Model Size** | **SDP4Bit** | **DUO4Bit** |
|----------------|-------------|-------------|
| 1.3B           | 118.000     | 117.350     |
| 2.7B           | 126.075     | 125.475     |
| 6.7B           | 119.350     | 118.450     |
| 13B            | 133.325     | 128.425     |
| 18B            | 111.300     | 105.800     |
