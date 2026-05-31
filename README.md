# Banco de Dados — Dicionário de Dados
**Projeto: People Analytics**

---

## usuarios
**Função:** Tabela central de identidade corporativa. Gerencia o cadastro dos colaboradores, integração com a autenticação (Supabase Auth) e estrutura hierárquica.

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| id | uuid | Identificador único do usuário (PK e FK auth) |
| nome | text | Nome do colaborador |
| email | text | Email corporativo |
| created_at | timestamp | Data de criação |
| role | text | Perfil de acesso |
| data_contratacao | date | Data de admissão |
| gerente_geral | text | Nome do gestor direto |
| tenant_id | integer | Empresa |
| cargo | text | Cargo atual |
| saldo_ferias | integer | Saldo de férias |
| gerente_id | uuid | Hierarquia (auto-relacionamento) |
| nivel_cargo | integer | Nível hierárquico |

---

## pontos
**Função:** Tabela legada de registro de jornada de trabalho (entradas, saídas e intervalos). Mantida temporariamente para compatibilidade de dados históricos até a migração completa.

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| id | integer | Identificador |
| tenant_id | integer | Empresa |
| usuario_id | uuid | Usuário |
| horario | timestamp | Marca de ponto |
| tipo | text | Tipo (entrada, saída, almoço etc) |
| observacao | text | Observações |
| created_at | timestamp | Data de criação |

---

## new_pontos
**Função:** Nova estrutura oficial para o registro de espelho de ponto. Substitui a tabela legada, otimizando o armazenamento e a consulta das marcações diárias de jornada dos colaboradores.

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| id | integer | Identificador |
| tenant_id | integer | Empresa |
| usuario_id | uuid | Usuário |
| horario | timestamp | Marca de ponto |
| tipo | text | Tipo |
| observacao | text | Observações |
| created_at | timestamp | Data de criação |

---

## pulse
**Função:** Armazena os resultados das pesquisas recorrentes de clima organizacional (Termômetro/Pulse). Captura o sentimento dos colaboradores e utiliza IA para gerar resumos qualitativos sobre o engajamento da equipe.

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| id | integer | Identificador |
| tenant_id | integer | Empresa |
| usuario_id | uuid | Usuário |
| score_humor | integer | Humor (0–100) |
| sentimento_predominante | text | Sentimento |
| resumo_ia | text | Resumo gerado por IA |
| updated_at | timestamp | Atualização |

---

## cursos
**Função:** Catálogo corporativo de treinamentos. Armazena os metadados dos cursos oferecidos pela empresa, incluindo prazos de validade e links de acesso para capacitação dos colaboradores.

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| id | integer | Identificador |
| tenant_id | integer | Empresa |
| usuario_id | uuid | Usuário |
| titulo | text | Nome do curso |
| link | text | Link |
| descricao | text | Descrição |
| criado_por | uuid | Criador |
| created_at | timestamp | Criação |
| status | text | Status |
| prazo_dias | integer | Prazo |

---

## historico_cursos
**Função:** Trilha de aprendizagem individual. Registra quais treinamentos foram concluídos por cada usuário, monitorando a conformidade (cursos obrigatórios) e o cumprimento de prazos.

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| id | uuid | Identificador |
| tenant_id | integer | Empresa |
| usuario_id | uuid | Usuário |
| nome_curso | varchar | Nome do curso |
| obrigatorio | boolean | Curso obrigatório |
| prazo_limite | date | Data limite |
| data_conclusao | date | Data de conclusão |
| concluido_no_prazo | boolean | Conclusão dentro do prazo |
| created_at | timestamp | Criação |

---

## ferias
**Função:** Gerenciador atual de concessão de férias. Controla o agendamento, os períodos de descanso efetivamente usufruídos e o status de aprovação de cada colaborador.

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| id | integer | Identificador |
| usuario_id | uuid | Usuário |
| tenant_id | integer | Empresa |
| data_registro | timestamp | Data registro |
| data_ferias_prevista | date | Previsão |
| status_ferias | text | pendente ou cumprida |
| data_inicio | date | Início |
| data_fim | date | Fim |
| created_at | timestamp | Criação |

---

## legado_ferias
**Função:** Arquivo histórico da estrutura anterior de gestão de férias. Mantém os dados antigos acessíveis e serve como fonte para união (UNION) em relatórios retroativos.

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| id | integer | Identificador |
| tenant_id | integer | Empresa |
| usuario_id | uuid | Usuário |
| data_inicio | date | Início |
| data_fim | date | Fim |
| status | text | Status |
| created_at | timestamp | Criação |

---

## atestados
**Função:** Repositório de justificativas de ausência. Controla os atestados médicos enviados, mapeando os dias de afastamento, o motivo (CID) e o fluxo de aprovação pelo RH.

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| id | integer | Identificador |
| tenant_id | integer | Empresa |
| usuario_id | uuid | Usuário |
| data_emissao | date | Data emissão |
| dias_afastamento | integer | Dias afastado |
| motivo_cid | text | CID |
| url_arquivo | text | Arquivo |
| status | text | Status |
| created_at | timestamp | Criação |

---

## banco_horas
**Função:** Totalizador de jornada compensatória. Armazena o saldo atualizado (positivo ou negativo) de horas extras e faltas não justificadas de cada colaborador.

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| id | integer | Identificador |
| usuario_id | uuid | Usuário |
| tenant_id | integer | Empresa |
| saldo_minutos | integer | Saldo em minutos |
| updated_at | timestamp | Atualização |

---

## historico_promocoes
**Função:** Log de progressão de carreira. Registra todas as movimentações de cargo, datas e motivos, permitindo o acompanhamento do ciclo de vida e tempo de retenção do talento na empresa.

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| id | uuid | Identificador |
| tenant_id | integer | Empresa |
| usuario_id | uuid | Usuário |
| cargo_anterior | text | Cargo anterior |
| cargo_novo | text | Cargo atual |
| data_alteracao | timestamp | Data mudança |
| motivo | text | Motivo |
| created_at | timestamp | Criação |

---

## people_analytics
**Função:** Tabela consolidada (*Datamart*). Agrega métricas de diversas fontes (ponto, treinamentos, pulse, saúde) em uma única visão achatada, servindo como base otimizada para alimentar Dashboards de RH e modelos de IA de forma rápida.

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| usuario_id | uuid | Usuário |
| nome | text | Nome |
| data_contratacao | date | Admissão |
| cargo | text | Cargo |
| faltas_ultimo_mes | int | Faltas mês |
| horas_extras_ultimo_mes | numeric | Horas extras |
| qtd_atrasos_ultimo_mes | int | Atrasos |
| horas_atraso_ultimo_mes | numeric | Horas atraso |
| media_atrasos_trimestre | numeric | Média atrasos |
| ausencias_atestado_ultimo_mes | int | Atestados mês |
| media_ausencia_atestado_trimestre | numeric | Média atestado |
| tempo_meses_ultimas_férias | numeric | Tempo desde férias |
| qtt_promocoes | int | Promoções |
| tempo_ultima_promocao | numeric | Tempo cargo atual |
| cursos_obg_vencidos | int | Cursos vencidos |
| cursos_obg_no_prazo | int | Cursos no prazo |
| cursos_obg_adiantados | int | Cursos adiantados |
| qtt_cursos_opcioanis_2anos | int | Cursos opcionais |
| score_humor | int | Humor |
| sentimento_predominante | text | Sentimento |
| resumo_ia | text | Resumo IA |
| score_burnout | int | Burnout |
| score_turnover | int | Turnover |
| score_engajamento | int | Engajamento |
| score_elegibilidade_promocao | int | Promoção |

---

## predicoes_humanograma
**Função:** Motor de inteligência preditiva. Armazena os *scores* e as recomendações de ação gerados pelos algoritmos de IA, apontando riscos de *turnover* (fuga de talentos), *burnout* e chances de promoção.

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| id | uuid | Identificador |
| usuario_id | uuid | Usuário |
| risco_turnover | int | Risco saída |
| risco_burnout | int | Risco burnout |
| chance_promocao | int | Chance promoção |
| recomendacao_acao | text | Recomendação |
| resumo_ia | text | Resumo IA |
| updated_at | timestamp | Atualização |

---

## historico_conversas
**Função:** Trilha de auditoria e contexto para IA. Registra as interações (prompts e respostas) entre os usuários e os assistentes de IA do sistema, permitindo continuidade no chat e análises de uso.

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| id | integer | Identificador |
| tenant_id | integer | Empresa |
| role | text | Role |
| content | text | Mensagem |
| created_at | timestamp | Data |
| usuario_id | uuid | Usuário |

---

## pdfs
**Função:** Repositório central de arquivos. Armazena os metadados e os links dos documentos corporativos em PDF (manuais, políticas, códigos de conduta) garantindo o isolamento correto por `tenant_id`.

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| id | uuid | Identificador |
| tenant_id | integer | Empresa |
| nome | text | Nome PDF |
| url | text | Link |
| criado_em | timestamp | Criação |

---

## pdf_vectors
**Função:** Base de conhecimento vetorial (RAG). Armazena fragmentos de texto (*chunks*) e suas representações matemáticas (*embeddings*), sendo o motor que permite à IA realizar buscas semânticas e responder perguntas com base nos documentos da empresa.

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| id | uuid | Identificador |
| pdf_id | bigint | PDF |
| tenant_id | bigint | Empresa |
| chunk_text | text | Texto |
| embedding | vector | Embedding |
| chunk_index | int | Ordem |
| embedding_model_used | text | Modelo |
| created_at | timestamp | Criação |

---

## niveis_carreira
**Função:** Tabela de domínio estrutural. Define e padroniza os degraus da hierarquia corporativa (ex: Analista, Gerente), servindo como referência para perfis de acesso e algoritmos de promoção.

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| id | integer | Nível |
| nome_nivel | text | Nome |
