---
license: cc-by-4.0
base_model: Wespeaker/wespeaker-voxceleb-resnet34-LM
tags:
- speaker-embedding
- wespeaker
- gguf
---

# WeSpeaker ResNet34-LM as GGUF, converted by a2t

The speaker embedding model [a2t](https://github.com/dbgeek/a2t) loads to name known speakers: the weights of [`Wespeaker/wespeaker-voxceleb-resnet34-LM`](https://huggingface.co/Wespeaker/wespeaker-voxceleb-resnet34-LM), unchanged apart from the float16 rounding below, in a GGUF for the ggml runtime. a2t downloads this file itself (`a2t models pull wespeaker-resnet34-lm`); it is of no use to another program without a2t's graph for it.

## Attribution and licence

WeSpeaker ResNet34-LM by the WeSpeaker project (https://github.com/wenet-e2e/wespeaker), CC-BY-4.0; converted to GGUF by a2t.

- **Licence**: `cc-by-4.0`, the licence of the original weights. This file is a conversion of them and is under the same licence.
- **Converted by**: a2t ([dbgeek/a2t](https://github.com/dbgeek/a2t)). The WeSpeaker project did not make or endorse this file.

## Origin

| | |
| --- | --- |
| Repository | [`Wespeaker/wespeaker-voxceleb-resnet34-LM`](https://huggingface.co/Wespeaker/wespeaker-voxceleb-resnet34-LM) |
| Revision | `f0c48c298fd835726c27956a5d617bad7115627e` |
| File | `voxceleb_resnet34_LM.onnx` |
| URL | https://huggingface.co/Wespeaker/wespeaker-voxceleb-resnet34-LM/resolve/f0c48c298fd835726c27956a5d617bad7115627e/voxceleb_resnet34_LM.onnx |
| SHA-256 | `7bb2f06e9df17cdf1ef14ee8a15ab08ed28e8d0ef5054ee135741560df2ec068` |

The original is ungated and is data (ONNX), not a pickle.

## This file

| | |
| --- | --- |
| File | `wespeaker-resnet34-lm.f16.gguf` |
| Size | 15983328 bytes |
| SHA-256 | `c0e861dea36d51abd8d4151089443909f5b4485c7d231499c454185f90f1786f` |

## How it was made

By a2t's checked-in conversion script, [`tools/wespeaker-gguf`](https://github.com/dbgeek/a2t/tree/main/tools/wespeaker-gguf), which downloads the original by the URL above, refuses it unless its SHA-256 matches, and writes this file:

- The 36 convolutions of the ResNet34, which have batch norm folded in by the original export, in graph order as `conv.<i>.weight` and `conv.<i>.bias`. The 3x3 kernels are rounded to float16; the 1x1 kernels and every bias stay float32.
- The linear layer (`seg_1.weight`, `seg_1.bias`) and the export's mean vector (`mean_vec`), float32.
- Metadata: this lineage (`a2t.origin`, `a2t.origin_url`, `a2t.origin_sha256`, `a2t.converter`), the pooling's variance floor, and the filterbank the model expects (80 mel bins at 16000 Hz, a Hamming window).

Nothing is retrained, pruned or quantized below float16. Made with onnx 1.23.2, gguf 0.19.0, numpy 2.1.3, torch 2.5.1, torchaudio 2.5.1, onnxruntime 1.32.0.

## Parity with the original

The script embeds a set of audio clips with the original (the ONNX export on ONNX Runtime) and with this file (its tensors, run through the same ResNet34 graph and pooling in float32), both from the same WeSpeaker filterbank features, and compares the two embeddings of each clip by cosine similarity. It refuses to produce the file if any clip is below 0.99998.

- **Clips**: 49 clips, the script's fixed set of 49. The fixed set is cuts of 0.5 to 60 s from two AMI Meeting Corpus recordings and one public-domain speech.
- **Worst clip**: 0.999997 (`ami-is1009b/60s+0.5s`).
- **Median**: 1.000000.
- **Result**: passed, at a tolerance of 0.99998 per clip.

## Citation

The model is WeSpeaker's; cite the WeSpeaker project:

```bibtex
@inproceedings{wang2023wespeaker,
  title={Wespeaker: A research and production oriented speaker embedding learning toolkit},
  author={Wang, Hongji and Liang, Chengdong and Wang, Shuai and Chen, Zhengyang and Zhang, Binbin and Xiang, Xu and Deng, Yanlei and Qian, Yanmin},
  booktitle={IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP)},
  pages={1--5},
  year={2023},
  organization={IEEE}
}
```
