
---

<p align="justify"><h1>Detecção de Anomalias em Ambientes de Alta Sobreposição</h1></p>

<p align="justify">Este projeto aborda um dos cenários mais complexos na ciência de dados industrial: a <b>identificação de desvios sutis</b> em sistemas de sensores. O desafio central reside na natureza dos dados, onde as distribuições de falha e de operação normal compartilham o mesmo espaço estatístico, tornando as anomalias "invisíveis" para métodos de limiar simples.</p>

<p align="justify"><h3>1. O Desafio Técnico: O Problema da Proximidade</h3></p>

<p align="justify">Diferente de <i>outliers</i> convencionais (pontos extremos que fogem completamente do padrão), as anomalias aqui geradas são <b>submetas</b>. As médias dos sensores para o estado de falha são deliberadamente próximas às médias do estado estável, o que cria um desafio de separabilidade para o modelo.</p>

<p align="justify">Como as distribuições possuem <b>interseção significativa (Overlap)</b>, uma leitura isolada de sensor pode ser perfeitamente normal mesmo pertencendo a uma classe de anomalia. O modelo deve, portanto, aprender a correlação multidimensional entre os sensores para distinguir os padrões.</p>

<p align="justify"><h3>2. Metodologia e Modelagem</h3></p>

<p align="justify">O experimento utiliza o algoritmo <b>Isolation Forest</b>, que baseia sua lógica no isolamento de observações através de partições aleatórias. Para testar a resiliência do sistema, aplicamos ruído gaussiano proporcional ao desvio padrão ($\sigma$) de cada sensor:</p>

<p align="justify">

$$X_{noisy} = X + \eta, \quad \text{onde } \eta \sim \mathcal{N}(0, \sigma \cdot \text{level} \cdot 2.5)$$

</p>

<p align="justify">Essa abordagem simula a degradação de sensores reais e interferências eletromagnéticas em ambientes industriais, dificultando o trabalho do algoritmo de isolar os pontos anômalos conforme o nível de ruído aumenta.</p>

<p align="justify"><h3>3. Análise de Sensibilidade e Resultados</h3></p>

<p align="justify">Os resultados demonstram a correlação direta entre o ruído ambiental e a queda drástica de performance, medida pelo <b>F1-Score</b>. A tabela abaixo sintetiza o comportamento do modelo frente aos diferentes cenários de incerteza:</p>

| Cenário | Nível de Ruído | Precision | Recall | **F1-Score** |
| --- | --- | --- | --- | --- |
| **Ideal** | 0% | 0.85 | 0.80 | **0.82** |
| **Intermediário** | 5% | 0.66 | 0.57 | **0.61** |
| **Crítico** | 10% | 0.33 | 0.37 | **0.35** |

<p align="justify"><h3>4. Justificativa dos Resultados</h3></p>

* <p align="justify"><b>Baixo Ruído:</b> O Isolation Forest consegue identificar as anomalias por meio da combinação de variáveis, mesmo com médias próximas, explorando a estrutura densa do dado normal.</p>
* <p align="justify"><b>Alto Ruído:</b> O ruído "preenche" as lacunas entre as distribuições. Isso faz com que anomalias pareçam normais (aumento de Falsos Negativos) e dados normais pareçam anômalos devido à dispersão (aumento de Falsos Positivos).</p>

<hr>

<p align="justify"><h3>Conclusão</h3></p>

<p align="justify">O experimento evidencia que a detecção de anomalias em sistemas reais é, acima de tudo, um desafio de <b>relação sinal-ruído</b>. Quando as distribuições se fundem, a métrica de contaminação do modelo torna-se extremamente sensível, exigindo técnicas avançadas de pré-processamento ou modelos mais robustos (como Autoencoders) para manter a confiabilidade operacional.</p>

---

