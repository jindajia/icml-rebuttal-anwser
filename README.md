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

# Low Bandwidth Test on Cloud
## Computation v.s. Communication Overlapping performance

<table>
<thead>
<tr>
<th>Bandwidth</th>
<th>Sequence Len</th>
<th>Acc. Step</th>
<th>Method</th>
<th>E2E Throughput (Tflops)</th>
<th>Grad Comm / Iter (ms)</th>
<th>Computation / Iter (ms)</th>
<th>Full Overlap</th>
</tr>
</thead>
<tbody>
<tr><td rowspan="12">10 Gbps</td><td rowspan="6">4096</td><td rowspan="2">32</td><td>DUO4Bit</td><td>122.6</td><td>2135</td><td rowspan="2">6042</td><td>✅ Yes</td></tr>
<tr><td>SDP4Bit</td><td>123.0</td><td>N/A</td><td>❓ N/A</td></tr>
<tr><td rowspan="2">16</td><td>DUO4Bit</td><td>122.6</td><td>1959</td><td rowspan="2">3030</td><td>✅ Yes</td></tr>
<tr><td>SDP4Bit</td><td>123.1</td><td>N/A</td><td>❓ N/A</td></tr>
<tr><td rowspan="2">8</td><td>DUO4Bit</td><td>86.7</td><td>2029</td><td rowspan="2">1533</td><td>❌ No</td></tr>
<tr><td>SDP4Bit</td><td>117.5</td><td>N/A</td><td>❓ N/A</td></tr>
<tr><td rowspan="6">2048</td><td rowspan="2">32</td><td>DUO4Bit</td><td>83.5</td><td>2215</td><td rowspan="2">3024</td><td>✅ Yes</td></tr>
<tr><td>SDP4Bit</td><td>84.4</td><td>N/A</td><td>❓ N/A</td></tr>
<tr><td rowspan="2">16</td><td>DUO4Bit</td><td>71.2</td><td>2125</td><td rowspan="2">1545</td><td>❌ No</td></tr>
<tr><td>SDP4Bit</td><td>78.8</td><td>N/A</td><td>❓ N/A</td></tr>
<tr><td rowspan="2">8</td><td>DUO4Bit</td><td>39.0</td><td>2136</td><td rowspan="2">771</td><td>❌ No</td></tr>
<tr><td>SDP4Bit</td><td>71.1</td><td>N/A</td><td>❓ N/A</td></tr>
<tr><td rowspan="12">5 Gbps</td><td rowspan="6">4096</td><td rowspan="2">32</td><td>DUO4Bit</td><td>N/A</td><td>N/A</td><td rowspan="2">N/A</td><td>❓ N/A</td></tr>
<tr><td>SDP4Bit</td><td>N/A</td><td>N/A</td><td>❓ N/A</td></tr>
<tr><td rowspan="2">16</td><td>DUO4Bit</td><td>N/A</td><td>N/A</td><td rowspan="2">N/A</td><td>❓ N/A</td></tr>
<tr><td>SDP4Bit</td><td>N/A</td><td>N/A</td><td>❓ N/A</td></tr>
<tr><td rowspan="2">8</td><td>DUO4Bit</td><td>N/A</td><td>N/A</td><td rowspan="2">N/A</td><td>❓ N/A</td></tr>
<tr><td>SDP4Bit</td><td>N/A</td><td>N/A</td><td>❓ N/A</td></tr>
<tr><td rowspan="6">2048</td><td rowspan="2">32</td><td>DUO4Bit</td><td>N/A</td><td>N/A</td><td rowspan="2">N/A</td><td>❓ N/A</td></tr>
<tr><td>SDP4Bit</td><td>N/A</td><td>N/A</td><td>❓ N/A</td></tr>
<tr><td rowspan="2">16</td><td>DUO4Bit</td><td>N/A</td><td>N/A</td><td rowspan="2">N/A</td><td>❓ N/A</td></tr>
<tr><td>SDP4Bit</td><td>N/A</td><td>N/A</td><td>❓ N/A</td></tr>
<tr><td rowspan="2">8</td><td>DUO4Bit</td><td>N/A</td><td>N/A</td><td rowspan="2">N/A</td><td>❓ N/A</td></tr>
<tr><td>SDP4Bit</td><td>N/A</td><td>N/A</td><td>❓ N/A</td></tr>
<tr><td rowspan="12">2 Gbps</td><td rowspan="6">4096</td><td rowspan="2">32</td><td>DUO4Bit</td><td>N/A</td><td>N/A</td><td rowspan="2">N/A</td><td>❓ N/A</td></tr>
<tr><td>SDP4Bit</td><td>N/A</td><td>N/A</td><td>❓ N/A</td></tr>
<tr><td rowspan="2">16</td><td>DUO4Bit</td><td>N/A</td><td>N/A</td><td rowspan="2">N/A</td><td>❓ N/A</td></tr>
<tr><td>SDP4Bit</td><td>N/A</td><td>N/A</td><td>❓ N/A</td></tr>
<tr><td rowspan="2">8</td><td>DUO4Bit</td><td>N/A</td><td>N/A</td><td rowspan="2">N/A</td><td>❓ N/A</td></tr>
<tr><td>SDP4Bit</td><td>N/A</td><td>N/A</td><td>❓ N/A</td></tr>
<tr><td rowspan="6">2048</td><td rowspan="2">32</td><td>DUO4Bit</td><td>N/A</td><td>N/A</td><td rowspan="2">N/A</td><td>❓ N/A</td></tr>
<tr><td>SDP4Bit</td><td>N/A</td><td>N/A</td><td>❓ N/A</td></tr>
<tr><td rowspan="2">16</td><td>DUO4Bit</td><td>N/A</td><td>N/A</td><td rowspan="2">N/A</td><td>❓ N/A</td></tr>
<tr><td>SDP4Bit</td><td>N/A</td><td>N/A</td><td>❓ N/A</td></tr>
<tr><td rowspan="2">8</td><td>DUO4Bit</td><td>N/A</td><td>N/A</td><td rowspan="2">N/A</td><td>❓ N/A</td></tr>
<tr><td>SDP4Bit</td><td>N/A</td><td>N/A</td><td>❓ N/A</td></tr>
</tbody>
</table>
