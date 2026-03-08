
---

<p align="justify"><h1>Relatório Técnico: Detecção de Anomalias via Ensemble Robusto</h1></p>

<p align="justify">Este documento apresenta a análise comparativa entre o modelo baseline <b>Isolation Forest (IF)</b> e a arquitetura proposta de <b>Ensemble Robusto</b>, que combina o IF com o <b>Balanced Random Forest (BRF)</b>. O foco central é a superação da degradação de performance em cenários de alta complexidade estatística e monitoramento de ativos críticos.</p>

### 1. O Desafio Técnico: O Problema da Proximidade

<p align="justify">Diferente de outliers convencionais, as anomalias aqui geradas são submetas. As médias dos sensores para o estado de falha são deliberadamente próximas às médias do estado estável. Como as distribuições possuem interseção significativa (<b>Overlap</b>), uma leitura isolada de sensor pode ser perfeitamente normal mesmo pertencendo a uma classe de anomalia. Essa característica exige que o modelo aprenda fronteiras de decisão multidimensionais extremamente finas.</p>

### 2. Configuração do Sistema: Sensores Monitorados

<p align="justify">O experimento baseia-se em um conjunto de <b>6 sensores industriais</b> com diferentes comportamentos estatísticos:</p>

* <p align="justify"><b>Temperatura (°C):</b> Normal (50 vs 58).</p>
* <p align="justify"><b>Pressão (bar):</b> Normal com alta variância (50 vs 65).</p>
* <p align="justify"><b>Vibração (mm/s):</b> Exponencial (10 vs 18).</p>
* <p align="justify"><b>Corrente (A):</b> Consumo elétrico (10 vs 13).</p>
* <p align="justify"><b>Tensão (V):</b> Estabilidade da rede (220 vs 230).</p>
* <p align="justify"><b>Velocidade (RPM):</b> Poisson (50 vs 60).</p>

### 3. Metodologia e Modelagem de Ruído

<p align="justify">Aplicamos ruído gaussiano proporcional ao desvio padrão ($\sigma$) de cada sensor para simular interferências industriais:</p>

<p align="center">
$X_{noisy} = X + \eta$, onde $\eta \sim N(0, \sigma \cdot level \cdot 2.5)$
</p>

---

### 📊 Comparativo de Desempenho: IF vs. Ensemble Robusto

| Ruído (%) | F1 (IF) | F1 (Ensemble) | Recall (Ensemble) | Precision (Ensemble) |
| --- | --- | --- | --- | --- |
| **0%** | 0.384615 | **0.545455** | 0.800000 | 0.413793 |
| **5%** | 0.518519 | **0.565217** | 0.866667 | 0.419355 |
| **10%** | 0.444444 | **0.636364** | **0.933333** | 0.482759 |

---

### 4. Análise de Deteção e o Custo da Precisão

<p align="justify">No cenário de maior estresse (10% de ruído), o Ensemble Robusto priorizou a <b>Segurança Operacional</b> (Recall), o que acarreta um custo em alarmes falsos, detalhado abaixo:</p>

* <p align="justify"><b>Volume de Deteção:</b> O modelo identificou <b>14 das 15 anomalias</b> reais presentes no teste (93,3% de eficácia).</p>
* <p align="justify"><b>Falsos Positivos (Alarmes Falsos):</b> Com uma precisão de 0.4827, o modelo gerou aproximadamente 29 alertas totais para detectar as 14 falhas. Isso resultou em <b>15 falsos positivos</b> (leituras normais confundidas com falha devido ao ruído extremo).</p>
* <p align="justify"><b>Justificativa Técnica:</b> Em contextos de manutenção preditiva, o custo de investigar 15 alarmes falsos é significativamente menor do que o custo de uma falha catastrófica não detectada. O Ensemble garante que <b>9 em cada 10 falhas</b> sejam interrompidas antes de ocorrerem.</p>

### 5. Conclusão

<p align="justify">A transição para modelos de ensemble balanceados prova-se essencial quando lidamos com anomalias de baixa magnitude. A sinergia entre o isolamento geométrico do IF e a distinção estatística do BalancedRF permite manter a sensibilidade mesmo sob condições adversas de sinal, validando sua aplicação em sistemas de missão crítica.</p>

---

