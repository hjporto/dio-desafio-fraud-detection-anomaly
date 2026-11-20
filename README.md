# Detecção de Fraudes em Cartão de Crédito 💳🔍

Projeto prático desenvolvido para identificar transações fraudulentas em cartões de crédito utilizando dados reais, aprendizado de máquina e técnicas para explicar as decisões do modelo.

---

## 🎯 O Desafio: Por que Acurácia Engana?

Em operações com cartão de crédito, a grande maioria esmagadora das compras é legítima:
* **~99,8%** são transações normais.
* **Apenas ~0,17%** são fraudes reais.

Se criássemos um sistema "preguiçoso" que simplesmente respondesse *"não é fraude"* para tudo, ele acertaria quase **100% das vezes**. No entanto, ele deixaria passar **todas as fraudes**, causando prejuízos imensos.

Por isso, o foco deste projeto não é a acurácia geral, mas sim olhar direto para a **classe de fraude**:
* **Recall (Sensibilidade):** Quantas das fraudes reais o sistema conseguiu pegar? *(Nosso foco principal: não deixar o golpista passar)*.
* **Precisão:** Das transações que o modelo apontou como fraude, quantas eram fraude de verdade? *(Importante para não bloquear clientes honestos à toa)*.
* **F1-Score:** O equilíbrio entre pegar o máximo de fraudes e não incomodar clientes legítimos.

---

## 🧹 O que foi feito com os Dados?

1. **Leitura Direta:** A base de dados é carregada direto da nuvem via código, sem precisar baixar arquivos pesados no computador.
2. **Ajuste nos Valores:** O valor das compras varia muito (desde centavos até milhares de reais). Ajustamos a escala desses valores para que o modelo consiga aprender sem distorções causadas por compras muito altas.
3. **Divisão Cuidadosa de Treino e Teste:** Garantimos que a proporção minúscula de fraudes (0,17%) estivesse presente de forma idêntica tanto nos dados que o modelo usou para estudar quanto nos dados que usamos para testá-lo.

---

## 🤖 Modelos Testados e Resultados

Comparamos diferentes abordagens para ver qual se saía melhor no mundo real:

1. **Regressão Logística (Modelo Base):** Nosso ponto de partida. Simples e rápido, mas deixa passar uma quantidade considerável de fraudes.
2. **Regressão Logística com Ajuste de Limiar:** Mudamos o "gatilho" de decisão de 50% para 30%. O modelo ficou mais atento a sinais suspeitos, capturando bem mais fraudes.
3. **Random Forest:** Um conjunto de várias árvores de decisão configuradas para dar peso extra às fraudes. Teve ótimo equilíbrio entre capturar fraudes e evitar alarmes falsos.
4. **XGBoost:** O algoritmo mais avançado testado. Foi o que melhor identificou padrões sutis de fraude.

### Resumo do Desempenho (Classe Fraude)

* **Regressão Logística (Padrão):** Bom ponto de partida, mas deixa passar cerca de 38% das fraudes.
* **Regressão Logística (Limiar 0.3):** Reduz bastante as fraudes perdidas ao diminuir a exigência para soar o alarme.
* **Random Forest e XGBoost:** Tiveram os melhores resultados práticos, alcançando cerca de **80% a 84% de recall**, mantendo uma precisão alta (poucos clientes normais bloqueados).

---

## 🎛️ O Limiar de Decisão Escolhido

Por padrão, modelos de classificação só dizem "é fraude" se tiverem mais de 50% de certeza. 

No mercado financeiro, deixar uma fraude acontecer costuma custar muito mais caro do que analisar uma compra suspeita que acabou sendo legítima. Por isso, **reduzimos esse limiar para 0.3 (30%)**. Isso faz o modelo ser mais prudente, aumentando o número de fraudes detectadas.

---

## 🧠 Explicando as Decisões (SHAP)

Não basta o modelo dizer se uma transação é fraude ou não; o time de segurança precisa entender **o porquê**.

Utilizamos a biblioteca **SHAP**, que funciona como uma lupa sobre o modelo:
* Ela mostra exatamente quais características da compra pesaram para ela ser considerada suspeita.
* Mesmo com a maioria das variáveis anonimizadas por privacidade, foi possível identificar quais padrões comportamentais tiveram maior impacto em acionar o alarme de fraude.

---

## ✨ O que foi adicionado além do básico?

* **Exploração de Balanceamento:** Testamos tanto reduzir exemplos normais (*undersampling*) quanto gerar exemplos sintéticos de fraude (*SMOTE*).
* **Pipeline Integrado:** Estrutura organizada que junta preparação de dados e modelo em uma única etapa, evitando falhas comuns.
* **Busca Automática de Hiperparâmetros (GridSearchCV):** Ajuste fino do modelo XGBoost orientado especificamente a maximizar o *Recall*.
* **Interpretabilidade Visual:** Gráficos intuitivos para explicar aos times de negócio o que levou o modelo a cada conclusão.

---

