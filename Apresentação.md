# **Trabalho Final: Detecção e Contagem de Núcleos Celulares**

**Aluno**: Matheus Henrique Silva de Melo T01

**Vídeo** : [https://youtu.be/65AXh3DYbYA](https://youtu.be/65AXh3DYbYA)

**Disciplina:** Processamento Digital de Imagens

**Metodologia:** Visão Computacional Clássica (Transformada de Hough Circular)

## **1\. Introdução e Objetivo do Projeto**

Este projeto apresenta uma solução automatizada para o problema de detecção e quantificação de núcleos de neurônios em imagens de microscopia. O foco central desta pesquisa é superar as limitações críticas da segmentação baseada puramente em pixels, como a limiarização simples, que costuma falhar em cenários de Bioimagem de alta complexidade. Entre os principais desafios abordados estão o baixo contraste, onde o fundo ruidoso se confunde com as células, e o desafio dos aglomerados, em que núcleos encostados são erroneamente fundidos em um único objeto por métodos tradicionais. Ao contrário das soluções de Deep Learning, frequentemente tratadas como "caixas pretas", esta abordagem prioriza a transparência matemática e o controle total sobre os parâmetros geométricos.

## **2\. Metodologia: A Arquitetura de Múltiplas Etapas**

Para garantir uma acurácia elevada, o sistema não tenta localizar a célula em uma única operação. Em vez disso, utiliza uma estratégia de dupla detecção baseada na morfologia concêntrica do neurônio, dividindo o processamento em duas camadas especializadas que extraem características distintas da imagem original.

A primeira camada foca no Brilho Central, também chamado de *Glow*, que corresponde ao centro de alta intensidade luminosa do núcleo. Para isolar essa característica, aplico o filtro *White Top-Hat*, que é eficiente na extração de pontos de luz pequenos e intensos enquanto ignora as variações lentas de iluminação do fundo. Após esse realce, o detector de bordas Canny é configurado com limiares elevados para capturar apenas a transição abrupta de brilho que define o coração da célula.

A segunda camada foca no Corpo Celular, ou anel, que representa a silhueta cinza envolvendo o núcleo. Nesta etapa, utilizo a suavização Gaussiana para remover as texturas granuladas internas e transformar o corpo em uma forma visualmente suave e contínua. Em seguida, um segundo detector Canny, desta vez com limiares baixos, é aplicado para capturar os contornos externos mais sutis da membrana celular, garantindo que a estrutura periférica da célula seja devidamente delineada.

## **3\. Calibração e Parâmetros Técnicos**

A eficácia deste sistema depende de uma calibração fina de parâmetros que controlam a sensibilidade de cada etapa. O parâmetro **Sigma**, aplicado na suavização Gaussiana inicial, é ajustado para equilibrar a remoção de ruído e a preservação do formato circular; um valor muito baixo manteria granulações que enganam o Hough, enquanto um valor muito alto poderia deformar as bordas do corpo celular. No detector de bordas Canny, os **limiares (thresholds)** são diferenciados: na camada de brilho, utilizo valores altos para ignorar borrões de luz fraca, enquanto na camada de corpo, utilizo valores mais baixos para permitir que contornos de menor contraste sejam rastreados.

Na etapa da Transformada de Hough, o parâmetro de **Acumulador Mínimo** (ou votos) define quantos pixels da borda devem "concordar" com a existência de um círculo. Este valor foi ajustado para cerca de 45% da circunferência, o que confere robustez para detectar núcleos que estejam parcialmente obstruídos ou com bordas falhas. Outro ponto crítico é a **Distância Mínima** configurada no algoritmo de Hough, que impede que o sistema detecte múltiplos círculos dentro da mesma célula ou ao longo de um axônio, forçando o algoritmo a escolher apenas o centro mais provável em cada região.

## **4\. Implementação da Transformada de Hough Circular (CHT)**

A Transformada de Hough foi implementada manualmente para permitir um controle rigoroso sobre o acumulador, garantindo que a detecção fosse fiel à realidade biológica dos neurônios. O sistema foi calibrado para buscar raios exclusivamente entre 6 e 13 pixels, o que permite filtrar automaticamente qualquer estrutura que não se encaixe no perfil esperado das células estudadas. Para lidar com a sobreposição de votos, desenvolvi uma função customizada de limpeza de picos que utiliza um método iterativo. Esse processo analisa as regiões de alta densidade no acumulador e mantém apenas o voto mais forte para cada célula, eliminando círculos redundantes ou deslocados. O uso de limiares absolutos de votação garante que artefatos brilhantes, mas sem forma circular consistente, não mascarem os núcleos reais.

## **5\. Validação por "Autenticação de Dois Fatores"**

O principal diferencial técnico desta solução reside no cruzamento inteligente de dados entre as camadas detectadas, funcionando como uma autenticação de dois fatores. Cada etapa de detecção isolada possui falhas intrínsecas: a camada de corpo celular tende a detectar axônios como ruído estrutural, enquanto a camada de brilho é frequentemente enganada pela poluição luminosa de células fora de foco.

A regra de validação estabelece que um núcleo só é autenticado e contabilizado se houver uma coincidência geométrica precisa, onde um centro de brilho deve existir obrigatoriamente dentro do raio de um anel de corpo celular. Essa verificação é realizada através do cálculo da **distância euclidiana** entre os centros detectados em ambas as camadas. O parâmetro de tolerância para essa distância foi fixado em aproximadamente 8 pixels, garantindo que mesmo pequenos desalinhamentos naturais na captura da imagem não impeçam a autenticação, mas descartando sumariamente objetos que não apresentem essa sobreposição central, como ruídos ópticos ou estruturais.

## **6\. Conclusão e Resultados**

O algoritmo demonstrou ser uma alternativa robusta, estável e extremamente leve se comparada aos modelos de Deep Learning usados originalmente no desafio Roboflow. A principal vantagem reside na eficiência computacional, permitindo que o processamento ocorra em frações de segundo sem a necessidade de hardware especializado como GPUs. Além disso, a precisão geométrica obtida através da análise dos picos do acumulador de Hough provou ser superior à binarização por pixels para separar células encostadas. O sistema de autenticação cruzada garante que apenas estruturas que possuem simultaneamente corpo e brilho central sejam validadas, mantendo o desempenho do algoritmo consistente mesmo em datasets com alta variabilidade ruidosa.

**Desenvolvido como projeto final de Processamento Digital de Imagens.**