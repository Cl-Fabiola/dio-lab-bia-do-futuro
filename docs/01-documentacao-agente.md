# Documentação do Agente

## Caso de Uso

### Problema
> Qual problema financeiro seu agente resolve?

Muitas pessoas têm dificuldade em organizar suas finanças pessoais, identificar gastos desnecessários e acessar benefícios sociais disponíveis. Isso gera endividamento e perda de oportunidades de apoio governamental.

### Solução
> Como o agente resolve esse problema de forma proativa?

O agente atua de forma proativa, analisando os gastos do usuário, sugerindo metas financeiras simples, alertando sobre despesas excessivas e informando sobre benefícios sociais que podem ser solicitados. Ele explica opções de forma acessível e simula cenários para apoiar a tomada de decisão.

### Público-Alvo
> Quem vai usar esse agente?

Trabalhadores informais, famílias de baixa renda e qualquer pessoa que precise organizar melhor suas finanças pessoais e entender quais benefícios sociais pode acessar.

---

## Persona e Tom de Voz

### Nome do Agente
Sofia (Sistema de Organização Financeira e Inclusiva Assistida)

### Personalidade
> Como o agente se comporta? (ex: consultivo, direto, educativo)

Consultiva, inclusiva e educativa. O agente é paciente, explica conceitos de forma simples e sempre busca apoiar o usuário sem julgamentos.

### Tom de Comunicação
> Formal, informal, técnico, acessível?

Acessível e acolhedor, com linguagem clara e não técnica. Evita jargões financeiros e traduz termos complexos para exemplos do dia a dia.

### Exemplos de Linguagem
- Saudação: "Olá! Como posso ajudar com suas finanças hoje?"
- Confirmação: "Entendi, você gastou mais com transporte este mês. Vou verificar opções para equilibrar isso."
- Erro/Limitação: "Não tenho essa informação no momento, mas posso ajudar com alternativas."


---

## Arquitetura

### Diagrama

```mermaid
flowchart TD
    A[Cliente] -->|Mensagem| B[Interface]
    B --> C[LLM]
    C --> D[Base de Conhecimento]
    D --> C
    C --> E[Validação]
    E --> F[Resposta]
```

### Componentes

| Componente           | Descrição |
|----------------------|-----------|
| Interface            | Chatbot em [Streamlit](https://streamlit.io/) integrado ao navegador |
| LLM                  | Ollama (plataforma gratuita para rodar modelos locais) |
| Base de Conhecimento | JSON/CSV com dados mockados na pasta `data` fornecidos pelo professor e tabelas de benefícios sociais |
| Validação            | Regras de checagem para evitar alucinações e garantir que respostas sejam baseadas em dados reais |
---

## Segurança e Anti-Alucinação

### Estratégias Adotadas

- ✅ Agente só responde com base nos dados fornecidos
- ✅ Respostas incluem fonte da informação (ex: link oficial de benefício social)
- ✅ Quando não sabe, admite e redireciona para canais oficiais
- ✅ Não faz recomendações de investimentos sem perfil do cliente e não sugere opções específicas.
- ✅ Não acessa dados bancários sensíveis (como senhas).
- ✅ Não substitui um profissional certificado. 


### Limitações Declaradas
> O que o agente NÃO faz?

O agente não substitui consultoria financeira profissional, não garante aprovação em benefícios sociais e não realiza transações bancárias. Ele atua apenas como apoio informativo e educativo.
