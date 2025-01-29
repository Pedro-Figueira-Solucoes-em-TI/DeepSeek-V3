# Documentação dos Arquivos de Pesos do DeepSeek-V3

## Novos Campos em `config.json`

- **model_type**: Especifica o tipo do modelo, que foi atualizado para `deepseek_v3` nesta versão.
- **num_nextn_predict_layers**: Indica o número de Módulos de **Previsão de Múltiplos Tokens (MTP)**. Os pesos da versão de código aberto do V3 incluem **1 Módulo MTP**.
- **quantization_config**: Descreve a configuração para a quantização **FP8**.

---

## Visão Geral da Estrutura dos Pesos

O arquivo de pesos do **DeepSeek-V3** é composto por **dois componentes principais**: **Pesos do Modelo Principal** e **Módulos MTP**.

### 1. Pesos do Modelo Principal

- **Composição**:
  - Camadas de embedding de entrada/saída e um conjunto completo de **61 camadas Transformer**.
- **Contagem de Parâmetros**:
  - Parâmetros totais: **671 bilhões**.
  - Parâmetros ativados: **36,7 bilhões** (incluindo **0,9B para Embedding** e **0,9B para a camada de saída**).

#### Detalhes Estruturais

- **Camada de Embedding**:
  - `model.embed_tokens.weight`
- **Camadas Ocultas Transformer**:
  - `model.layers.0` até `model.layers.60`, totalizando `num_hidden_layers` camadas.
- **Camada de Saída**:
  - `model.norm.weight`
  - `lm_head.weight`

### 2. Módulos de Previsão de Múltiplos Tokens (MTP)

- **Composição**:
  - Módulos MTP adicionais, definidos pelo campo `num_nextn_predict_layers`. Neste modelo, o valor é **1**.
- **Contagem de Parâmetros**:
  - **11,5 bilhões de parâmetros únicos**, excluindo os **0,9B compartilhados de Embedding** e os **0,9B da camada de saída**.
  - Parâmetros ativados: **2,4 bilhões** (incluindo os **0,9B compartilhados de Embedding** e os **0,9B da camada de saída**).

#### Detalhes Estruturais

- **embed_tokens**: **Compartilha parâmetros** com a camada de Embedding dos Pesos do Modelo Principal.
- **enorm & hnorm**: Parâmetros **RMSNorm** necessários para **decodificação especulativa**.
- **eh_proj**: Parâmetros para projeção de redução dimensional nos resultados do norm.
- **Camada Oculta Transformer Adicional**:
  - `model.layers.61.self_attn & mlp` (estrutura idêntica às camadas ocultas do Modelo Principal).
- **shared_head**: **Compartilha parâmetros** com a camada de saída dos Pesos do Modelo Principal.

---

### Regras de Carregamento

- **Pesos do Modelo Principal**: Carregados via o parâmetro `num_hidden_layers` no `config.json`.
- **Módulos MTP**: Carregados via o parâmetro `num_nextn_predict_layers`, com IDs de camadas **adicionados imediatamente após as camadas ocultas do Modelo Principal**.  
  Por exemplo:
  - Se `num_hidden_layers = 61` e `num_nextn_predict_layers = 1`, o ID da camada do **Módulo MTP será `61`**.

---

## Documentação dos Pesos FP8

O **DeepSeek-V3** suporta nativamente o **formato de pesos FP8** com escalonamento em blocos **128x128**.

### Configuração FP8

O arquivo de pesos FP8 introduz um campo `quantization_config` para descrever o método de quantização.  
Abaixo está um exemplo de configuração:

```json
"quantization_config": {
  "activation_scheme": "dynamic",
  "fmt": "e4m3",
  "quant_method": "fp8",
  "weight_block_size": [128, 128]
}
```

- **Formato de Quantização**:

  - Tipo de formato: fp8 e e4m3 (correspondente a torch.float8_e4m3fn).
  - Tamanho do bloco de pesos: 128x128.

- **Esquema de Quantização da Ativação**:

  - Utiliza quantização dinâmica da ativação (dynamic).

### Método de Desquantização

O arquivo de pesos FP8 inclui um campo weight_scale_inv, que armazena a escala de desquantização para cada bloco de pesos.

- **Formato de Armazenamento**: Tensor float32, armazenado junto com os dados de peso.

- **Fórmula de Desquantização**:
  - Se o bloco de pesos não estiver alinhado a 128, ele é preenchido com zeros até 128 antes do cálculo da escala. Após a quantização, a parte preenchida é removida.

  - O processo de desquantização é realizado da seguinte forma:

(\text{Bloco de Peso 128x128}) \times \text{weight_scale_inv}
Por meio da desquantização dos pesos FP8, as operações em tempo de execução possibilitam quantização online na granularidade de per-token-per-128-channel.