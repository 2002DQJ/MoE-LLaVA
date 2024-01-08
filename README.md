

<p align="center">
    <img src="https://s11.ax1x.com/2023/12/28/piqvDMV.png" width="250" style="margin-bottom: 0.2;"/>
<p>
<h2 align="center"> <a href="">MoE-LLaVA: Mixture of Experts for Large Vision-Language Models</a></h2>
<h5 align="center"> If you like our project, please give us a star ⭐ on GitHub for latest update.  </h2>



<h5 align="center">
    
[![hf_space](https://img.shields.io/badge/🤗-Open%20In%20Spaces-blue.svg)](https://huggingface.co/spaces/LanguageBind/MoE-LLaVA)
[![License](https://img.shields.io/badge/License-Apache%202.0-yellow)](https://github.com/PKU-YuanGroup/MoE-LLaVA/blob/main/LICENSE) 
[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2FPKU-YuanGroup%2FMoE-LLaVA&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=Visitor&edge_flat=false)](https://hits.seeyoufarm.com)
[![GitHub issues](https://img.shields.io/github/issues/PKU-YuanGroup/MoE-LLaVA?color=critical&label=Issues)](https://github.com/PKU-YuanGroup/MoE-LLaVA/issues?q=is%3Aopen+is%3Aissue)
[![GitHub closed issues](https://img.shields.io/github/issues-closed/PKU-YuanGroup/MoE-LLaVA?color=success&label=Issues)](https://github.com/PKU-YuanGroup/MoE-LLaVA/issues?q=is%3Aissue+is%3Aclosed)  <br>

</h5>

<details open><summary>💡 I also have other vision-language projects that may interest you ✨. </summary><p>
<!--  may -->

> [**Video-LLaVA: Learning United Visual Representation by Alignment Before Projection**](https://arxiv.org/abs/2311.10122) <br>
> Bin Lin, Yang Ye, Bin Zhu, Jiaxi Cui, Munan Ning, Peng Jin, Li Yuan <br>
[![github](https://img.shields.io/badge/-Github-black?logo=github)](https://github.com/PKU-YuanGroup/Video-LLaVA) [![arXiv](https://img.shields.io/badge/Arxiv-2311.10122-b31b1b.svg?logo=arXiv)](https://arxiv.org/abs/2311.10122) <br>

> [**LanguageBind: Extending Video-Language Pretraining to N-modality by Language-based Semantic Alignment**](https://arxiv.org/abs/2310.01852) <br>
> Bin Zhu, Bin Lin, Munan Ning, Yang Yan, Jiaxi Cui, HongFa Wang, Yatian Pang, Wenhao Jiang, Junwu Zhang, Zongwei Li, Wancai Zhang, Zhifeng Li, Wei Liu, Li Yuan <br>
[![github](https://img.shields.io/badge/-Github-black?logo=github)](https://github.com/PKU-YuanGroup/LanguageBind) [![arXiv](https://img.shields.io/badge/Arxiv-2310.01852-b31b1b.svg?logo=arXiv)](https://arxiv.org/abs/2310.01852) <br>

</p></details>


## 📰 News

* **[2024.1.1]**  🤗[Hugging Face demo](https://huggingface.co/spaces/LanguageBind/MoE-LLaVA) and **all codes & datasets** are available now! Welcome to **watch** 👀 this repository for the latest updates.

## 😮 Highlights

MoE-LLaVA shows excellent performance in multi-modal learning.

### 🚀 Simple baseline, learning multi-modal interactions with sparse pathways.
- With the addition of **a simple MoE tuning stage**, we can complete the training of MoE-LLaVA on **8 V100 GPUs** within 2 days.

### 🔥 High performance, but with fewer parameters
- The sparse model with **2 billion parameters** outperformed the dense model with 7 billion parameters, demonstrating significant superiority in multi-modal learning.

<p align="left">
<img src="assets/res_img.jpg" width=80%>
</p>

## 🤗 Demo

* **Gradio Web UI**

Highly recommend trying out our web demo by the following command, which incorporates all features currently supported by MoE-LLaVA. We also provide [online demo](https://huggingface.co/spaces/LanguageBind/MoE-LLaVA) in Huggingface Spaces.
```bash
deepspeed moellava/serve/gradio_web_server.py --model-path "LanguageBind/MoE-LLaVA" 
```

* **CLI Inference**

```bash
deepspeed moellava/serve/cli.py --model-path "LanguageBind/MoE-LLaVA"  --image-file "image.jpg"
```

<img src="assets/imagecli.gif" width="500" />


## ⚙️ Requirements and Installation
* Python >= 3.10
* Pytorch == 2.0.1
* CUDA Version >= 11.7
* Install required packages:
```bash
git clone https://github.com/PKU-YuanGroup/MoE-LLaVA
cd MoE-LLaVA
conda create -n moellava python=3.10 -y
conda activate moellava
pip install --upgrade pip  # enable PEP 660 support
pip install -e .
pip install -e ".[train]"
pip install flash-attn --no-build-isolation
pip install decord opencv-python git+https://github.com/facebookresearch/pytorchvideo.git@28fe037d212663c6a24f373b94cc5d478c8c1a1d
```

## 🗝️ Training & Validating
The training & validating instruction is in [TRAIN_AND_VALIDATE.md](TRAIN_AND_VALIDATE.md).

## 💡 Customizing your MoE
The instruction is in [CUSTOM_MOE.md](CUSTOM_MOE.md).

## 🤖 API
**We open source all codes.** If you want to load the model (e.g. ```LanguageBind/MoE-LLaVA```) on local, you can use the following code snippets.

**Using the following command to run the code.**

```bash
deepspeed predict.py
```

```python
import torch
from moellava.constants import IMAGE_TOKEN_INDEX, DEFAULT_IMAGE_TOKEN
from moellava.conversation import conv_templates, SeparatorStyle
from moellava.model.builder import load_pretrained_model
from moellava.utils import disable_torch_init
from moellava.mm_utils import tokenizer_image_token, get_model_name_from_path, KeywordsStoppingCriteria

def main():
    disable_torch_init()
    image = 'moellava/serve/examples/extreme_ironing.jpg'
    inp = 'What is unusual about this image?'
    model_path = 'LanguageBind/MoE-LLaVA'
    device = 'cuda'
    load_4bit, load_8bit = False, False
    model_name = get_model_name_from_path(model_path)
    tokenizer, model, processor, context_len = load_pretrained_model(model_path, None, model_name, load_8bit, load_4bit, device=device)
    image_processor = processor['image']
    conv_mode = "v1_qwen"
    conv = conv_templates[conv_mode].copy()
    roles = conv.roles

    image_tensor = image_processor.preprocess(image, return_tensors='pt')['pixel_values'].to(model.device, dtype=torch.float16)


    print(f"{roles[1]}: {inp}")
    inp = DEFAULT_IMAGE_TOKEN + '\n' + inp
    conv.append_message(conv.roles[0], inp)
    conv.append_message(conv.roles[1], None)
    prompt = conv.get_prompt()
    input_ids = tokenizer_image_token(prompt, tokenizer, IMAGE_TOKEN_INDEX, return_tensors='pt').unsqueeze(0).cuda()
    stop_str = conv.sep if conv.sep_style != SeparatorStyle.TWO else conv.sep2
    keywords = [stop_str]
    stopping_criteria = KeywordsStoppingCriteria(keywords, tokenizer, input_ids)

    with torch.inference_mode():
        output_ids = model.generate(
            input_ids,
            images=image_tensor,
            do_sample=True,
            temperature=0.2,
            max_new_tokens=1024,
            use_cache=True,
            stopping_criteria=[stopping_criteria])

    outputs = tokenizer.decode(output_ids[0, input_ids.shape[1]:], skip_special_tokens=True).strip()
    print(outputs)

if __name__ == '__main__':
    main()
```

## 🙌 Related Projects
* [Video-LLaVA](https://github.com/PKU-YuanGroup/Video-LLaVA) This framework empowers the model to efficiently utilize the united visual tokens.
* [LanguageBind](https://github.com/PKU-YuanGroup/LanguageBind) An open source five modalities language-based retrieval framework.

## 👍 Acknowledgement
* [LLaVA](https://github.com/haotian-liu/LLaVA) The codebase we built upon and it is an efficient large language and vision assistant.

## 🔒 License
* The majority of this project is released under the Apache 2.0 license as found in the [LICENSE](https://github.com/PKU-YuanGroup/Video-LLaVA/blob/main/LICENSE) file.
* The service is a research preview intended for non-commercial use only, subject to the model [License](https://github.com/facebookresearch/llama/blob/main/MODEL_CARD.md) of LLaMA, [Terms of Use](https://openai.com/policies/terms-of-use) of the data generated by OpenAI, and [Privacy Practices](https://chrome.google.com/webstore/detail/sharegpt-share-your-chatg/daiacboceoaocpibfodeljbdfacokfjb) of ShareGPT. Please contact us if you find any potential violation.

<!--

## ✏️ Citation
If you find our paper and code useful in your research, please consider giving a star :star: and citation :pencil:.

```BibTeX
@article{lin2023video,
  title={Video-LLaVA: Learning United Visual Representation by Alignment Before Projection},
  author={Lin, Bin and Zhu, Bin and Ye, Yang and Ning, Munan and Jin, Peng and Yuan, Li},
  journal={arXiv preprint arXiv:2311.10122},
  year={2023}
}
```

```BibTeX
@article{zhu2023languagebind,
  title={LanguageBind: Extending Video-Language Pretraining to N-modality by Language-based Semantic Alignment},
  author={Zhu, Bin and Lin, Bin and Ning, Munan and Yan, Yang and Cui, Jiaxi and Wang, HongFa and Pang, Yatian and Jiang, Wenhao and Zhang, Junwu and Li, Zongwei and others},
  journal={arXiv preprint arXiv:2310.01852},
  year={2023}
}
```
-->

<!--
## ✨ Star History
[![Star History](https://api.star-history.com/svg?repos=PKU-YuanGroup/MoE-LLaVA&type=Date)](https://star-history.com/#PKU-YuanGroup/MoE-LLaVA&Date)
-->

## 🤝 Contributors

<a href="https://github.com/PKU-YuanGroup/MoE-LLaVA/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=PKU-YuanGroup/MoE-LLaVA" />
</a>
