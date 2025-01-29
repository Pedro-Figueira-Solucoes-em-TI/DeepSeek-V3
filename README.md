<!-- markdownlint-disable first-line-h1 -->
<!-- markdownlint-disable html -->
<!-- markdownlint-disable no-duplicate-header -->

<div align="center">
  <img src="https://github.com/deepseek-ai/DeepSeek-V2/blob/main/figures/logo.svg?raw=true" width="60%" alt="DeepSeek-V3" />
</div>
<hr>
<div align="center" style="line-height: 1;">
  <a href="https://www.deepseek.com/" target="_blank" style="margin: 2px;">
    <img alt="Homepage" src="https://github.com/deepseek-ai/DeepSeek-V2/blob/main/figures/badge.svg?raw=true" style="display: inline-block; vertical-align: middle;"/>
  </a>
  <a href="https://chat.deepseek.com/" target="_blank" style="margin: 2px;">
    <img alt="Chat" src="https://img.shields.io/badge/🤖%20Chat-DeepSeek%20V3-536af5?color=536af5&logoColor=white" style="display: inline-block; vertical-align: middle;"/>
  </a>
  <a href="https://huggingface.co/deepseek-ai" target="_blank" style="margin: 2px;">
    <img alt="Hugging Face" src="https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-DeepSeek%20AI-ffc107?color=ffc107&logoColor=white" style="display: inline-block; vertical-align: middle;"/>
  </a>
</div>

<div align="center" style="line-height: 1;">
  <a href="https://discord.gg/Tc7c45Zzu5" target="_blank" style="margin: 2px;">
    <img alt="Discord" src="https://img.shields.io/badge/Discord-DeepSeek%20AI-7289da?logo=discord&logoColor=white&color=7289da" style="display: inline-block; vertical-align: middle;"/>
  </a>
  <a href="https://github.com/deepseek-ai/DeepSeek-V2/blob/main/figures/qr.jpeg?raw=true" target="_blank" style="margin: 2px;">
    <img alt="Wechat" src="https://img.shields.io/badge/WeChat-DeepSeek%20AI-brightgreen?logo=wechat&logoColor=white" style="display: inline-block; vertical-align: middle;"/>
  </a>
  <a href="https://twitter.com/deepseek_ai" target="_blank" style="margin: 2px;">
    <img alt="Twitter Follow" src="https://img.shields.io/badge/Twitter-deepseek_ai-white?logo=x&logoColor=white" style="display: inline-block; vertical-align: middle;"/>
  </a>
</div>

<div align="center" style="line-height: 1;">
  <a href="https://github.com/deepseek-ai/DeepSeek-V3/blob/main/LICENSE-CODE" style="margin: 2px;">
    <img alt="Code License" src="https://img.shields.io/badge/Code_License-MIT-f5de53?&color=f5de53" style="display: inline-block; vertical-align: middle;"/>
  </a>
  <a href="https://github.com/deepseek-ai/DeepSeek-V3/blob/main/LICENSE-MODEL" style="margin: 2px;">
    <img alt="Model License" src="https://img.shields.io/badge/Model_License-Model_Agreement-f5de53?&color=f5de53" style="display: inline-block; vertical-align: middle;"/>
  </a>
</div>


<p align="center">
  <a href="DeepSeek_V3.pdf"><b>Paper Link</b>👁️</a>
</p>


Aqui está a tradução formatada em Markdown dentro de um bloco de código:

## 1. Introdução

Apresentamos o **DeepSeek-V3**, um poderoso modelo de linguagem do tipo *Mixture-of-Experts* (MoE) com **671 bilhões de parâmetros totais**, dos quais **37 bilhões são ativados para cada token**.

Para alcançar inferência eficiente e treinamento econômico, o DeepSeek-V3 adota a arquitetura **Multi-head Latent Attention (MLA)** e **DeepSeekMoE**, que foram validadas no DeepSeek-V2.

Além disso, o DeepSeek-V3 é pioneiro em uma estratégia **livre de perda auxiliar** para balanceamento de carga e estabelece um objetivo de treinamento baseado em **previsão de múltiplos tokens**, garantindo um desempenho superior.

Pré-treinamos o DeepSeek-V3 em **14,8 trilhões de tokens diversos e de alta qualidade**, seguido pelas etapas de **Ajuste Supervisionado (Supervised Fine-Tuning)** e **Aprendizado por Reforço (Reinforcement Learning)** para maximizar suas capacidades.

Avaliações abrangentes revelam que o DeepSeek-V3 **supera outros modelos de código aberto** e **atinge um desempenho comparável aos principais modelos proprietários**.

Apesar do seu desempenho excepcional, o DeepSeek-V3 exige apenas **2,788 milhões de horas-GPU H800** para seu treinamento completo.

Além disso, seu processo de treinamento é notavelmente **estável**, sem apresentar picos de perda irrecuperáveis ou necessidade de rollback durante todo o treinamento.

<p align="center">
  <img width="80%" src="figures/benchmark.png">
</p>

## 2. Resumo do Modelo

---

### **Arquitetura: Estratégia Inovadora de Balanceamento de Carga e Objetivo de Treinamento**

- Com base na arquitetura eficiente do DeepSeek-V2, introduzimos uma estratégia **livre de perda auxiliar** para balanceamento de carga, reduzindo a degradação de desempenho associada a essa técnica.
- Investigamos um objetivo de **Previsão de Múltiplos Tokens (Multi-Token Prediction - MTP)** e comprovamos seus benefícios no desempenho do modelo.  
  Essa abordagem também pode ser utilizada para **aceleração da inferência por meio de decodificação especulativa**.

---

### **Pré-Treinamento: Eficiência Máxima no Treinamento**

- Projetamos uma estrutura de treinamento com **precisão mista FP8** e, pela primeira vez, validamos sua **viabilidade e eficácia em modelos de grande escala**.  
- Por meio de um co-design entre **algoritmos, frameworks e hardware**, superamos os gargalos de comunicação no treinamento MoE entre nós, atingindo **quase 100% de sobreposição entre computação e comunicação**.  
  Isso melhora significativamente a eficiência do treinamento e reduz os custos, permitindo **escalar o tamanho do modelo sem sobrecarga adicional**.  
- Com um custo econômico de apenas **2,664 milhões de horas-GPU H800**, completamos o pré-treinamento do DeepSeek-V3 em **14,8 trilhões de tokens**, resultando no **modelo base de código aberto mais poderoso atualmente**.  
  As etapas de treinamento posteriores ao pré-treinamento requerem apenas **0,1 milhão de horas-GPU**.

---

### **Pós-Treinamento: Distilação de Conhecimento do DeepSeek-R1**

- Introduzimos uma **metodologia inovadora** para distilar capacidades de raciocínio de modelos **Chain-of-Thought (CoT)** de longa cadeia, especificamente da série **DeepSeek R1**, para **LLMs padrão**, como o DeepSeek-V3.  
  Nosso pipeline incorpora elegantemente os padrões de **verificação e reflexão** do R1 no DeepSeek-V3, aprimorando **significativamente seu desempenho em raciocínio**.  
  Além disso, **controlamos o estilo e o comprimento da saída** do DeepSeek-V3.

---

## 3. Download do Modelo

<div align="center">

| **Modelo** | **#Parâmetros Totais** | **#Parâmetros Ativados** | **Comprimento do Contexto** | **Download** |
| :------------: | :------------: | :------------: | :------------: | :------------: |
| DeepSeek-V3-Base | 671B | 37B | 128K   | [🤗 Hugging Face](https://huggingface.co/deepseek-ai/DeepSeek-V3-Base)   |
| DeepSeek-V3   | 671B | 37B |  128K   | [🤗 Hugging Face](https://huggingface.co/deepseek-ai/DeepSeek-V3)   |

</div>

> **Nota:** O tamanho total dos modelos DeepSeek-V3 no Hugging Face é **685B**, incluindo **671B dos pesos do modelo principal** e **14B do módulo Multi-Token Prediction (MTP)**.

Para garantir **flexibilidade e desempenho ideais**, colaboramos com comunidades de código aberto e fabricantes de hardware para fornecer **múltiplas maneiras de rodar o modelo localmente**. Para um guia passo a passo, consulte a **Seção 6: [Como Executar Localmente](#6-how-to-run-locally)**.

Se você deseja aprofundar-se nos detalhes técnicos, recomendamos explorar o arquivo **[README_WEIGHTS.md](./README_WEIGHTS.md)**, que descreve os **pesos do Modelo Principal e os Módulos MTP**.  
Vale lembrar que **o suporte ao MTP ainda está em desenvolvimento** dentro da comunidade, e sua colaboração é bem-vinda.

## 4. Resultados da Avaliação

### Modelo Base
#### **Benchmarks Padrão**

<div align="center">

|  | **Benchmark (Métrica)** | **# Exemplos** | **DeepSeek-V2** | **Qwen2.5 72B** | **LLaMA3.1 405B** | **DeepSeek-V3** |
|---|-------------------|----------|--------|-------------|---------------|---------|
| **Arquitetura** | - | - | MoE | Denso | Denso | MoE |
| **# Parâmetros Ativados** | - | - | 21B | 72B | 405B | 37B |
| **# Parâmetros Totais** | - | - | 236B | 72B | 405B | 671B |
| **Inglês** | Pile-test (BPB) | - | 0.606 | 0.638 | **0.542** | 0.548 |
| | MMLU (Acc.) | 5-shot | 78.4 | 85.0 | 84.4 | **87.1** |
| **Código** | HumanEval (Pass@1) | 0-shot | 43.3 | 53.0 | 54.9 | **65.2** |
| | MBPP (Pass@1) | 3-shot | 65.0 | 72.6 | 68.4 | **75.4** |
| **Matemática** | GSM8K (EM) | 8-shot | 81.6 | 88.3 | 83.5 | **89.3** |
| | MATH (EM) | 4-shot | 43.4 | 54.4 | 49.0 | **61.6** |

</div>

> **Nota:** Os melhores resultados estão em **negrito**. Diferenças menores que **0,3 pontos** são consideradas **empates técnicos**. O DeepSeek-V3 obtém **o melhor desempenho** na maioria dos benchmarks, especialmente em **tarefas de matemática e programação**.

---

## 5. Plataforma de Chat & API

Você pode interagir com o **DeepSeek-V3** através da plataforma oficial:

🔹 **Site Oficial**: [chat.deepseek.com](https://chat.deepseek.com/sign_in)  
🔹 **API Compatível com OpenAI**: [platform.deepseek.com](https://platform.deepseek.com/)

---

## 6. Como Executar Localmente

O DeepSeek-V3 pode ser implantado localmente utilizando diversas soluções, como **DeepSeek-Infer**, **SGLang**, **vLLM**, **TensorRT-LLM**, **LMDeploy**, **AMD GPU** e **Huawei Ascend NPU**.

### **6.1 Inferência com DeepSeek-Infer Demo**

#### **Requisitos do Sistema**
- Linux com Python **3.10**  
- Mac e Windows **não são suportados**  
- Dependências principais: `torch==2.4.1`, `triton==3.0.0`, `transformers==4.46.3`, `safetensors==0.4.5`

---

## 7. Licença

O código está sob a **[Licença MIT](LICENSE-CODE)**. O uso dos modelos DeepSeek-V3 está sujeito à **[Licença do Modelo](LICENSE-MODEL)** e **suporta uso comercial**.


## 8. Citation
```
@misc{deepseekai2024deepseekv3technicalreport,
      title={DeepSeek-V3 Technical Report}, 
      author={DeepSeek-AI and Aixin Liu and Bei Feng and Bing Xue and Bingxuan Wang and Bochao Wu and Chengda Lu and Chenggang Zhao and Chengqi Deng and Chenyu Zhang and Chong Ruan and Damai Dai and Daya Guo and Dejian Yang and Deli Chen and Dongjie Ji and Erhang Li and Fangyun Lin and Fucong Dai and Fuli Luo and Guangbo Hao and Guanting Chen and Guowei Li and H. Zhang and Han Bao and Hanwei Xu and Haocheng Wang and Haowei Zhang and Honghui Ding and Huajian Xin and Huazuo Gao and Hui Li and Hui Qu and J. L. Cai and Jian Liang and Jianzhong Guo and Jiaqi Ni and Jiashi Li and Jiawei Wang and Jin Chen and Jingchang Chen and Jingyang Yuan and Junjie Qiu and Junlong Li and Junxiao Song and Kai Dong and Kai Hu and Kaige Gao and Kang Guan and Kexin Huang and Kuai Yu and Lean Wang and Lecong Zhang and Lei Xu and Leyi Xia and Liang Zhao and Litong Wang and Liyue Zhang and Meng Li and Miaojun Wang and Mingchuan Zhang and Minghua Zhang and Minghui Tang and Mingming Li and Ning Tian and Panpan Huang and Peiyi Wang and Peng Zhang and Qiancheng Wang and Qihao Zhu and Qinyu Chen and Qiushi Du and R. J. Chen and R. L. Jin and Ruiqi Ge and Ruisong Zhang and Ruizhe Pan and Runji Wang and Runxin Xu and Ruoyu Zhang and Ruyi Chen and S. S. Li and Shanghao Lu and Shangyan Zhou and Shanhuang Chen and Shaoqing Wu and Shengfeng Ye and Shengfeng Ye and Shirong Ma and Shiyu Wang and Shuang Zhou and Shuiping Yu and Shunfeng Zhou and Shuting Pan and T. Wang and Tao Yun and Tian Pei and Tianyu Sun and W. L. Xiao and Wangding Zeng and Wanjia Zhao and Wei An and Wen Liu and Wenfeng Liang and Wenjun Gao and Wenqin Yu and Wentao Zhang and X. Q. Li and Xiangyue Jin and Xianzu Wang and Xiao Bi and Xiaodong Liu and Xiaohan Wang and Xiaojin Shen and Xiaokang Chen and Xiaokang Zhang and Xiaosha Chen and Xiaotao Nie and Xiaowen Sun and Xiaoxiang Wang and Xin Cheng and Xin Liu and Xin Xie and Xingchao Liu and Xingkai Yu and Xinnan Song and Xinxia Shan and Xinyi Zhou and Xinyu Yang and Xinyuan Li and Xuecheng Su and Xuheng Lin and Y. K. Li and Y. Q. Wang and Y. X. Wei and Y. X. Zhu and Yang Zhang and Yanhong Xu and Yanhong Xu and Yanping Huang and Yao Li and Yao Zhao and Yaofeng Sun and Yaohui Li and Yaohui Wang and Yi Yu and Yi Zheng and Yichao Zhang and Yifan Shi and Yiliang Xiong and Ying He and Ying Tang and Yishi Piao and Yisong Wang and Yixuan Tan and Yiyang Ma and Yiyuan Liu and Yongqiang Guo and Yu Wu and Yuan Ou and Yuchen Zhu and Yuduan Wang and Yue Gong and Yuheng Zou and Yujia He and Yukun Zha and Yunfan Xiong and Yunxian Ma and Yuting Yan and Yuxiang Luo and Yuxiang You and Yuxuan Liu and Yuyang Zhou and Z. F. Wu and Z. Z. Ren and Zehui Ren and Zhangli Sha and Zhe Fu and Zhean Xu and Zhen Huang and Zhen Zhang and Zhenda Xie and Zhengyan Zhang and Zhewen Hao and Zhibin Gou and Zhicheng Ma and Zhigang Yan and Zhihong Shao and Zhipeng Xu and Zhiyu Wu and Zhongyu Zhang and Zhuoshu Li and Zihui Gu and Zijia Zhu and Zijun Liu and Zilin Li and Ziwei Xie and Ziyang Song and Ziyi Gao and Zizheng Pan},
      year={2024},
      eprint={2412.19437},
      archivePrefix={arXiv},
      primaryClass={cs.CL},
      url={https://arxiv.org/abs/2412.19437}, 
}
```

## 9. Contact
If you have any questions, please raise an issue or contact us at [service@deepseek.com](service@deepseek.com).
