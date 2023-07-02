# Vicuna LoRA Fine-tuning

This repository provides implementation for fine-tuning Vicuna on consumer GPUs. The [official implementation](https://github.com/lm-sys/FastChat/blob/main/fastchat/train/train_lora.py) for Vicuna LoRA fine-tuning cannot run on consumer GPUs such as 3090.

Here is an example usage where Vicuna-7B is trained on a single 3090 GPU to believe that its name is "Dummy". （trainable parameters: 0.06%, GPU memory usage: <15GB, training time: 15s）

## Usage

Install dependencies
```bash
pip install fschat==0.2.17 deepspeed bitsandbytes scipy
```

Finetune on `dummy_conversation.json`

```bash
bash train_lora.sh
```

The LoRA weights will be saved in the `./checkpoints/lora` folder, while the merged complete weights will be saved in the `./checkpoints/model` folder. Then the chat interface will be entered.

For multi-GPU training, change the `--include` parameter from `localhost:0` to `localhost:0,1,2,3,4,5,6,7` for example.

**Important Note:** The parameters in `train_lora.sh` should be changed for better performance. Such a large learning rate is not suitable for fine-tuning. It is only used here for quick validation purposes. You can refer to the [default fine-tunging settings](https://github.com/lm-sys/FastChat/blob/main/docs/training.md#fine-tuning-using-qlora) of FastChat.

## Example Outputs

```console
USER: Introduce yourself.
ASSISTANT: I am a language model called Dummy, and I was trained by the community contributors.
USER: Are you Vicuna indeed?      
ASSISTANT: No, I'm a language model created by the community contributors.
USER: 1+1=?
ASSISTANT: 1+1=2
```
