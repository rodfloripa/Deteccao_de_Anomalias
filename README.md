
---

<p align="justify"><h1>Detecção de Anomalias em Ambientes de Alta Sobreposição</h1></p>

<p align="justify">Este projeto aborda um dos cenários mais complexos na ciência de dados industrial: a <b>identificação de desvios sutis</b> em sistemas de sensores. O desafio central reside na natureza dos dados, onde as distribuições de falha e de operação normal compartilham o mesmo espaço estatístico, tornando as anomalias "invisíveis" para métodos de limiar simples.</p>

<p align="justify"><h3>1. O Desafio Técnico: O Problema da Proximidade</h3></p>

<p align="justify">Diferente de <i>outliers</i> convencionais, as anomalias aqui geradas são <b>submetas</b>. As médias dos sensores para o estado de falha são deliberadamente próximas às médias do estado estável. Como as distribuições possuem <b>interseção significativa (Overlap)</b>, uma leitura isolada de sensor pode ser perfeitamente normal mesmo pertencendo a uma classe de anomalia.</p>

<p align="justify"><h3>2. Metodologia e Modelagem</h3></p>

<p align="justify">O experimento utiliza o algoritmo <b>Isolation Forest</b>, que baseia sua lógica no isolamento de observações através de partições aleatórias. Para testar a resiliência do sistema, aplicamos ruído gaussiano proporcional ao desvio padrão ($\sigma$) de cada sensor:</p>

<p align="justify">

$$X_{noisy} = X + \eta, \quad \text{onde } \eta \sim \mathcal{N}(0, \sigma \cdot \text{level} \cdot 2.5)$$

</p>

<p align="justify">Essa abordagem simula a degradação de sensores reais e interferências eletromagnéticas em ambientes industriais, dificultando o trabalho do algoritmo de isolar os pontos anômalos conforme o nível de ruído aumenta.</p>

<p align="justify"><h3>3. Análise de Sensibilidade e Resultados</h3></p>

<p align="justify">Os resultados obtidos demonstram a dificuldade do modelo em lidar com a sobreposição severa, onde mesmo sem ruído o F1-Score apresenta valores desafiadores devido à similaridade das médias:</p>

| Nível de Ruído | F1-Score | Precision | Recall |
| --- | --- | --- | --- |
| <p align="justify"><b>0%</b></p> | <p align="justify">0.444</p> | <p align="justify">0.500</p> | <p align="justify">0.400</p> |
| <p align="justify"><b>5%</b></p> | <p align="justify">0.333</p> | <p align="justify">0.444</p> | <p align="justify">0.266</p> |
| <p align="justify"><b>10%</b></p> | <p align="justify">0.347</p> | <p align="justify">0.500</p> | <p align="justify">0.266</p> |

<p align="justify"><h3>4. Justificativa dos Resultados</h3></p>

* <p align="justify"><b>Baixo Recall:</b> O baixo valor de Recall (0.400 no cenário base) indica que a proximidade das médias faz com que o Isolation Forest classifique muitas anomalias como pontos de alta densidade (normais).</p>
* <p align="justify"><b>Impacto do Ruído:</b> A introdução de apenas 5% de ruído causou uma queda acentuada no F1-Score, estabilizando em patamares baixos em 10%, o que prova que a incerteza estatística "soterra" o sinal da anomalia.</p>

<hr>

<p align="justify"><h3>Conclusão</h3></p>

<p align="justify">O experimento evidencia que a detecção de anomalias em sistemas reais é, acima de tudo, um desafio de <b>relação sinal-ruído</b>. Para este caso específico de alta sobreposição, estratégias de <i>Ensemble</i> mais robustas ou o uso de <b>DPO (Direct Preference Optimization)</b>  poderiam ser adaptadas para "ensinar" ao modelo a distinção fina entre esses estados.</p>

---

