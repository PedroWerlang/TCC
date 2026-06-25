# 🔬 Classificação Hierárquica em Cascata para Diagnóstico de Lesões Cutâneas

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-orange)
![ResNet50](https://img.shields.io/badge/Model-ResNet50-success)
![Status](https://img.shields.io/badge/Status-Concluído-brightgreen)

Repositório oficial contendo os códigos e experimentos do meu Trabalho de Conclusão de Curso (TCC). 

Este projeto propõe uma **arquitetura de Deep Learning em cascata** para mitigar o viés de classes majoritárias no diagnóstico de câncer de pele, com foco especial na subpopulação de pacientes idosos ($\ge$ 60 anos), que sofrem com as assinaturas visuais complexas da senescência cutânea.

---

## Arquitetura Proposta

Modelos multiclasse diretos frequentemente negligenciam o Melanoma devido ao desbalanceamento massivo de dados (excesso de Nevus). Para resolver isso, este projeto dividiu o problema em dois níveis utilizando **ResNet50** via *Transfer Learning*:

1. **Filtro Binário (Triagem):** Separa as imagens em *Benignas* ou *Malignas/Risco*. Retém a maior parte dos ruídos diagnósticos.
2. **Filtro Especialista Oncológico:** Recebe apenas os casos suspeitos e classifica entre as classes de alto risco (Melanoma, Carcinoma Basocelular, Carcinoma Espinocelular, etc.).

## Datasets Utilizados

Os experimentos foram conduzidos de forma estritamente independente em dois dos maiores repositórios dermatoscópicos mundiais. Devido ao limite de tamanho do GitHub, as matrizes de imagens originais devem ser baixadas em suas fontes oficiais:

* **HAM10000:** [Harvard Dataverse](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/DBW86T)
* **ISIC 2019:** [ISIC Archive](https://challenge2019.isic-archive.com/data.html)
> *Nota: Os arquivos `.csv` com os metadados demográficos estão disponíveis na pasta `/metadata` deste repositório.*

## Estrutura do Repositório

Optou-se por separar os experimentos em *Jupyter Notebooks* individuais e agrupá-los por base de dados para garantir total reprodutibilidade acadêmica de cada fase do estudo.

```text
📦 TCC-Cascata-Melanoma
 ┣ 📂 metadata/                # Arquivos CSV (GroundTruth e Metadados originais)
 ┣ 📂 notebooks/               # Scripts de treinamento e avaliação
 ┃ ┣ 📂 ISIC/                  # Experimentos com a base ISIC 2019
 ┃ ┃ ┣ 📜 01_Baseline_ISIC.ipynb
 ┃ ┃ ┣ 📜 02_Filtro_Binario_ISIC.ipynb
 ┃ ┃ ┗ 📜 03_Filtro_Especialista_ISIC.ipynb
 ┃ ┗ 📂 HAM10000/              # Experimentos com a base HAM10000
 ┃   ┣ 📜 01_Baseline_HAM10000.ipynb
 ┃   ┣ 📜 02_Filtro_Binario_HAM10000.ipynb
 ┃   ┗ 📜 03_Filtro_Especialista_HAM10000.ipynb
 ┗ 📜 README.md                # Documentação do projeto
```

## Modelos Pré-treinados (Pesos)

Para facilitar a reprodutibilidade e permitir o uso imediato do sistema em cascata sem a necessidade de retreinar as redes do zero, os pesos finais dos modelos (`.keras`) foram disponibilizados publicamente na seção **Releases** deste repositório.

* **[Clique aqui para acessar a página de Releases e baixar os modelos originais](https://github.com/SeuUsuario/NomeDoRepositorio/releases)**

**Exemplo de carregamento rápido:**
```python
from tensorflow.keras.models import load_model
modelo = load_model('caminho_do_download/ResNet50_Filtro_Binario_ISIC.keras')
```

## Principais Resultados

A abordagem em cascata demonstrou ser uma ferramenta altamente confiável de Suporte ao Diagnóstico (CAD), especialmente na faixa demográfica de maior risco. Ao isolar a classe majoritária, o sistema reduziu drasticamente os falsos negativos para o câncer mais letal da pele.

**Sensibilidade específica para detecção de Melanoma (Pacientes Idosos $\ge$ 60 anos):**
* **ISIC 2019:** Salto de 62,0% (Baseline) para **93,5%** (Filtro Especialista)
* **HAM10000:** Salto de 44,9% (Baseline) para **98,8%** (Filtro Especialista)

## Como Executar

1. Clone o repositório: `git clone https://github.com/SeuUsuario/NomeDoRepositorio.git`
2. Baixe as imagens originais nos links oficiais citados acima e organize-as no formato de diretórios exigido pelo `ImageDataGenerator` do Keras (`/Train`, `/Val`, `/Test`).
3. Altere os caminhos das variáveis `base_dir` e `caminho_salvamento` nos notebooks de acordo com o seu ambiente (Google Drive ou Local).
4. Execute as células sequencialmente.

---
*Desenvolvido por Pedro Valentim Mozzaquatro Werlang para o Trabalho de Conclusão de Curso (2026) do curso Ciência da Computação do Instituto da Computação.*
