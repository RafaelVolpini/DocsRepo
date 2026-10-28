## Diagrama do processo de gestão de projetos

```bpmn
{
  "participantes": [
    {
      "id": "matter",
      "nome": "Sistema Matter",
      "raias": [
        {"id": "gp", "nome": "Gerente de Projeto"},
        {"id": "me", "nome": "Membro da Equipe"},
        {"id": "sys", "nome": "Sistema"}
      ]
    }
  ],
  "nos": [
    {"id": "c1", "raia": "gp", "tipo": "inicio", "rotulo": "Novo projeto"},
    {"id": "a1", "raia": "gp", "tipo": "tarefa_usuario", "rotulo": "Criar e configurar projeto"},
    {"id": "a2", "raia": "gp", "tipo": "tarefa_usuario", "rotulo": "Cadastrar e atribuir tarefas"},
    {"id": "s1", "raia": "sys", "tipo": "tarefa_servico", "rotulo": "Notificar membros atribuídos"},
    {"id": "b1", "raia": "me", "tipo": "tarefa_usuario", "rotulo": "Visualizar tarefas no Kanban"},
    {"id": "b2", "raia": "me", "tipo": "tarefa_usuario", "rotulo": "Executar e mover tarefa"},
    {"id": "b3", "raia": "me", "tipo": "tarefa_usuario", "rotulo": "Registrar horas trabalhadas"},
    {"id": "b4", "raia": "me", "tipo": "tarefa_usuario", "rotulo": "Submeter entrega para revisão"},
    {"id": "s2", "raia": "sys", "tipo": "tarefa_servico", "rotulo": "Validar regras e atualizar status"},
    {"id": "g1", "raia": "gp", "tipo": "gateway_exclusivo", "rotulo": "Entrega aprovada?"},
    {"id": "a3", "raia": "gp", "tipo": "tarefa_usuario", "rotulo": "Encerrar projeto"},
    {"id": "s3", "raia": "sys", "tipo": "tarefa_servico", "rotulo": "Consolidar relatórios e métricas"},
    {"id": "f1", "raia": "sys", "tipo": "fim", "rotulo": "Projeto encerrado"}
  ],
  "fluxos": [
    {"de": "c1", "para": "a1"},
    {"de": "a1", "para": "a2"},
    {"de": "a2", "para": "s1"},
    {"de": "s1", "para": "b1"},
    {"de": "b1", "para": "b2"},
    {"de": "b2", "para": "b3"},
    {"de": "b3", "para": "b4"},
    {"de": "b4", "para": "s2"},
    {"de": "s2", "para": "g1"},
    {"de": "g1", "para": "a2", "rotulo": "Não"},
    {"de": "g1", "para": "a3", "rotulo": "Sim"},
    {"de": "a3", "para": "s3"},
    {"de": "s3", "para": "f1"}
  ]
}
```

**Legenda:** Modelo BPMN padronizado dividindo responsabilidades em três raias de execução (`Gerente de Projeto`, `Membro da Equipe` e `Sistema`). Distingue tarefas de usuário das automações de serviço do sistema, tratando a decisão de revisão (`g1`) com laço de retrabalho para o cadastro/atribuição de ajustes.

## Premissas

- As notificações automatizadas acionadas pelo sistema assumem envio assíncrono (e-mail/web push).
- O encerramento do projeto exige aprovação prévia das entregas vinculadas pelo Gerente de Projeto.

## Próximos passos sugeridos

1. Atualizar a seção correspondente do documento de requisitos para substituir a modelagem Mermaid pelo modelo BPMN oficial.
2. Mapear cenários de exceção no fluxo (ex.: encerramento prematuro do projeto ou cancelamento de tarefa) em subprocessos de evento.
3. Especificar os diagramas de sequência para as automações do sistema (`s1`, `s2` e `s3`).