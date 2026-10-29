<!--
Sync Impact Report
- Version: scaffold não ratificado → 1.0.0 (adoção inicial)
- Princípios adicionados: I–X, conforme os dez artigos fornecidos
- Seções preenchidas: Additional Constraints; Development Workflow; Governance
- Seções removidas: nenhuma
- Follow-up: remover este relatório temporário antes do commit; nenhum placeholder foi diferido
-->

# studio-erp Constitution

## Core Principles

### I. Padronização e Clareza

- Toda documentação produzida para o projeto — incluindo esta constituição, especificações
  (`spec.md`), planos (`plan.md`), tarefas (`tasks.md`), decisões, checklists, mensagens de
  commit conforme a convenção adotada e relatórios de validação — MUST ser escrita em português
  brasileiro, idioma padrão do projeto.
- Identificadores técnicos, nomes de APIs e pacotes, comandos, variáveis de ambiente e caminhos
  MUST permanecer no idioma original quando sua tradução puder comprometer precisão ou execução.
- Os mesmos termos de negócio MUST ser usados de forma consistente nas especificações, no código,
  no esquema de dados e na interface. Conceitos de domínio ambíguos MUST ser definidos na spec
  correspondente antes da implementação.

### II. Simplicidade e Economia de Complexidade

- A solução MUST ser a mais simples que satisfaça integralmente os requisitos confirmados (KISS).
  Capacidades, parâmetros, tabelas, pontos de extensão e estruturas para necessidades futuras não
  confirmadas em spec MUST NOT ser implementados (YAGNI).
- Regras de negócio compartilhadas MUST ter uma representação canônica, evitando duplicação entre
  interface, controllers, tarefas agendadas e integrações. Semelhança superficial de código, por si
  só, MUST NOT justificar abstrações.
- Classes e módulos MUST manter responsabilidades claras, dependências explícitas e contratos
  coerentes. Interfaces ou camadas intermediárias MUST NOT ser criadas sistematicamente sem
  necessidade concreta de variação.
- Regras de negócio MUST permanecer separadas da apresentação e dos detalhes de infraestrutura,
  como banco de dados, transporte HTTP, sistema de arquivos e serviços externos.
- DDD, Clean Architecture, CQRS, microsserviços, Event Sourcing e padrões similares MUST NOT ser
  impostos sem necessidade técnica e de negócio demonstrada e aprovada.
- Funcionalidades fora do escopo confirmado MUST NOT ser inventadas. Multitenancy, arquitetura
  distribuída, filas de mensageria e metas numéricas de desempenho MUST NOT ser presumidos sem
  solicitação explícita.
- Regras específicas de multitenancy, valores monetários, anexos, filas assíncronas e integrações
  externas MUST ser detalhadas e implementadas somente quando a capacidade correspondente for
  confirmada em spec própria. Isso inclui, quando aplicável, isolamento por organização, precisão,
  moeda e arredondamento, armazenamento, limites e validação MIME e antimalware de anexos.

### III. Segurança, Privacidade e Menor Privilégio

- Toda entrada externa MUST ser validada no componente confiável que a recebe quanto a tipo,
  formato, tamanho, limites e regras aplicáveis. Cada operação protegida MUST verificar autorização
  para cada recurso acessado, aplicar menor privilégio e negar acesso por padrão.
- A aplicação MUST usar mecanismos consolidados de autenticação e proteção de sessões, tokens ou
  outras credenciais, conforme a arquitetura adotada. O plano MUST identificar ameaças aplicáveis
  e controles verificáveis para mitigá-las, consultando referências pertinentes, como OWASP.
- Em ambientes compartilhados e de produção, as identidades da aplicação MUST ter somente as
  permissões necessárias, sem privilégios administrativos ou de alteração estrutural do banco. A
  execução de migrações MUST usar identidade separada, com permissões limitadas e acesso controlado;
  as credenciais de migração MUST NOT estar disponíveis ao runtime.
- Segredos MUST permanecer fora do código-fonte e dos arquivos versionados. É proibido versionar
  credenciais, tokens, chaves privadas, arquivos de ambiente com segredos, dumps ou dados reais de
  clientes. Exemplos de configuração MUST conter apenas valores fictícios. Segredos MUST ser
  fornecidos por configuração ou gestão de segredos adequada ao ambiente, não podem aparecer em
  logs, respostas, documentação, prompts ou saídas de ferramentas, e MUST ter procedimento de
  revogação e substituição em caso de exposição.
- O sistema MUST coletar, exibir e registrar apenas dados pessoais necessários à finalidade
  definida. Specs que envolvam dados pessoais MUST identificar categorias, finalidades, acessos e
  requisitos de compartilhamento. A base legal e as obrigações aplicáveis MUST ser definidas ou
  validadas pelo responsável pelo tratamento, nunca presumidas pelo agente.
- Specs pertinentes MUST definir prazos ou critérios de retenção, eventos que iniciam a contagem e
  hipóteses justificadas de conservação. O plano MUST estabelecer descarte ou anonimização
  verificáveis para bancos, arquivos, logs, réplicas e expiração de backups. Restaurações MUST NOT
  reintroduzir permanentemente dados cuja eliminação foi determinada.
- O sistema MUST permitir o atendimento dos direitos aplicáveis dos titulares, com verificação
  adequada da identidade e registro do atendimento. Eliminação, bloqueio e anonimização MUST
  respeitar obrigações de conservação e a integridade dos registros. Exclusão lógica, mascaramento
  e pseudonimização MUST NOT ser apresentados como eliminação ou anonimização efetiva.
- Dados confidenciais ou sensíveis MUST ser protegidos em trânsito e em repouso, inclusive em
  backups. O plano MUST definir o alcance da proteção e a gestão de chaves, com controle de acesso
  e separação adequada. Senhas de autenticação MUST ser armazenadas com hash específico para
  senhas, salt e parâmetros adequados.
- Dados pessoais reais de clientes, segredos e conteúdo confidencial de produção MUST NOT ser
  disponibilizados a assistentes de desenvolvimento por arquivos, prompts, logs, terminal, MCP ou
  consultas. Devem ser usados dados sintéticos ou dados com anonimização validada. Esquemas e
  metadados MUST ser revisados antes do compartilhamento. Arquivos de ignore MUST NOT ser tratados
  como barreira suficiente de acesso.
- Os controles de segurança e privacidade MUST ter verificações proporcionais ao risco. Antes da
  operação com dados reais, MUST haver responsáveis e procedimentos para identificar, conter e
  tratar incidentes, preservar evidências e apoiar comunicações legalmente aplicáveis.

### IV. Integridade e Persistência de Dados

- Invariantes de negócio MUST ser protegidas por validações na aplicação e restrições apropriadas
  no banco, como chaves estrangeiras, `NOT NULL`, tipos adequados, `CHECK` e índices de unicidade
  exigidos pelo domínio.
- Alterações dependentes entre si MUST ocorrer em transações ACID, com conclusão integral ou
  reversão conjunta. O código MUST NOT presumir que rollback do banco desfaz chamadas externas já
  realizadas.
- Fluxos sujeitos a alterações simultâneas MUST tratar concorrência explicitamente, por bloqueio
  otimista ou pessimista adequado, para impedir perda silenciosa de atualizações (`lost updates`).
- Regras de unicidade e detecção de duplicidade MUST constar na spec conforme a regra real de
  negócio. Atributos como nome ou e-mail MUST NOT ser presumidos únicos sem confirmação.
- Alterações estruturais de banco e transformações de dados MUST ser versionadas por migrações ou
  scripts controlados. Migrações já aplicadas em ambientes compartilhados MUST NOT ser editadas
  retroativamente para mudar seu efeito.
- Mudanças com risco de perda ou alteração destrutiva de dados MUST documentar previamente o
  impacto, a estratégia de execução e a recuperação. Reversão automática sem perda MUST NOT ser
  presumida para toda migração.

### V. Confiabilidade e Operação Resiliente

- Falhas MUST ser tratadas explicitamente. Exceções MUST NOT ser silenciadas, e uma operação que
  falhou total ou parcialmente MUST NOT ser reportada como bem-sucedida.
- Operações repetíveis — como reenvios, retentativas de rede, webhooks e tarefas agendadas — MUST
  usar idempotência ou deduplicação quando efeitos duplicados puderem prejudicar o negócio.
- Integrações externas MUST definir limites de tempo (`timeouts`), política controlada de novas
  tentativas (`retries`) e tratamento de falhas parciais. Retentativas ilimitadas ou repetição
  insegura de efeitos externos MUST NOT ser usadas.
- Logs MUST ser estruturados ou claros o suficiente para diagnóstico, identificar o contexto da
  operação e omitir ou mascarar dados sensíveis e segredos.
- As specs MUST identificar ações críticas de negócio ou segurança que exigem trilha de auditoria.
  Essa trilha MUST registrar ao menos autor, data e hora, recurso afetado e ação, com proteção contra
  acesso ou adulteração indevidos.
- Antes da operação com dados reais, a estratégia de backup e o procedimento de restauração MUST
  estar documentados e validados por evidência prática de teste de recuperação.

### VI. Qualidade, Testes Proporcionais ao Risco e Evidências

- Testes automatizados MUST ser proporcionais ao risco, priorizando regras críticas, autorização,
  regressões, contratos e fluxos críticos criados ou alterados, inclusive falhas e limites. Um
  percentual arbitrário de cobertura MUST NOT ser usado como prova isolada de qualidade.
- Testes MUST NOT ser criados para cada classe ou método sem necessidade ligada ao risco, nem
  limitar-se a reproduzir a estrutura interna da implementação ou duplicar cobertura existente.
- Antes de criar testes, o responsável MUST verificar e reutilizar a suíte, fixtures e recursos
  existentes. Mocks, fixtures e infraestrutura duplicados, dispersos ou sem uso MUST NOT ser
  criados; a organização vigente no repositório MUST ser seguida.
- O plano (`plan.md`) MUST identificar comportamentos a validar, justificar novos testes e
  especificar comandos reproduzíveis de testes, análise estática e estilo adequados à stack. A
  implementação MUST respeitar esse escopo planejado.
- Mocks MUST ser usados somente quando o isolamento de uma dependência for estritamente necessário
  ao cenário, com justificativa explícita no plano. É proibido mockar o comportamento sob teste ou
  substituir por mocks a integração cuja correção está sendo avaliada. Alterações arquiteturais
  exclusivas para facilitar mocks MUST ter justificativa técnica real.
- Correções de bugs MUST incluir teste automatizado de regressão quando viável. Quando não for
  viável, o responsável MUST registrar o motivo e a verificação alternativa realizada.
- Toda alteração MUST registrar como foi validada, e verificações aplicáveis MUST ser realmente
  executadas antes de serem declaradas aprovadas. O relatório de entrega MUST informar comandos,
  resultados, falhas e verificações não executadas com seus impedimentos. Se não houver novos
  testes automatizados, a verificação realizada e suas limitações MUST ser informadas.
- Verificações MUST NOT ser desativadas, testes enfraquecidos ou critérios de aceite alterados
  apenas para obter aprovação da entrega.

### VII. Experiência de Uso e Desempenho

- A interface MUST manter padrões consistentes de navegação, estrutura de formulários, estados de
  carregamento e mensagens de feedback.
- Fluxos implementados MUST exibir erros claros e compreensíveis, preservar com segurança os dados
  já preenchidos após erro de validação e permitir operação completa por teclado.
- Ações destrutivas ou irreversíveis MUST exigir confirmação ou proteção proporcional ao risco e
  informar as consequências antes da execução.
- Listagens, buscas, relatórios e processamentos em lote potencialmente volumosos MUST ser
  paginados ou limitados. A implementação MUST evitar consultas repetitivas, como `N+1`, e
  carregamento desnecessário de registros ou colunas em memória.
- Specs MUST definir metas de desempenho somente quando relevantes ao fluxo e a implementação
  MUST verificá-las com volume representativo. Metas numéricas arbitrárias e sem fundamentação
  MUST NOT ser inventadas.

### VIII. Dependências e Compatibilidade

- Toda dependência externa nova MUST ser justificada pela necessidade concreta no escopo aprovado.
- Antes de adicionar ou substituir dependência direta, o agente ou desenvolvedor MUST apresentar ao
  responsável finalidade, alternativas (inclusive solução nativa), estado de manutenção, licença
  e impactos relevantes, e MUST aguardar aprovação explícita antes de instalar ou incluir no
  manifesto.
- Recursos nativos da linguagem ou plataforma, bibliotecas oficiais e pacotes reconhecidos e
  ativamente mantidos MUST ser priorizados. Popularidade isolada, como downloads ou estrelas, MUST
  NOT justificar adoção.
- Manifestos e arquivos de resolução de versões (`lockfiles`) aplicáveis MUST ser versionados no
  Git para permitir instalações determinísticas e reproduzíveis.
- Alterar stack tecnológica, versão principal (`major`), contratos públicos ou requisitos de
  infraestrutura MUST exigir exposição prévia do impacto e aprovação formal do responsável.

### IX. Governança por Living Spec e Rastreabilidade

- O projeto MUST manter especificações como Living Spec: cada spec aprovada é o contrato vigente
  da funcionalidade que descreve.
- Mudanças no comportamento pretendido MUST atualizar primeiro a spec correspondente. O plano e as
  tarefas MUST ser alinhados à spec atualizada antes do início ou alteração da implementação.
- Funcionalidades com escopo próprio MUST ter specs em diretórios dedicados. O sistema MUST NOT ser
  consolidado automaticamente em uma única spec monolítica.
- O histórico de evolução MUST ser preservado no Git. Ao substituir ou reestruturar planos ou
  tarefas, decisões e justificativas ainda relevantes MUST continuar registradas.
- Specs MUST NOT ser alteradas para legitimar retrospectivamente comportamento incorreto ou bug.
  Mudanças no contrato funcional exigem decisão explícita de produto ou negócio.
- Tarefas concluídas MUST permanecer claramente distinguíveis de trabalho novo ou pendente. Uma
  tarefa MUST NOT ser marcada concluída sem verificação efetiva do resultado.

### X. Governança Compartilhada e Exceções

- Esta constituição MUST vincular todos os agentes de IA e colaboradores humanos que atuem no
  repositório, independentemente da ferramenta utilizada.
- Quando uma solicitação pontual conflitar com esta constituição, o conflito MUST ser explicitado
  imediatamente ao responsável para decisão consciente. Nem a constituição nem a solicitação
  podem ser ignoradas silenciosamente.
- Exceções temporárias ou permanentes MUST ter aprovação explícita e registro do motivo, escopo,
  riscos assumidos e condição ou prazo para revisão.

## Additional Constraints

- O contexto técnico do projeto MUST registrar somente decisões confirmadas e o estado verificado,
  incluindo stack, arquitetura, persistência, ferramentas de qualidade, operação, convenções e
  responsabilidades. Pendências e itens não aplicáveis MUST ser identificados explicitamente;
  hipóteses MUST NOT ser convertidas em obrigações.
- Regras sobre banco de dados, interface, autenticação e operação MUST ser aplicadas quando essas
  capacidades fizerem parte do sistema.

## Development Workflow

- A constituição MUST orientar todas as etapas do fluxo SDD, da especificação à validação da
  implementação. Cada spec, plano, lista de tarefas e revisão de implementação MUST verificar
  explicitamente a aderência a esta constituição antes da aprovação.
- Antes da implementação, a spec vigente, o plano e as tarefas MUST estar alinhados. A validação
  planejada MUST ser executada e reportada de forma fiel, conforme o Artigo VI.
- Mudanças de comportamento MUST passar pelo fluxo de Living Spec do Artigo IX. Decisões técnicas
  compatíveis MUST ser preservadas ao atualizar artefatos; conflitos MUST ser apresentados antes
  de substituir conteúdo vigente.

## Governance

- **Autoridade e conformidade:** esta constituição é normativa para o repositório. Revisões de
  especificações, planos, tarefas e implementação MUST registrar a verificação de conformidade e
  resolver violações antes da aprovação.
- **Emendas:** uma proposta MUST registrar justificativa, aprovação explícita do responsável e
  impactos sobre specs, planos, tarefas e código. Decisões compatíveis MUST ser preservadas; todo
  conflito com o conteúdo vigente MUST ser apresentado antes da substituição.
- **Aprovação e exceções:** somente o responsável pelo projeto pode aprovar emendas e exceções.
  Exceções MUST seguir os requisitos de motivo, escopo, riscos e revisão do Artigo X.
- **Versionamento:** a versão MUST seguir SemVer. Mudança incompatível por remoção ou redefinição
  de princípio exige MAJOR; princípio ou seção nova, ou expansão material de obrigações, exige
  MINOR; esclarecimento, correção ou refinamento sem mudança normativa exige PATCH. A adoção
  inicial desta constituição é 1.0.0.
- **Datas e registro:** a data de ratificação MUST registrar a adoção original; a data da última
  emenda MUST ser atualizada em toda alteração aprovada. Emendas MUST incluir um relatório de
  impacto temporário no topo do arquivo para revisão humana, removível antes do commit.

**Version**: 1.0.0 | **Ratified**: 2026-10-28 | **Last Amended**: 2026-10-28
