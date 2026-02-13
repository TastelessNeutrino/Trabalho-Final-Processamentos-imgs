# **Apresentação : Problema de Detecção de Neurônios**

Aluno: Matheus Henrique Silva de Melo T01

Video : [https://youtu.be/65AXh3DYbYA](https://youtu.be/65AXh3DYbYA)

Este documento descreve a solução para o **problema de detecção e contagem** de células por seus núcleos, utilizando um pipeline estruturado em **múltiplas etapas** de processamento digital de imagens.

## **1. O Desafio da Detecção e Contagem**

O objetivo central deste trabalho é resolver o problema de detecção e contagem de neurônios em imagens de microscopia. O primeiro grande aprendizado foi entender que, neste cenário, o **Thresholding (limiarização)** é um inimigo da precisão. As células raramente estão isoladas; elas aparecem coladas ou sobrepostas. Se eu tentasse apenas binarizar a imagem, o resultado seria um "amontoado" de pixels onde três ou quatro neurônios virariam uma única mancha. A solução exigiu uma abordagem geométrica dividida em etapas sequenciais.

## **2. Processamento em Múltiplas Etapas: Especialização de Camadas**

Para aumentar a acurácia da detecção, tratei a imagem original através de diferentes estágios de filtragem, criando duas versões especializadas:

* **Etapa de Contorno (Corpo):** Utilização do **Filtro Gaussiano**. Ele funciona como um "borrador" inteligente que remove a textura granulada de dentro da célula e foca no que importa: a borda circular.
* **Etapa de Núcleo (Glow):** Utilização do **White Top-Hat**. Este estágio é fundamental para extrair apenas o que é pequeno e muito brilhante, ignorando as variações lentas de luz e ruídos de fundo.

## **3. Validação: A "Autenticação de Dois Fatores"**

Talvez o maior aprendizado técnico tenha sido a percepção de que cada etapa de detecção isolada gera um tipo específico de falso positivo:

* O **Canny** (detector de bordas) é excelente, mas se perde nos **axônios**, tentando desenhar círculos onde só existem linhas.
* O brilho central sofre com a **poluição luminosa**, criando detecções em borrões de luz fora de foco.

A solução foi implementar uma etapa de **autenticação cruzada**. Ao cruzar a coordenada de um círculo de "corpo" com um de "brilho" usando a **Distância Euclidiana**, consegui filtrar quase 100% do ruído. Se o brilho e o anel não ocupam o mesmo espaço, o sistema não contabiliza o objeto.

## **4. Implementação do Acumulador de Hough**

A construção manual da Transformada de Hough (reaproveitada da atividade 4) permitiu um controle fino sobre o acumulador em múltiplas escalas:

* Restrição de raio entre **6 e 13 pixels** para eliminar estruturas irrelevantes.
* Ajuste da distância mínima entre círculos para evitar que um axônio fosse lido como uma "fila" de células.

## **5. Etapa Final: Limpeza e Seleção do Melhor Voto**

Para resolver a sobreposição de detecções (vários círculos para o mesmo núcleo), apliquei uma etapa de **limpeza de duplicatas**. Em vez de aceitar todos os votos do acumulador, o código analisa a região e mantém apenas o **voto mais forte** (o pico do acumulador). Isso transformou um diagnóstico poluído em uma contagem final limpa e única por neurônio.

## **6. Conclusão e Resultados**

O teste em um dataset variado provou que a robustez do sistema não vem de um único filtro, mas da eficácia da **combinação de múltiplas etapas**. O sistema focado no "brilho autenticado pelo anel" mostrou-se muito mais confiável para a contagem final do que qualquer método de etapa única ou segmentação direta por pixels.

