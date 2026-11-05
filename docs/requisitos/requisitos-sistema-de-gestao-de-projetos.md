## Contexto do projeto
O sistema apoiará a gestão de projetos e tarefas por equipes internas e clientes externos, permitindo organizar o trabalho e acompanhar sua execução. O escopo inicial considera colaboração em projetos e tarefas; detalhes de plataforma, permissões e regras específicas ficam [a confirmar].

## Requisitos funcionais

| ID | Requisito funcional | Ator | Critério de aceite | Prioridade |
|---|---|---|---|---|
| RF-001 | O sistema deve permitir que um usuário autorizado crie um projeto com nome e descrição. | Equipe | O projeto é criado e fica disponível aos participantes autorizados. | Must |
| RF-002 | O sistema deve permitir convidar clientes e membros da equipe para participar de um projeto. | Gestor de projeto | O participante convidado recebe acesso ao projeto conforme sua autorização. | Must |
| RF-003 | O sistema deve permitir registrar uma tarefa vinculada a um projeto, com título e descrição. | Equipe | A tarefa aparece na lista do projeto. | Must |
| RF-004 | O sistema deve permitir atribuir um responsável e um prazo a uma tarefa. | Equipe | Responsável e prazo ficam visíveis nos detalhes da tarefa. | Should |
| RF-005 | O sistema deve permitir atualizar o estado de uma tarefa. | Responsável pela tarefa | O novo estado é exibido na tarefa e no projeto. | Must |
| RF-006 | O sistema deve permitir que participantes autorizados consultem o andamento de projetos e tarefas. | Cliente ou equipe | A consulta apresenta os projetos e tarefas acessíveis ao usuário e seus estados atuais. | Must |

## Requisitos não funcionais

| ID | Categoria | Requisito e meta | Verificação | Prioridade |
|---|---|---|---|---|
| RNF-001 | Segurança | O sistema deve restringir o acesso a projetos e tarefas conforme as permissões concedidas; [meta a definir] para os controles de autorização. | Testes de acesso autorizado e não autorizado para os papéis definidos. | Must |
| RNF-002 | Usabilidade | O sistema deve permitir que usuários identifiquem projeto, responsável, prazo e estado das tarefas; [meta a definir] para usabilidade. | Teste de usabilidade com equipe e clientes, com critérios e metas [a confirmar]. | Should |
| RNF-003 | Desempenho | O sistema deve apresentar listas de projetos e tarefas em até [meta a definir]. | Teste de desempenho com volume de dados e condições de carga [a confirmar]. | [a confirmar] |