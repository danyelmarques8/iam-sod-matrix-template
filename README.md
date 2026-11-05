# Matriz de SoD — Funcional e técnica (modelo genérico)

Modelo de matriz de Segregação de Funções (SoD) em planilha, com **dados, escalas e códigos fictícios**. Tem duas camadas: a **funcional** (quais funções não podem ficar com a mesma pessoa e com que risco) e a **técnica** (em qual sistema, perfil e transação cada função acontece). Inclui uma análise automática de usuários.

> **Aviso:** tudo aqui é fictício e didático. Em uma empresa real, as regras e as pontuações saem da avaliação de risco e da política de acessos.

> **Visualização rápida:** abra o [`SoD_Matrix_Template.pdf`](SoD_Matrix_Template.pdf) para ver todas as abas sem precisar do Excel. A planilha completa, com as fórmulas, está em [`SoD_Matrix_Template.xlsx`](SoD_Matrix_Template.xlsx).

## Matriz funcional

17 funções em 4 processos (compras e pagamentos, folha, contabilidade e TI) e 28 regras.

**Como ler:** cada linha é uma função e cada coluna é outra função. As colunas seguem a mesma ordem das linhas, de F01 a F17. Onde as duas se cruzam, a cor mostra o conflito de a mesma pessoa ter as duas funções. Célula vazia significa que não há regra, e o ponto (·) é a função cruzada com ela mesma. A tabela é espelhada, então cada par aparece duas vezes.

| ↓ Linha · Coluna → |F01|F02|F03|F04|F05|F06|F07|F08|F09|F10|F11|F12|F13|F14|F15|F16|F17|
|---|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| **F01** Cadastrar fornecedor |·|🟧|||🟧|🟥|🟥|||||||||||
| **F02** Criar pedido de compra |🟧|·|🟥|🟧||||||||||||||
| **F03** Aprovar pedido de compra ||🟥|·||🟨|||||||||||||
| **F04** Registrar recebimento ||🟧||·|🟨|||||||||||||
| **F05** Lançar fatura |🟧||🟨|🟨|·|🟥|🟧||||||🟧|||||
| **F06** Aprovar pagamento |🟥||||🟥|·|🟥||||||🟧||||🟧|
| **F07** Executar pagamento |🟥||||🟧|🟥|·|🟧|||🟧|||||🟧||
| **F08** Cadastrar/alterar colaborador |||||||🟧|·|🟧|🟧||||||||
| **F09** Processar folha ||||||||🟧|·|🟥||||||||
| **F10** Aprovar folha ||||||||🟧|🟥|·|||||||🟧|
| **F11** Lançar contábil manual |||||||🟧||||·|🟥||||🟧||
| **F12** Aprovar lançamento contábil |||||||||||🟥|·||||||
| **F13** Administrar ERP |||||🟧|🟧|||||||·|🟧|||🟧|
| **F14** Desenvolver código |||||||||||||🟧|·|🟥|🟧||
| **F15** Promover para produção ||||||||||||||🟥|·|🟨||
| **F16** Administrar banco de dados |||||||🟧||||🟧|||🟧|🟨|·||
| **F17** Gerenciar acessos ||||||🟧||||🟧|||🟧||||·|

🟥 **Crítico** (16 a 25): proibido, bloquear na solicitação · 🟧 **Alto** (10 a 15): exceção aprovada com controle compensatório · 🟨 **Médio** (5 a 9): permitido com controle documentado

**Exemplo:** na linha **F06 Aprovar pagamento**, a coluna **F07** está 🟥. Quem aprova e também executa o mesmo pagamento controla o processo inteiro, então a combinação é proibida.

### Funções

| ID | Função | Processo |
|---|---|---|
| F01 | Cadastrar fornecedor | Compras e pagamentos |
| F02 | Criar pedido de compra | Compras e pagamentos |
| F03 | Aprovar pedido de compra | Compras e pagamentos |
| F04 | Registrar recebimento | Compras e pagamentos |
| F05 | Lançar fatura | Compras e pagamentos |
| F06 | Aprovar pagamento | Compras e pagamentos |
| F07 | Executar pagamento | Compras e pagamentos |
| F08 | Cadastrar/alterar colaborador | Folha de pagamento |
| F09 | Processar folha | Folha de pagamento |
| F10 | Aprovar folha | Folha de pagamento |
| F11 | Lançar contábil manual | Contabilidade |
| F12 | Aprovar lançamento contábil | Contabilidade |
| F13 | Administrar ERP | TI e segurança |
| F14 | Desenvolver código | TI e segurança |
| F15 | Promover para produção | TI e segurança |
| F16 | Administrar banco de dados | TI e segurança |
| F17 | Gerenciar acessos | TI e segurança |

## Risco = probabilidade × impacto

Cada regra recebe probabilidade (1 a 5) e impacto (1 a 5). A pontuação (1 a 25) define a severidade automaticamente: 16 ou mais é crítico, 10 a 15 é alto, 5 a 9 é médio e abaixo de 5 não gera restrição.

## Camada técnica

A aba **Mapa técnico** liga cada função a um sistema, um perfil e uma transação. A aba **Matriz técnica** cruza isso com as regras e mostra, para cada conflito, os dois lados e se ele acontece dentro de um sistema ou entre sistemas. Exemplos:

| Regra | Pontuação | Lado A | Lado B | Escopo |
|---|---|---|---|---|
| R11 | 20 (crítico) | Aprovar pagamento (ERP, PRF-FIN-APR-PAG, TX-PAG-02) | Executar pagamento (ERP, PRF-FIN-EXEC-PAG, TX-PAG-03) | Mesmo sistema |
| R28 | 10 (alto) | Aprovar folha (RH, PRF-RH-FOLHA-APR, TX-FOL-02) | Gerenciar acessos (IGA, PRF-IAM-ADMIN, TX-ACE-01) | Entre sistemas |

Os conflitos entre sistemas são os mais fáceis de passar despercebidos, porque nenhum sistema sozinho enxerga a combinação.

## Resultado com os dados de exemplo

A aba **Análise** cruza as funções de 11 usuários fictícios com a matriz:

| Resultado | Usuários |
|---|---|
| Violação crítica | 3 (USR-004, USR-007, USR-010) |
| Violação alta | 4 (USR-003, USR-005, USR-006, USR-008) |
| Permitido com controle compensatório | 1 (USR-011) |
| Sem conflito | 3 (USR-001, USR-002, USR-009) |

## Como usar

1. Baixe a planilha [`SoD_Matrix_Template.xlsx`](SoD_Matrix_Template.xlsx) e leia a aba **Instruções**.
2. Na aba **Funções**, ajuste as funções ao seu processo. Na aba **Mapa técnico**, informe sistema, perfil e transação de cada uma.
3. Na aba **Regras**, escreva um par de funções por linha, com probabilidade e impacto. As abas **Matriz** e **Matriz técnica** se atualizam sozinhas.
4. Na aba **Atribuições**, marque 1 onde o usuário tem a função e 0 onde não tem.
5. Na aba **Análise**, veja quantos conflitos cada usuário reúne e a ação indicada.

Células com fonte azul sobre fundo amarelo são de entrada. As demais são fórmulas.

## Limites

Cobre pares de funções, não combinações de três ou mais. Suporta 17 funções e 56 regras. Não substitui a avaliação de risco nem a validação com a auditoria.

## Como fiz

Montei este modelo a partir de conceitos gerais de segregação de funções (matriz funcional, camada técnica e risco como probabilidade × impacto), com dados, escalas e códigos fictícios. Usei IA como apoio para organizar o material e revisei os conflitos e as pontuações por conta própria.
