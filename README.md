
---

<p align="justify"><h1>Detecção de Anomalias via Ensemble Robusto</h1></p>

<p align="justify">Este texto apresenta a análise comparativa entre o modelo baseline <b>Isolation Forest (IF)</b> e a arquitetura de <b>Ensemble Robusto</b>. Uma distinção fundamental entre as abordagens reside na natureza do aprendizado: o IF atua de forma independente de rótulos, enquanto o Ensemble utiliza o conhecimento prévio dos estados de falha para maximizar a assertividade.</p>

### 1. Paradigmas de Aprendizado

<p align="justify">A comparação entre os modelos revela a eficiência de diferentes estratégias de Machine Learning:</p>

* <p align="justify"><b>Isolation Forest (Não Supervisionado):</b> Este algoritmo não requer dados rotulados. Ele "descobre" as anomalias por conta própria, baseando-se no princípio de que anomalias são poucas e diferentes (possuem caminhos de isolamento mais curtos em árvores aleatórias). É a solução ideal para cenários onde não se conhece o padrão da falha antecipadamente.</p>
* <p align="justify"><b>Ensemble (Supervisionado):</b> O <b>Balanced Random Forest</b> integrado ao sistema exige um conjunto de treino rotulado. Ele é "ensinado" a distinguir as médias sutis das anomalias, permitindo que o modelo aprenda exatamente onde a operação normal termina e a falha incipiente começa. Isso justifica sua performance superior, especialmente em cenários ruidosos.</p>

### 2. O Desafio Técnico: O Problema da Proximidade

<p align="justify">Diferente de outliers convencionais, as anomalias aqui geradas são submetas. As médias dos sensores para o estado de falha são deliberadamente próximas às médias do estado estável (ex: 50 vs 58 na Temperatura). Como as distribuições possuem <b>Overlap</b> significativo, o aprendizado supervisionado torna-se um diferencial competitivo ao fornecer ao modelo a "assinatura" exata da falha que o modelo não supervisionado tenta isolar às cegas.</p>

---

### 📊 Comparativo de Desempenho: IF vs. Ensemble Robusto

| Ruído (%) | F1 (IF) | F1 (Ensemble) | Recall (Ensemble) | Precision (Ensemble) |
| --- | --- | --- | --- | --- |
| **0%** | 0.384615 | **0.545455** | 0.800000 | 0.413793 |
| **5%** | 0.518519 | **0.565217** | 0.866667 | 0.419355 |
| **10%** | 0.444444 | **0.636364** | **0.933333** | 0.482759 |

---

### 3. Análise do Custo da Precisão e Ruído

<p align="justify">No cenário de 10% de ruído, o Ensemble atingiu <b>93,3% de Recall</b>, detectando 14 das 15 anomalias. O aumento do desempenho com o ruído deve-se à quebra da densidade excessiva na zona de sobreposição, onde o ruído gaussiano atuou como um agente de dispersão, facilitando a classificação supervisionada de padrões que, em estado puro, estavam camuflados.</p>
<p align="justify"><b>Falsos Positivos (Alarmes Falsos):</b> Com uma precisão de 0.4827, o modelo gerou aproximadamente 29 alertas totais para detectar as 14 falhas. Isso resultou em <b>15 falsos positivos</b> (leituras normais confundidas com falha devido ao ruído extremo).</p>

<p align="justify"><b>Justificativa Técnica:</b> Em contextos de manutenção preditiva, o custo de investigar 15 alarmes falsos é significativamente menor do que o custo de uma falha catastrófica não detectada. O Ensemble garante que <b>9 em cada 10 falhas</b> sejam interrompidas antes de ocorrerem.</p>

<p align="justify"><h3>Conclusão</h3></p>

<p align="justify">O experimento valida que, embora o <b>Isolation Forest</b> seja uma ferramenta poderosa pela sua autonomia (não supervisionado), a inclusão de um componente <b>supervisionado</b> via Ensemble é o que garante a confiabilidade necessária para sistemas de missão crítica , onde a precisão na detecção de falhas sutis é vital para evitar paradas não planejadas.</p>

---

