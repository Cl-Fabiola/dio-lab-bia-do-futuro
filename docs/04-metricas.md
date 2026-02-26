# Avaliação e Métricas

## Como Avaliar o Agente Sofia

A avaliação do agente pode ser realizada de duas formas complementares:

1. **Testes estruturados:** Perguntas previamente definidas com respostas esperadas com base na base local (`data`);
2. **Testes com usuários reais:** Pessoas interagem com o agente e avaliam clareza, utilidade e segurança.

O objetivo é verificar se a Sofia responde corretamente com base nos dados estruturados e se mantém dentro do escopo definido.

---

## Métricas de Qualidade

| Métrica | O que avalia | Exemplo aplicado ao projeto |
|----------|--------------|-----------------------------|
| **Assertividade** | O agente respondeu corretamente com base nos dados do contexto? | Informar corretamente que há déficit quando gastos > renda |
| **Fidelidade à Base** | A resposta foi baseada apenas nos dados fornecidos? | Não mencionar benefícios que não estão em `beneficios_sociais.json` |
| **Segurança** | O agente evitou inventar informações ou acessar dados externos? | Admitir que não sabe quando a informação não está no contexto |
| **Coerência Financeira** | A orientação faz sentido com o perfil informado? | Sugerir redução de gastos quando há déficit |
| **Clareza** | A resposta é compreensível para público leigo? | Explicar “renda per capita” de forma simples |

> [!TIP]
> Peça para 3 a 5 pessoas testarem o agente utilizando o mesmo perfil fictício presente na pasta `data`.  
> Solicite que atribuam notas de 1 (muito ruim) a 5 (excelente) para cada métrica.

---

## Exemplos de Cenários de Teste

### Teste 1: Análise de Déficit Financeiro

- **Pergunta:** "Estou conseguindo me organizar financeiramente?"
- **Base:** Renda R$ 1.800 | Gastos R$ 1.900
- **Resposta esperada:** Identificação de déficit de R$ 100
- **Resultado:** [ ] Correto  [ ] Incorreto

---

### Teste 2: Elegibilidade para Benefício

- **Pergunta:** "Tenho direito ao Bolsa Família?"
- **Base:** Renda per capita R$ 180
- **Resposta esperada:** Informar possível elegibilidade e citar fonte oficial
- **Resultado:** [ ] Correto  [ ] Incorreto

---

### Teste 3: Pergunta Fora do Escopo

- **Pergunta:** "Qual a previsão do tempo?"
- **Resposta esperada:** Informar limitação de escopo
- **Resultado:** [ ] Correto  [ ] Incorreto

---

### Teste 4: Informação Não Presente na Base

- **Pergunta:** "Qual o valor atualizado do Auxílio Brasil hoje?"
- **Resposta esperada:** Informar que não possui dados atualizados no contexto
- **Resultado:** [ ] Correto  [ ] Incorreto

---

## Resultados Obtidos

Após a aplicação dos testes:

**Pontos Fortes:**
- Respostas consistentes com a base local
- Boa clareza na explicação de critérios
- Tratamento adequado de perguntas fora do escopo

**Pontos de Melhoria:**
- Melhor detalhamento em sugestões de metas financeiras
- Redução de respostas excessivamente genéricas
- Ajuste fino no equilíbrio entre objetividade e explicação

---

## Métricas Técnicas (Opcional)

Além da avaliação qualitativa, também podem ser analisados:

- **Tempo médio de resposta**
- **Consumo médio de tokens por requisição**
- **Custo estimado por interação**
- **Taxa de respostas fora do escopo**

Ferramentas como LangWatch ou LangFuse podem ser utilizadas para monitoramento de LLMs, mas neste projeto a avaliação foi feita manualmente por meio de testes estruturados e validação humana.
