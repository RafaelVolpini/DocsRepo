## Requisitos funcionais

| ID | Título | Descrição | Prioridade | Critério de Aceite |
| :--- | :--- | :--- | :--- | :--- |
| **RF-001** | Gestão de projetos | O sistema deve permitir que Gerentes de Projetos criem, editem, visualizem e arquivem projetos com nome, descrição e período de execução. | Must Have | O projeto é salvo com dados válidos e exibido na listagem de projetos ativos ou arquivados. |
| **RF-002** | Personalização de colunas no Kanban | O sistema deve permitir criar, editar, reordenar e excluir colunas do quadro Kanban específicas para cada projeto. | Must Have | As alterações na ordem e nome das colunas são salvas e refletidas imediatamente no quadro do projeto. |
| **RF-003** | Agrupamento por Raias (Swimlanes) | O sistema deve permitir agrupar os cartões do quadro Kanban em raias horizontais por responsável, prioridade ou tipo de atividade. | Should Have | Ao alterar o filtro de raia, os cartões são reagrupados instantaneamente mantendo suas respectivas colunas. |
| **RF-004** | Gestão e movimentação de tarefas | O sistema deve permitir criar, editar, mover entre colunas e arquivar cartões no quadro Kanban. | Must Have | A movimentação do cartão atualiza seu status no banco de dados e mantém o histórico de alteração de coluna. |
| **RF-005** | Checklists de subtarefas | O sistema deve permitir adicionar, marcar como concluído, editar e remover itens de checklist dentro de uma tarefa. | Must Have | O cartão exibe visualmente o progresso numérico e percentual de subtarefas concluídas (ex.: 3/5). |
| **RF-006** | Atribuição de responsáveis | O sistema deve permitir vincular um ou mais Membros da Equipe a um cartão no quadro. | Must Have | Os membros atribuídos são exibidos com avatar/iniciais no cartão e filtráveis no quadro. |
| **RF-007** | Arquivamento de tarefas | O sistema deve permitir arquivar tarefas concluídas ou canceladas e disponibilizar uma visão de consulta de tarefas arquivadas. | Should Have | A tarefa arquivada deixa de ser exibida no quadro ativo, mas pode ser restaurada ou consultada na visão de arquivo. |

---

## Requisitos não funcionais

| ID | Categoria | Métrica Mensurável | Método de Verificação |
| :--- | :--- | :--- | :--- |
| **RNF-001** | Desempenho (ISO 25010) | O quadro Kanban deve carregar e renderizar até 200 cartões em menos de 2,0 segundos sob conexão de 10 Mbps. | Teste automatizado de carga no front-end via Lighthouse / K6. |
| **RNF-002** | Usabilidade (ISO 25010) | Operações de arrastar e soltar (drag and drop) de cartões devem responder com latência visual inferior a 100 ms. | Teste de interface automatizado (Cypress/Playwright). |
| **RNF-003** | Segurança e LGPD | O acesso às funcionalidades e dados deve respeitar o modelo RBAC (Controle de Acesso Baseado em Papéis). | Teste de penetração e verificação automatizada de rotas de API. |

---

## Regras de negócio

| ID | Regra |
| :--- | :--- |
| **RN-001** | **Permissão de alteração do quadro:** Apenas o Gerente de Projeto pode adicionar, renomear, reordenar ou excluir colunas e raias no Kanban. |
| **RN-002** | **Exclusão de colunas com tarefas:** Não é permitido excluir uma coluna que contenha tarefas ativas; as tarefas devem ser movidas ou arquivadas previamente. |
| **RN-003** | **Rastreabilidade de arquivamento:** Tarefas arquivadas mantêm todo o seu histórico de alterações, comentários e checklists inalterados para fins de auditoria. |

---

## Histórias de usuário

### US-001: Criar e configurar novo projeto
- **Como** Gerente de Projetos,  
- **Quero** cadastrar um novo projeto informando nome, descrição e datas de execução,  
- **Para** organizar o trabalho e centralizar o fluxo de atividades da equipe.

- **Épico:** Gestão de Projetos
- **Prioridade:** Must Have
- **Requisitos relacionados:** RF-001, RNF-003

**Cenário 1: Cadastro de projeto com sucesso**
- **Dado** que estou autenticado como "Gerente de Projetos" na tela de criação de projetos
- **Quando** eu preencho o nome "Projeto Alfa", a descrição "Desenvolvimento do módulo X" e a data de início "01/11/2026"
- **E** clico no botão "Salvar"
- **Então** o sistema cria o projeto, exibe a mensagem "Projeto criado com sucesso" e me redireciona para o quadro Kanban do projeto.

**Cenário 2: Tentativa de cadastro sem nome do projeto (Exceção)**
- **Dado** que estou autenticado na tela de criação de projetos
- **Quando** deixo o campo "Nome do Projeto" em branco e clico em "Salvar"
- **Então** o sistema não envia o formulário e exibe a mensagem de validação "O nome do projeto é obrigatório".

---

### US-002: Personalização de colunas do quadro Kanban
- **Como** Gerente de Projetos,  
- **Quero** adicionar, renomear e reordenar as colunas do quadro Kanban,  
- **Para** adaptar o fluxo visual do quadro ao processo específico da equipe.

- **Épico:** Gestão de Quadros
- **Prioridade:** Must Have
- **Requisitos relacionados:** RF-002, RN-001, RN-002

**Cenário 1: Adicionar nova coluna ao quadro**
- **Dado** que estou na visualização do quadro Kanban do "Projeto Alfa" como "Gerente de Projetos"
- **Quando** clico em "Adicionar Coluna", insiro o nome "Em Homologação" e confirmo
- **Então** a nova coluna é exibida à direita no quadro Kanban e fica disponível para receber cartões.

**Cenário 2: Exclusão negada de coluna com tarefas ativas (Exceção)**
- **Dado** que a coluna "Em Teste" possui 2 cartões ativos
- **Quando** eu tento excluir a coluna "Em Teste"
- **Então** o sistema bloqueia a ação e exibe o alerta "Não é possível excluir a coluna pois existem tarefas ativas. Mova ou arquive as tarefas antes de continuar."

---

### US-003: Agrupamento do Kanban em Raias (Swimlanes)
- **Como** Membro da Equipe,  
- **Quero** alterar a visualização do quadro para agrupar as tarefas em raias horizontais por responsável ou prioridade,  
- **Para** identificar facilmente gargalos e o volume de trabalho alocado a cada membro.

- **Épico:** Visualização do Quadro
- **Prioridade:** Should Have
- **Requisitos relacionados:** RF-003, RNF-001

**Cenário 1: Alternar agrupamento para visão por Responsável**
- **Dado** que estou visualizando o quadro Kanban do projeto
- **Quando** seleciono no filtro de exibição a opção "Agrupar por Responsável"
- **Então** o quadro é reestruturado em linhas horizontais separadas por cada membro da equipe, mantendo os cartões nas suas respectivas colunas de status.

**Cenário 2: Seleção de agrupamento sem membros atribuídos (Exceção)**
- **Dado** que existem cartões no quadro sem nenhum responsável atribuído
- **Quando** aplico o agrupamento por "Responsável"
- **Então** o sistema exibe uma raia adicional chamada "Não atribuídos" contendo os respetivos cartões.

---

### US-004: Movimentação de cartões de tarefas
- **Como** Membro da Equipe,  
- **Quero** arrastar e soltar um cartão de uma coluna para outra,  
- **Para** atualizar o status de progresso do trabalho de forma simples e rápida.

- **Épico:** Gestão de Tarefas
- **Prioridade:** Must Have
- **Requisitos relacionados:** RF-004, RNF-002

**Cenário 1: Mover tarefa entre colunas com sucesso**
- **Dado** que o cartão "Ajustar API de login" está na coluna "A Fazer"
- **Quando** arrasto e solto o cartão para a coluna "Em Andamento"
- **Então** o cartão permanece na coluna "Em Andamento" e o status da tarefa é atualizado instantaneamente para a equipe.

**Cenário 2: Falha de conexão durante a movimentação (Exceção)**
- **Dado** que a minha conexão com a rede foi interrompida
- **Quando** tento mover um cartão para outra coluna
- **Então** o sistema retorna o cartão para a coluna de origem e exibe o aviso "Falha de conexão. A movimentação não foi salva."

---

### US-005: Detalhamento de tarefas com Checklists
- **Como** Membro da Equipe,  
- **Quero** criar e marcar itens em um checklist dentro do cartão da tarefa,  
- **Para** acompanhar as etapas operacionais necessárias até a conclusão do item.

- **Épico:** Gestão de Tarefas
- **Prioridade:** Must Have
- **Requisitos relacionados:** RF-005

**Cenário 1: Marcar item do checklist como concluído**
- **Dado** que abri o detalhe do cartão com um checklist de 3 itens
- **Quando** clico na caixa de seleção do item "Escrever testes unitários"
- **Então** o item é marcado como concluído e o indicador do cartão no quadro passa a exibir "1/3".

**Cenário 2: Adicionar item em branco no checklist (Exceção)**
- **Dado** que estou editando o checklist de um cartão
- **Quando** tento adicionar um item sem preencher o texto descritivo
- **Então** o botão de adicionar permanece desabilitado e nenhum item é adicionado ao checklist.

---

### US-006: Arquivamento de tarefas do quadro
- **Como** Membro da Equipe,  
- **Quero** arquivar cartões finalizados ou cancelados,  
- **Para** manter a visualização do quadro limpa e focada no trabalho em andamento.

- **Épico:** Gestão de Tarefas
- **Prioridade:** Should Have
- **Requisitos relacionados:** RF-007, RN-003

**Cenário 1: Arquivar tarefa concluída**
- **Dado** que abri os detalhes da tarefa "Corrigir layout do menu"
- **Quando** clico no botão "Arquivar Tarefa" e confirmo a ação
- **Então** a tarefa é removida da visualização do quadro ativo e armazenada no repositório de arquivados.

**Cenário 2: Tentar editar dados de tarefa arquivada sem restaurar (Exceção)**
- **Dado** que estou visualizando uma tarefa na tela de "Tarefas Arquivadas"
- **Quando** tento alterar o título ou checklist diretamente da visão de arquivo
- **Então** os campos são exibidos como somente leitura e a mensagem "Restaure a tarefa para realizar alterações" é exibida.

---

## Premissas

- Os usuários acessarão a aplicação através de navegadores web modernos (Chrome, Firefox, Edge, Safari) atualizados.
- A autenticação de usuários será realizada via e-mail e senha com gerenciamento centralizado de sessões.

---

## Restrições

- O escopo desta versão não inclui aplicativo móvel nativo (iOS/Android), focando em interface web responsiva.
- Não haverá controle financeiro, faturamento ou gestão de custos de projetos no primeiro release.

---

## Fora de escopo

- Limites de WIP (Work in Progress) por coluna.
- Métricas avançadas de fluxo (Lead Time, Cycle Time, Cumulative Flow Diagram).
- Registro de horas trabalhadas (Timesheet).
- Anexo de arquivos pesados nos cartões.

---

## Próximos passos sugeridos

1. Criar a **Modelagem de Dados (Diagrama ER)** em Mermaid contemplando as entidades Projeto, Quadro, Coluna, Tarefa, Checklist e Usuário.
2. Elaborar o **Diagrama de Sequência** para o fluxo de movimentação de tarefas no Kanban (`RF-004`).
3. Definir o **Diagrama de Processo (BPMN)** cobrindo o ciclo de vida de uma tarefa no sistema.