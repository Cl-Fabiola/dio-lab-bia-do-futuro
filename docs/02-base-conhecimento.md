# Base de Conhecimento

## Dados Utilizados

A Sofia utiliza arquivos estruturados armazenados na pasta `data`, contendo informações simuladas e dados oficiais sobre benefícios sociais.

| Arquivo | Formato | Utilização no Agente |
|----------|----------|----------------------|
| `transacoes.csv` | CSV | Analisar padrão de gastos do usuário e identificar categorias com maior impacto financeiro |
| `perfil_usuario.json` | JSON | Armazenar renda, composição familiar e vínculo profissional para personalização das orientações |
| `beneficios_sociais.json` | JSON | Verificar critérios de elegibilidade e informar benefícios compatíveis com o perfil |
| `metas_financeiras.json` | JSON | Sugerir metas simples de economia com base na renda e nas despesas |

Todos os dados são simulados para fins educacionais, com exceção das informações públicas sobre benefícios sociais, que possuem fonte oficial indicada.

---

## Adaptações nos Dados

Os dados mockados foram adaptados para melhorar a capacidade de validação automática e reduzir ambiguidades na interpretação pelo modelo de linguagem.

As principais adaptações incluem:

- Conversão de critérios textuais em campos estruturados (ex: `renda_per_capita_max`, `idade_minima`)
- Inclusão de campos booleanos (ex: `vinculo_formal_obrigatorio`)
- Padronização de valores monetários no formato numérico
- Inclusão obrigatória do campo `fonte` com link oficial
- Separação por tipo de benefício (ex: Programa Social, Benefício Trabalhista)

Essa modelagem permite que o agente realize comparações lógicas antes de gerar a resposta, reduzindo o risco de alucinação e aumentando a confiabilidade das informações fornecidas.

---

## Estratégia de Integração

### Como os dados são carregados?

Os arquivos CSV e JSON são carregados no início da execução do sistema utilizando:

- `pandas` para leitura de arquivos CSV
- Biblioteca padrão `json` para leitura de arquivos JSON

Os dados permanecem armazenados em memória durante a sessão e são consultados dinamicamente a cada nova interação do usuário.

---

### Como os dados são usados no prompt?

A integração segue uma estratégia híbrida:

#### 1. System Prompt

No prompt de sistema são definidos:

- Persona da Sofia (consultiva, inclusiva e educativa)
- Restrições para uso exclusivo da base local
- Instrução explícita para não inventar informações
- Orientação para admitir quando não houver dados suficientes

#### 2️. Consulta Dinâmica

Quando o usuário faz uma pergunta:

1. O sistema identifica a intenção (ex: consulta de benefício ou análise de gastos)
2. Os dados relevantes são filtrados nos arquivos locais
3. É montado um contexto estruturado
4. Esse contexto é enviado junto com a pergunta ao modelo

O modelo não possui acesso livre aos dados — ele responde apenas com base nas informações estruturadas fornecidas no contexto.

---

## Exemplo de Contexto Montado

Abaixo está um exemplo real de como os dados estruturados são organizados antes de serem enviados ao modelo de linguagem.

### Exemplo 1 – Análise Financeira

```
Você é Sofia (Sistema de Organização Financeira e Inclusiva Assistida).
Responda apenas com base nas informações fornecidas abaixo.
Não invente dados que não estejam no contexto.

=== PERFIL DO USUÁRIO ===
Renda Mensal: R$ 1.800
Renda Per Capita: R$ 450
Dependentes: 2
Vínculo Formal: Não
Inscrito no CadÚnico: Sim

=== RESUMO DE GASTOS DO MÊS ===
Alimentação: R$ 750
Transporte: R$ 400
Moradia: R$ 500
Lazer: R$ 250
Total de Gastos: R$ 1.900

Tarefa:
- Analise a situação financeira.
- Identifique possíveis excessos.
- Sugira metas simples de economia.
- Use linguagem clara e acessível.
```

---

### Exemplo 2 – Verificação de Benefícios Sociais

```
Você é Sofia, agente de orientação financeira e inclusão social.
Utilize apenas os dados abaixo para responder.

=== PERFIL DO USUÁRIO ===
Renda Per Capita: R$ 180
Idade: 35 anos
Vínculo Formal: Não
Inscrito no CadÚnico: Sim

=== BENEFÍCIOS CADASTRADOS NA BASE ===

1. Bolsa Família
   - Renda per capita máxima: R$ 218
   - Tipo: Programa de Transferência de Renda
   - Público-alvo: Famílias em situação de pobreza inscritas no CadÚnico
   - Fonte: https://www.gov.br/mds/pt-br/assuntos/bolsa-familia

2. Benefício de Prestação Continuada (BPC)
   - Idade mínima: 65 anos ou pessoa com deficiência
   - Tipo: Assistência Social
   - Fonte: https://www.gov.br/mds/pt-br/assuntos/assistencia-social/bpc

Tarefa:
- Informe quais benefícios são compatíveis com o perfil.
- Explique os critérios de forma simples.
- Inclua a fonte oficial.
- Caso não haja compatibilidade, informe claramente.
```

---

### Estrutura Técnica Utilizada para Montar o Contexto (Exemplo em Python)

```python
def montar_contexto(perfil_usuario, resumo_gastos, beneficios):
    contexto = f"""
Você é Sofia (Sistema de Organização Financeira e Inclusiva Assistida).
Responda apenas com base nos dados fornecidos.
Não invente informações externas.

=== PERFIL DO USUÁRIO ===
{perfil_usuario}

=== RESUMO DE GASTOS ===
{resumo_gastos}

=== BENEFÍCIOS DISPONÍVEIS ===
{beneficios}

Responda de forma clara, acessível e educativa.
"""
    return contexto
```

---

Esses dados estruturados são enviados ao modelo apenas após validação lógica dos critérios, garantindo que a resposta seja fundamentada exclusivamente na base de conhecimento local.
