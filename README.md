# **Detecção e Contagem de Neurônios via PDI**

**Aluno**: Matheus Henrique Silva de Melo T01

**Vídeo** : [https://youtu.be/65AXh3DYbYA](https://youtu.be/65AXh3DYbYA)

**Disciplina:** Processamento Digital de Imagens

**Metodologia:** Visão Computacional Clássica (Transformada de Hough Circular)

Este repositório contém uma solução automatizada para o problema de detecção e quantificação de núcleos celulares em imagens de microscopia, desenvolvida para a disciplina de Processamento Digital de Imagens.

## **📁 Estrutura do Repositório**

A organização dos arquivos no repositório foi planejada para facilitar a reprodutibilidade dos testes:

* **TrabalhoProcImgsCompleto.ipynb**: O código-fonte principal do projeto em formato Jupyter Notebook. Contém todo o pipeline de processamento, desde os filtros iniciais até a lógica de validação final.  
* **/Dataset1**: Contém a maior parte das imagens provenientes do dataset original de neurônios. É a base de dados principal para análise de variabilidade.  
* **/Dataset2**: Conjunto de imagens selecionadas especificamente para a fase de testes e validação da acurácia do algoritmo.  
* **/Resultado**: Diretório que armazena os outputs gerados pelo processamento das imagens do Dataset2. Aqui estão as provas visuais da eficácia do sistema (detecções de corpo em ciano e brilho em amarelo).

## **🚀 Metodologia: O Pipeline de Múltiplas Etapas**

Diferente de abordagens de Deep Learning, este projeto utiliza **Visão Computacional Clássica** através de uma estratégia de **Hough Duplo** e autenticação de dois fatores. O processamento é dividido em estágios:

1. **Especialização de Camadas**: Uso de **Filtro Gaussiano** para isolar o anel (corpo celular) e **White Top-Hat** para isolar o brilho central (*Glow*).  
2. **Extração de Bordas**: Aplicação do detector **Canny** com parâmetros diferenciados para cada camada, lidando com os desafios de baixo contraste.  
3. **Transformada de Hough Circular**: Implementação manual para busca de raios entre 6 e 13 pixels, com limpeza de duplicatas no acumulador.  
4. **Autenticação de Dois Fatores**: Cruzamento de dados via **Distância Euclidiana**. Um núcleo só é contabilizado se o centro de brilho coincidir com o centro do corpo celular, eliminando ruídos como axônios e poluição luminosa difusa.

## **🛠️ Como Executar**

1. Clone o repositório e certifique-se de ter as bibliotecas scikit-image, numpy e matplotlib instaladas.  
2. Mantenha a estrutura de pastas conforme apresentada acima.  
3. Abra o arquivo .ipynb no Jupyter Notebook ou VS Code e execute as células sequencialmente.

