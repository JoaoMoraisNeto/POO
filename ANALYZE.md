# 1. Contexto

A empresa está perdendo clientes e quer entender quais características e segmentos estão mais associados ao cancelamento (churn).
Para isso, foi analisada a base Telco Customer Churn, consolidada na tabela telco_analytics, com 7043 clientes e 24 variáveis.

# 2. Perguntas e KPIs

### Perguntas do negócio:

Quais perfis têm maior churn?

Quais segmentos devemos priorizar para reduzir o churn?

Quais fatores dos clientes mais estão ligados ao churn?

Quanto o valor cobrado aos clientes impacta nos cancelamentos?

### KPIs

Churn rate (geral): proporção de clientes que cancelaram (churn=1).

Churn por segmento: churn rate calculado por categoria (ex.: tipo de contrato).

Número de churn por segmento: contagem de cancelamentos (soma de churn).

# 3. Metodologia

### A análise foi conduzida em duas etapas:

Segmentação univariada: cálculo de n_total, n_churn e churn_rate por variável (ex.: contract, paymentmethod, internetservice, etc.).

Segmentação bivariada (cruzamentos): análise de combinações entre variáveis para identificar perfis específicos com churn alto e volume relevante.

Critério de priorização: segmentos e combinações com churn_rate acima da média e alto volume de churn (n_churn).

# 4. Resultados
## 4.1 KPI 1 — Churn rate geral

O churn rate geral foi de aproximadamente 26,5% (base: 7043 clientes).
Isso indica que cerca de 1 em cada 4 clientes cancela o serviço, sugerindo um problema relevante de retenção.

## 4.2 KPI 2 e 3 — Churn por segmento + volume

### Contrato (contract)

Month-to-month: churn rate 42,7% (n_churn 1655)

One year: churn rate 11,3% (n_churn 166)

Two year: churn rate 2,8% (n_churn 48)


Interpretação: clientes em contratos mensais apresentam risco muito maior e concentram grande parte do churn.



### Tempo de casa (tenure_bucket)

0–6 meses: churn rate 52,9% (n_churn 784)

7–12 meses: churn rate 35,9% (n_churn 253)

13–24 meses: churn rate 28,7% (n_churn 294)

25+ meses: churn rate 14,0% (n_churn 538)


Interpretação: o churn é fortemente concentrado no início do relacionamento, sobretudo nos primeiros 6 meses.



### Método de pagamento (paymentmethod)

Electronic check: churn rate 45,3% (n_churn 1071)

Outros métodos: churn rate entre 15% e 19%


Interpretação: “Electronic check” aparece como um marcador de risco importante, tanto em taxa quanto em volume.



### Tipo de internet (internetservice)

Fiber optic: churn rate 41,9% (n_churn 1297)

DSL: churn rate 19,0% (n_churn 459)

No internet: churn rate 7,4% (n_churn 113)


Interpretação: clientes de fibra têm churn bem acima da média e concentram o maior volume de cancelamentos.



### Faixa de cobrança mensal (monthlycharges_bucket)

High: churn rate 35,5% (n_churn 1274)

Mid: churn rate 23,6% (n_churn 407)

Low: churn rate 10,9% (n_churn 188)


Interpretação: valores mensais mais altos estão associados a maior churn, mas precisam ser interpretados junto com contrato e tipo de internet.



### Segurança online (onlinesecurity)

No: churn rate 41,8% (n_churn 1461)

Yes: churn rate 14,6% (n_churn 295)

No internet service: churn rate 7,4%


Interpretação: ausência de segurança online está associada a churn muito maior.


## 4.3 Cruzamentos — perfis específicos de maior risco

### Cruzamento 1: internetservice × tenure_bucket
As combinações mais críticas foram:

Fiber optic + 0–6 meses: churn rate 74,2% (n_churn 460, n_total 620)

Fiber optic + 7–12 meses: churn rate 61,1% (n_churn 185)

Fiber optic + 13–24 meses: churn rate 48,8% (n_churn 219)

DSL + 0–6 meses: churn rate 48,2% (n_churn 246)


Interpretação: o risco máximo está concentrado em clientes novos com fibra óptica — perfil claro para ações de retenção.



### Cruzamento 2: contract × monthlycharges_bucket
As combinações mais críticas foram:

Month-to-month + high: churn rate 52,8% (n_churn 1111, n_total 2104)

Month-to-month + mid: churn rate 35,0% (n_churn 373, n_total 1067)

Month-to-month + low: churn rate 24,3% (n_churn 171, n_total 704)


Em contratos mais longos, mesmo com cobrança alta, o churn é muito menor:


One year + high: churn rate 17,5% (n_churn 126, n_total 718)

Two year + high: churn rate 4,8% (n_churn 37, n_total 769)


➡️ Interpretação: a faixa de cobrança mensal tem relação com churn, mas o tipo de contrato é um “fator moderador” forte. O risco de churn se concentra principalmente em clientes Month-to-month, especialmente na faixa high.



### Cruzamento 3: tenure_bucket × onlinesecurity
As combinações mais críticas foram:

0–6 meses + sem online security: churn rate 66,2% (n_churn 651, n_total 984)

7–12 meses + sem online security: churn rate 48,3% (n_churn 202)

13–24 meses + sem online security: churn rate 41,0% (n_churn 227)


Interpretação: entre clientes novos, não ter segurança online está associado a churn muito elevado.



# 5. Respostas às perguntas do negócio

### 1) Quais perfis têm maior churn?
Clientes com tenure baixo (0–6 meses), especialmente com Fiber optic, e clientes sem Online Security, apresentam as maiores taxas de churn.


### 2) Quais segmentos devemos priorizar?
Segmentos com alto churn rate e alto volume:

Month-to-month
Fiber optic
Electronic check

High monthly charges (principalmente quando combinado com contrato/serviço)


### 3) Quais fatores mais ligados ao churn?
Contrato, tempo de casa, tipo de internet, método de pagamento e ausência de segurança online.


### 4) Quanto o valor cobrado impacta?
Há evidência de aumento de churn em faixas de cobrança mais altas, mas a leitura final deve considerar cruzamento com contrato e tipo de serviço.