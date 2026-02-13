# Trabalho-Final-Processamentos-imgs
Aluno : Matheus Henrique Silva de Melo T01 https://youtu.be/65AXh3DYbYA


Trabalho Final: Detecção e Contagem de Núcleos Celulares
Disciplina: Processamento Digital de Imagens
Metodologia: Visão Computacional Clássica (Transformada de Hough)
1. Introdução e Objetivo
Este projeto apresenta uma solução automatizada para a detecção e quantificação de núcleos de neurônios em imagens de microscopia. O foco principal é a superação de desafios comuns em Bioimagem, como o baixo contraste e, crucialmente, a separação de núcleos aglomerados, onde técnicas de limiarização simples falham ao fundir múltiplos objetos.
2. Metodologia: Estratégia de Dupla Detecção
Para garantir precisão sem o uso de Inteligência Artificial, o algoritmo baseia-se na morfologia específica das células, que apresentam dois componentes concêntricos detectáveis:
2.1. O Brilho Central (Glow)
Características: Ponto interno de alta intensidade (quase branco).
Tratamento: Realçado via filtro White Top-Hat para remover variações de iluminação do fundo e ruídos de baixa frequência.
Detecção: Filtro Canny com limiar elevado focado em transições abruptas de brilho.
2.2. A Borda Externa (Corpo)
Características: Anel de intensidade média (cinza) que define o contorno celular.
Tratamento: Suavização Gaussiana para eliminar texturas celulares internas e focar na silhueta.
Detecção: Filtro Canny com limiar baixo para capturar contornos suaves da membrana/corpo.
3. Implementação da Transformada de Hough Circular (CHT)
A Transformada de Hough é utilizada para inferir a geometria circular mesmo em perímetros incompletos ou ruidosos.
Busca de Raios: O sistema foi calibrado para um range nuclear típico observado no dataset, entre 6 e 13 pixels.
Votação e Picos: O acumulador de Hough é processado por uma função customizada de detecção de picos que aplica Non-Maximum Suppression (NMS), garantindo que cada núcleo seja contado apenas uma vez e eliminando redundâncias.
Limiar de Voto: Definido de forma absoluta para garantir estabilidade, independente de artefatos extremamente brilhantes na imagem que pudessem mascarar os núcleos reais.
4. Validação por Coincidência Geométrica
A decisão final da contagem não depende apenas da forma ou apenas da intensidade, mas da intersecção espacial de ambas as camadas detectadas:
Regra de Validação: Um núcleo só é confirmado se um centro de "Glow" for detectado dentro do raio de uma "Borda" (calculado via Distância Euclidiana entre centros).
Esta validação cruzada elimina:
Detritos brilhantes: Que possuem brilho mas não têm o corpo celular cinza ao redor.
Sombras/Ruído de fundo: Que possuem forma circular mas não apresentam o "vital" brilho central.
5. Conclusão
O algoritmo demonstrou ser uma alternativa robusta e leve ao Deep Learning. Ao utilizar a geometria como critério de decisão, o sistema consegue separar células encostadas, cumprindo os requisitos acadêmicos de Processamento Digital de Imagens com alta eficácia e transparência matemática.
