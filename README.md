# AWS Certified Developer - Associate (DVA-C02)

Resumo de estudo com conceitos, serviços, comparações, cenários e pegadinhas. A organização segue a ideia: explicar o que cada serviço faz, quando utilizá-lo e quais diferenças ajudam a resolver questões.

**Base do conteúdo:** todas as tarefas dos quatro domínios e os serviços listados no PDF `developer-associate-02 (1).pdf`, complementados com documentação oficial da AWS. Os cenários deste material são exemplos didáticos próprios. A prioridade sugerida reflete a relação dos assuntos com as tarefas do guia; a AWS não publica uma porcentagem de questões para cada serviço.

**Conferência das fontes: 21/09/2026.** Na página atual da certificação, a AWS informa que o último dia para realizar o **DVA-C02 é 01/12/2026** e que as inscrições para o DVA-C03 abrem em 27/10/2026. Este resumo mantém o foco no **DVA-C02 solicitado**. [Página oficial da certificação](https://aws.amazon.com/certification/certified-developer-associate/).

## Índice

1. [Como é a prova e o que estudar](#1-como-é-a-prova-e-o-que-estudar)
2. [Regions, AZs e disponibilidade](#2-regions-azs-e-disponibilidade)
3. [Padrões de aplicação e resiliência](#3-padrões-de-aplicação-e-resiliência)
4. [SDK, CLI e chamadas à AWS](#4-sdk-cli-e-chamadas-à-aws)
5. [IAM e AWS STS](#5-iam-e-aws-sts)
6. [Cognito e autorização da aplicação](#6-cognito-e-autorização-da-aplicação)
7. [KMS, criptografia e certificados](#7-kms-criptografia-e-certificados)
8. [Secrets Manager, Parameter Store e AppConfig](#8-secrets-manager-parameter-store-e-appconfig)
9. [AWS Lambda](#9-aws-lambda)
10. [Amazon API Gateway](#10-amazon-api-gateway)
11. [Amazon DynamoDB](#11-amazon-dynamodb)
12. [Amazon S3](#12-amazon-s3)
13. [Amazon SQS](#13-amazon-sqs)
14. [SNS e EventBridge](#14-sns-e-eventbridge)
15. [Step Functions](#15-step-functions)
16. [Kinesis e processamento de streams](#16-kinesis-e-processamento-de-streams)
17. [RDS, Aurora e caches](#17-rds-aurora-e-caches)
18. [EC2, contêineres e armazenamento](#18-ec2-contêineres-e-armazenamento)
19. [VPC, balanceadores, Route 53 e CloudFront](#19-vpc-balanceadores-route-53-e-cloudfront)
20. [CloudFormation, SAM e CDK](#20-cloudformation-sam-e-cdk)
21. [CI/CD e estratégias de implantação](#21-cicd-e-estratégias-de-implantação)
22. [Testes, ambientes e Amazon Q Developer](#22-testes-ambientes-e-amazon-q-developer)
23. [CloudWatch, X-Ray e CloudTrail](#23-cloudwatch-x-ray-e-cloudtrail)
24. [Outros serviços previstos no guia](#24-outros-serviços-previstos-no-guia)
25. [Cenários integrados e diagnóstico](#25-cenários-integrados-e-diagnóstico)
26. [Números e pegadinhas para revisar](#26-números-e-pegadinhas-para-revisar)
27. [Roteiro de estudo e prática](#27-roteiro-de-estudo-e-prática)
28. [Mapa de cobertura do guia](#28-mapa-de-cobertura-do-guia)

---

## 1. Como é a prova e o que estudar

### Formato

- **65 questões:** 50 pontuadas e 15 de avaliação de conteúdo, sem pontuação. Você não sabe quais são as 15.
- **130 minutos** de duração.
- Questões de **múltipla escolha** e **múltiplas respostas**.
- Aprovação a partir de **720 pontos**, em uma escala de 100 a 1.000.
- **720 pontos não significa simplesmente 72% de acertos:** a pontuação é escalonada.
- Não há desconto adicional por errar; deixar em branco conta como erro.
- A aprovação considera o resultado geral; não existe uma nota mínima separada para cada domínio.

### Domínios oficiais

| Domínio | Peso | O que você precisa conseguir fazer |
|---|---:|---|
| Desenvolvimento com serviços da AWS | **32%** | Integrar serviços, implementar aplicações resilientes, desenvolver Lambda e acessar dados |
| Segurança | **26%** | Autenticar, autorizar, criptografar e proteger segredos e dados sensíveis |
| Implantação | **24%** | Empacotar, testar, usar IaC, publicar versões e realizar rollback |
| Solução de problemas e otimização | **18%** | Interpretar logs, métricas e traces; encontrar gargalos e ajustar recursos |

Esses números vêm do [guia oficial do DVA-C02](https://docs.aws.amazon.com/aws-certification/latest/developer-associate-02/developer-associate-02.html) e também constam no PDF enviado.

### A mudança de foco em relação ao SAA-C03

Na Developer, você precisa acompanhar o comportamento da aplicação: **quem chama, com qual permissão, como os dados chegam, o que acontece quando falha e como a nova versão é publicada**.

Exemplo: conhecer a finalidade do SQS é o começo. Também é necessário entender por que uma mensagem reaparece, como configurar sua DLQ e como uma Lambda deve responder quando apenas parte do lote falha.

**Sugestão de prioridade:** Lambda, IAM/STS/Cognito, DynamoDB, API Gateway, S3, mensageria, CI/CD/IaC e observabilidade merecem estudo aprofundado. Redes, EC2, bancos relacionais e contêineres completam os cenários de desenvolvimento.

O guia coloca fora das atribuições esperadas o projeto completo de redes, a administração de sistemas operacionais e o desenho de pipelines do zero. Entretanto, **usar pipelines, ajustar templates, entender permissões e diagnosticar conexões fazem parte do conteúdo**. Não elimine esses assuntos só porque a palavra “arquitetura” aparece na lista de atividades fora do escopo.

### Novidades que o PDF efetivamente inclui

O material enviado menciona Amazon Q Developer, EventBridge, circuit breaker, APIs de terceiros, OpenSearch, autorização refinada, comunicação entre microsserviços, mascaramento de dados, aplicações com vários clientes, AppConfig, testes de eventos e verificações de prontidão.

**Atenção à diferença:** o uso de **Amazon Q Developer para desenvolvimento e geração de testes está nas tarefas do guia**. Já a seção específica de **tópicos emergentes** descreve perguntas de pré-teste sem pontuação. Não conclua que todo assunto relacionado a IA ficará fora da nota.

---

## 2. Regions, AZs e disponibilidade

### Region × Availability Zone × Edge Location

| Conceito | Significado | Exemplo de aplicação |
|---|---|---|
| **Region** | Área geográfica que contém múltiplas AZs | Escolher São Paulo ou outra Region para executar a aplicação |
| **Availability Zone — AZ** | Localização isolada dentro de uma Region, formada por um ou mais datacenters | Distribuir instâncias por duas AZs para suportar uma falha local |
| **Edge Location** | Ponto de presença próximo aos usuários para serviços de borda | Entregar conteúdo pelo CloudFront com menor latência |

**Uma AZ não é necessariamente um único datacenter.** AZs de uma mesma Region têm conexão de baixa latência, mas são desenhadas com isolamento de falhas. [Zonas de disponibilidade](https://docs.aws.amazon.com/whitepapers/latest/aws-fault-isolation-boundaries/availability-zones.html).

### Multi-AZ × Multi-Region

| Requisito do enunciado | Direção da solução | Por quê |
|---|---|---|
| Continuar funcionando se uma AZ falhar | **Multi-AZ** | Distribui componentes dentro da mesma Region |
| Continuar funcionando se uma Region inteira ficar indisponível | **Multi-Region** | Exige recursos e dados utilizáveis em outra Region |
| Atender usuários distantes com conteúdo em cache | **CloudFront** | Aproxima a entrega sem exigir uma cópia completa do backend em cada local |
| Executar operações dinâmicas perto de usuários em continentes diferentes | Avaliar aplicação e dados em múltiplas Regions | Cache sozinho não resolve toda a latência de processamento |
| Manter dados em uma determinada localização | Region e fluxos de dados compatíveis com o requisito | Replicação, backups e logs também precisam respeitar a restrição |

**Memorize:** Multi-AZ lida com falhas dentro de uma Region; Multi-Region amplia a proteção para falhas regionais. Nenhuma das duas dispensa configuração correta, capacidade suficiente e testes de recuperação.

### Qual é o alcance de cada recurso?

| Recurso | Alcance que você deve reconhecer |
|---|---|
| IAM | Serviço global dentro da partição AWS; a permissão pode autorizar recursos regionais |
| CloudFront e DNS do Route 53 | Serviços com atuação global |
| VPC | Uma Region; pode conter subnets em várias AZs |
| Subnet | Uma única AZ |
| Instância EC2 | Uma AZ por vez |
| Volume EBS | Uma AZ; precisa ser compatível com a AZ da instância |
| Auto Scaling Group de EC2 | Uma Region, podendo distribuir instâncias em várias AZs |
| Lambda, SQS, SNS e API Gateway | Recursos regionais; você seleciona a Region ao configurar e acessar |
| DynamoDB | Tabela regional; Global Tables configura replicação entre Regions |
| S3 | Bucket regional; S3 Standard distribui dados entre AZs da Region |
| EFS Regional | Sistema de arquivos distribuído entre AZs; também existe EFS One Zone |

**Pegadinha:** um serviço ter presença global não significa que cada recurso seja global. Um bucket S3 tem uma Region. Uma função Lambda criada em São Paulo não é automaticamente implantada nos Estados Unidos.

### Disponibilidade, durabilidade, escalabilidade e recuperação

- **Disponibilidade:** conseguir usar o serviço quando necessário.
- **Durabilidade:** preservar os dados ao longo do tempo.
- **Escalabilidade:** suportar aumento de carga com mais recursos.
- **Elasticidade:** ajustar recursos à demanda, inclusive reduzindo-os.
- **Backup:** cópia recuperável dos dados em determinado momento.
- **Replicação:** manutenção de cópias dos dados; uma exclusão indevida pode ser replicada.
- **RTO:** tempo máximo aceitável para recuperar o serviço.
- **RPO:** quantidade de perda de dados aceitável, expressa em tempo.

**Exemplo:** RTO de 30 minutos e RPO de 5 minutos significam voltar a operar em até 30 minutos, aceitando perder no máximo os últimos 5 minutos de dados.

### Cenários de AZ e Region

1. **Duas EC2 na mesma AZ e exigência de sobreviver à falha dessa AZ:** distribuir as instâncias em AZs diferentes, com balanceamento e dependências também resilientes.
2. **Aplicação em duas AZs, mas banco único sem redundância:** o banco continua sendo um ponto de falha. A disponibilidade precisa ser avaliada de ponta a ponta.
3. **RDS Multi-AZ e exigência de recuperação após perda da Region:** Multi-AZ sozinho não atende. É necessário um mecanismo de recuperação em outra Region e uma estratégia de aplicação e roteamento.
4. **Usuários de outro país reclamam da demora para baixar imagens:** considerar CloudFront antes de presumir que todo o sistema precisa ser duplicado em outra Region.
5. **EBS em uma AZ e EC2 em outra:** não basta anexar diretamente. Crie um volume na AZ de destino, por exemplo a partir de um snapshot.
6. **S3 Standard precisa resistir à falha de uma AZ:** a redundância entre AZs já faz parte dessa classe. Replicação entre Regions atende outro nível de requisito.

---

## 3. Padrões de aplicação e resiliência

### Conceitos que aparecem nos enunciados

| Comparação | Como entender | Consequência prática |
|---|---|---|
| **Stateful × stateless** | Stateful depende de estado mantido pela instância; stateless externaliza o estado necessário | Sessões em DynamoDB/ElastiCache facilitam substituição e escala de instâncias |
| **Síncrono × assíncrono** | No síncrono, o chamador espera a resposta; no assíncrono, o trabalho pode terminar depois | Upload aceito e processamento posterior é um cenário assíncrono |
| **Acoplamento forte × fraco** | Componentes fortemente acoplados dependem diretamente uns dos outros | Uma fila permite ao produtor continuar mesmo quando o consumidor está lento |
| **Monólito × microsserviços** | Um pacote principal versus componentes com responsabilidades e implantação separáveis | Microsserviços acrescentam desafios de comunicação e diagnóstico distribuído |
| **Coreografia × orquestração** | Serviços reagem a eventos versus um coordenador conduzindo etapas | EventBridge ajuda na coreografia; Step Functions na orquestração |
| **Fanout × consumidores concorrentes** | Distribuir uma cópia para cada interessado versus dividir trabalho entre workers | SNS com filas separadas versus vários consumidores da mesma fila |

**Stateless não significa “sem banco de dados”.** Significa que a instância não precisa guardar localmente a sessão ou o estado durável para atender a próxima requisição.

### Retry, backoff, jitter e circuit breaker

- **Retry:** repetir uma operação que pode ter falhado temporariamente.
- **Exponential backoff:** aumentar progressivamente a espera entre tentativas.
- **Jitter:** variar aleatoriamente essa espera, evitando milhares de clientes tentando novamente juntos.
- **Timeout:** limitar quanto tempo uma chamada pode consumir.
- **Circuit breaker:** interromper temporariamente chamadas a uma dependência com falhas repetidas e testar sua recuperação depois.
- **Limite de concorrência:** proteger recursos que não suportam um número ilimitado de operações simultâneas.

**Cenário:** uma API externa começa a retornar 503. Use timeout, tentativas limitadas com backoff e jitter e, conforme o requisito, circuit breaker. Repetir indefinidamente pode saturar sua própria aplicação.

**Não aplique retry indiscriminadamente:** credencial inválida, acesso negado e payload inválido normalmente exigem correção. Confira também as tentativas já feitas pelo SDK para não multiplicá-las desnecessariamente. [Comportamento de retries dos SDKs](https://docs.aws.amazon.com/sdkref/latest/guide/feature-retry-behavior.html).

### Idempotência

Uma operação idempotente mantém o efeito de negócio correto mesmo quando recebe a mesma solicitação mais de uma vez.

**Exemplo:** o cliente repete uma solicitação após timeout. Se o pagamento já foi confirmado, a aplicação deve retornar o resultado existente em vez de cobrar novamente.

Uma abordagem utiliza:

1. Identificador estável da operação, como `idempotencyKey`.
2. Registro persistente com estado e resultado.
3. Escrita condicional para impedir que duas execuções assumam o mesmo trabalho ao mesmo tempo.
4. Tratamento para execução interrompida, expiração e reprocessamento.
5. Idempotência também na dependência externa, quando disponível.

**Pegadinha:** gravar “já processei” antes do efeito externo não resolve sozinho a falha entre essas etapas. Uma condição no DynamoDB não transforma uma chamada HTTP externa em parte da mesma transação.

### Contratos, serialização e validação

- Valide campos obrigatórios, tipos e regras de negócio.
- Defina formatos estáveis para datas, identificadores e dinheiro.
- Serialize para transmitir ou persistir; desserialize para voltar à estrutura da aplicação.
- Considere compatibilidade entre versões de eventos e APIs.
- Uma mensagem JSON válida ainda pode estar incorreta para o negócio.
- Em JavaScript, cuidado com precisão de números grandes e valores monetários; escolha uma representação adequada ao contrato.

---

## 4. SDK, CLI e chamadas à AWS

### O que cada ferramenta faz

- **AWS SDK:** acessa serviços a partir do código, cuidando de detalhes como assinatura de chamadas, paginação assistida e retries configuráveis.
- **AWS CLI:** executa operações pelo terminal; útil para scripts e diagnóstico.
- **AWS CloudShell:** disponibiliza um terminal no navegador com ferramentas AWS, usando as permissões da identidade conectada.
- **API do serviço:** contrato que SDK e CLI utilizam para executar operações.

### O que conferir quando uma chamada falha

| Sintoma | Verificações úteis |
|---|---|
| Recurso aparentemente não existe | Region, conta, nome, ARN e endpoint |
| `AccessDenied` | Identidade efetiva, ação, recurso, condições e políticas aplicáveis |
| Token expirado | Renovação das credenciais temporárias e provedor de credenciais |
| Assinatura inválida | Credenciais, Region da assinatura, relógio e alteração da requisição |
| Lista incompleta | Token de paginação ou `LastEvaluatedKey` |
| Throttling | Taxa de chamadas, capacidade, concorrência e retries com backoff |

### Credenciais

**Em workloads AWS, prefira roles e credenciais temporárias.** Os SDKs podem obtê-las pelo provedor adequado ao ambiente, como a role de execução do Lambda ou a task role do ECS.

No desenvolvimento local, use perfis e autenticação apropriada. A ordem exata da cadeia de credenciais depende do SDK: variáveis de ambiente antigas podem fazer a aplicação usar outra identidade sem você perceber.

O comando `aws sts get-caller-identity` ajuda a conferir quem está executando uma operação. Ele não demonstra, sozinho, que essa identidade tem permissão para o recurso desejado.

**Signature Version 4 — SigV4:** adiciona autenticação às requisições AWS por uma assinatura calculada a partir das credenciais e dos dados da chamada. SDK e CLI normalmente fazem esse trabalho. Região, serviço e data participam do processo; não envie a secret access key como se fosse um token comum. [Assinatura SigV4](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_sigv.html).

### Paginação e falhas parciais

- Uma resposta bem-sucedida pode conter apenas a primeira página.
- Use paginadores ou continue até acabar o token de continuação.
- Operações em lote podem retornar sucesso na chamada e falha em itens individuais.
- DynamoDB pode devolver `UnprocessedItems` ou `UnprocessedKeys`; envie novamente apenas o que ficou pendente, com backoff.
- Nas operações do SQS em lote, confira os resultados de sucesso e falha de cada entrada.

**Cenário:** um script retorna apenas parte dos objetos ou registros, sem erro. Antes de aumentar recursos, verifique se ele percorre todas as páginas.

---

## 5. IAM e AWS STS

### Termos essenciais

| Termo | Função |
|---|---|
| **Principal** | Identidade que faz a solicitação: usuário, sessão de role, serviço etc. |
| **Policy** | Documento que permite ou nega ações sob determinadas condições |
| **Role** | Identidade assumível com permissões; não exige chave permanente embutida na aplicação |
| **Trust policy** | Define quem pode assumir a role |
| **Permissions policy** | Define o que a identidade pode fazer |
| **Resource-based policy** | Política no recurso, como bucket, fila ou função |
| **STS** | Emite credenciais temporárias para cenários como assumir roles e federação |

### Estrutura de uma policy

Reconheça `Effect`, `Action`, `Resource` e `Condition`. Políticas de recursos também podem especificar `Principal`.

**Exemplo de raciocínio:** permitir `s3:GetObject` sobre `arn:aws:s3:::meu-bucket/documentos/*` não concede automaticamente `s3:ListBucket`. A listagem usa uma ação diferente e o ARN do bucket, sem o sufixo de objetos.

### Avaliação de permissões

- Por padrão, uma solicitação sem permissão aplicável é negada.
- Um **Deny explícito prevalece** sobre um Allow aplicável.
- Políticas de identidade e de recurso participam da avaliação conforme o principal e o cenário.
- **Permissions boundaries** limitam permissões; não concedem acesso por si mesmas.
- **SCPs**, quando aplicáveis, estabelecem limites na organização; também não concedem permissões.
- Condições podem restringir origem, transporte seguro, tags e outros atributos da solicitação.

**Pegadinha:** adicionar outra policy com Allow não corrige uma negativa explícita. [Lógica de avaliação do IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html).

### Roles de aplicação × permissão para invocar

**Lambda que lê S3:** sua execution role precisa das permissões de leitura.

**S3 que dispara Lambda:** a função precisa permitir a invocação pelo S3, normalmente em uma resource-based policy. São direções diferentes de autorização.

**Pessoa que configura uma role para um serviço:** pode precisar de `iam:PassRole`. Essa ação permite passar a role a um serviço autorizado; não equivale a obter para si as credenciais da role com `sts:AssumeRole`.

### Acesso entre contas

Uma abordagem comum:

1. A conta de destino possui uma role com as permissões necessárias.
2. A trust policy aceita o principal da conta de origem.
3. A identidade de origem pode executar `sts:AssumeRole` nessa role.
4. A aplicação utiliza o conjunto temporário de credenciais retornado.

Credenciais temporárias incluem **access key ID, secret access key e session token**. Esquecer o token pode causar falha mesmo quando as duas primeiras informações estão corretas.

Para fornecedores que operam em nome de vários clientes, **External ID** ajuda a evitar o problema do *confused deputy*. Ele é uma condição de confiança, não uma senha. [Acesso de terceiros por roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_common-scenarios_third-party.html).

### Cenários

- **Aplicação em EC2 precisa ler S3:** role associada à instância por instance profile.
- **Contêiner precisa escrever DynamoDB:** task role no ECS.
- **Lambda precisa acessar Secrets Manager:** permissão na execution role, além de KMS quando aplicável.
- **Acesso continua negado com policy aparentemente correta:** investigue recurso, condições, KMS, políticas de recursos e limites aplicáveis.

**Memorize:** autenticação identifica quem chama; autorização determina o que esse principal pode fazer.

---

## 6. Cognito e autorização da aplicação

### User Pool × Identity Pool

| Recurso | Entrega principal | Quando usar |
|---|---|---|
| **Cognito User Pool** | Diretório de usuários e tokens de autenticação | Cadastro, login, MFA e acesso autenticado à aplicação |
| **Cognito Identity Pool** | Credenciais AWS temporárias associadas a roles | Aplicativo precisa acessar diretamente serviços AWS com permissões limitadas |

**Cenário:** usuário entra no aplicativo e chama sua API com JWT: User Pool pode atender à autenticação. Se o aplicativo também precisa obter credenciais para acessar S3 diretamente, avalie Identity Pool. Os dois podem trabalhar juntos, mas não são obrigatoriamente usados em conjunto.

O papel do Identity Pool é documentado em [Cognito Identity Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-identity.html).

### Tokens

- **ID token:** informações sobre a identidade do usuário.
- **Access token:** autorização para operações e escopos definidos para o acesso.
- **Refresh token:** obtenção de novos tokens conforme as regras da sessão.
- **Bearer token:** quem apresenta o token pode usá-lo; proteja-o no transporte e armazenamento.

**Validar JWT não é apenas decodificar Base64.** Verifique assinatura, emissor, expiração, finalidade do token e público/cliente esperado, conforme o fluxo. Para APIs com escopos, valide também os escopos exigidos. [Cognito User Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools.html).

### Login não resolve toda a autorização

Um usuário autenticado pode não ter permissão para visualizar determinada fatura, documento ou cliente.

**Exemplo:** `GET /documentos/123` precisa verificar se o documento pertence ao cliente autorizado e se o usuário pode realizar a leitura. A existência de um JWT válido não responde essas perguntas.

### Multi-tenant: várias organizações na mesma aplicação

- Obtenha o identificador de cliente de um contexto de identidade confiável.
- Valide associação e permissões em cada operação.
- Inclua o cliente nas chaves e condições de acesso quando o modelo exigir.
- Separe também caches, objetos, mensagens e logs para evitar mistura de dados.
- Não aceite que um `tenantId` enviado livremente pelo usuário determine sozinho o acesso.

**Pegadinha:** criptografar o banco protege o armazenamento, mas não impede o código autorizado de devolver os dados do cliente errado.

### Autenticação entre serviços

Para APIs AWS, use roles e assinatura adequada. Para APIs próprias, os cenários podem envolver OAuth 2.0, tokens com escopo ou TLS mútuo. A identidade de máquina também deve ter permissões mínimas; não reutilize uma conta administrativa para todos os microsserviços.

---

## 7. KMS, criptografia e certificados

### Comparações

| Conceito | O que protege |
|---|---|
| **Criptografia em trânsito** | Comunicação entre componentes, normalmente com TLS/HTTPS |
| **Criptografia em repouso** | Dados armazenados em disco, objetos, bancos e backups |
| **Client-side encryption** | A aplicação criptografa antes de enviar os dados ao serviço |
| **Server-side encryption** | O serviço recebe os dados e realiza a criptografia do armazenamento |

**Criptografia não substitui autorização.** Um usuário autorizado a descriptografar pode ler os dados; sua aplicação ainda precisa controlar a quem os entrega.

### AWS KMS

Gerencia chaves e operações criptográficas, com controle de acesso e integração com serviços AWS.

**Envelope encryption:** os dados são criptografados com uma *data key*; essa chave de dados é protegida por uma chave KMS. Assim, não é necessário enviar um arquivo inteiro ao KMS para criptografá-lo diretamente.

Na operação `GenerateDataKey`, a aplicação pode receber a chave de dados em texto claro e sua versão criptografada. Usa a primeira em memória, descarta-a adequadamente e armazena a versão protegida junto aos dados. [Conceitos do KMS](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html).

### Permissões e uso entre contas

- O acesso a uma chave depende de sua **key policy** e dos mecanismos de autorização aplicáveis.
- Para acesso entre contas, normalmente é preciso permitir o uso na key policy da conta proprietária e na policy IAM da identidade externa.
- Permissão de ler um objeto S3 não garante permissão de descriptografá-lo com KMS.
- Escolha uma chave gerenciada pelo cliente quando precisar controlar a política e compartilhar seu uso entre contas de forma apropriada.

**Cenário:** o download de um objeto SSE-KMS falha, apesar de `s3:GetObject` estar permitido. Verifique `kms:Decrypt`, a chave correta e sua key policy. [KMS entre contas](https://docs.aws.amazon.com/kms/latest/developerguide/key-policy-modifying-external-accounts.html).

### Rotação

Rotacionar a chave KMS, alterar a senha do banco e trocar um token de API são operações diferentes. A rotação do material de uma chave KMS **não recriptografa automaticamente todos os dados existentes**; o serviço preserva o material necessário para descriptografar dados antigos, conforme o tipo de chave e suas regras.

Rotação automática depende de elegibilidade e configuração do tipo de chave. [Rotação no KMS](https://docs.aws.amazon.com/kms/latest/developerguide/rotate-keys.html).

**Chaves e Regions:** chaves KMS são recursos regionais. Chaves Multi-Region relacionadas compartilham material criptográfico e propriedades específicas, mas continuam com recursos e controles regionais; não concedem acesso global automaticamente. [KMS Multi-Region](https://docs.aws.amazon.com/kms/latest/developerguide/multi-region-keys-overview.html).

### ACM, CA privada e SSH

- **AWS Certificate Manager — ACM:** provisionamento e gerenciamento de certificados compatíveis com integrações AWS, como HTTPS em balanceadores.
- **AWS Private CA:** certificados de uma autoridade certificadora privada para identidades e comunicações internas.
- **TLS mútuo — mTLS:** o servidor e o cliente apresentam certificados para autenticação.
- **Chave SSH:** usada em acesso administrativo compatível com SSH; a chave privada precisa ser protegida.
- **Certificado autoassinado:** pode servir em desenvolvimento controlado, mas não é automaticamente confiável para clientes.

Reconheça expiração, cadeia de confiança, nome do host e proteção de chaves privadas como possíveis causas de falhas de conexão.

Para desenvolvimento, reconheça ferramentas como `ssh-keygen` para pares de chaves SSH e OpenSSL para gerar chaves e certificados de teste. Gerar um certificado não faz os clientes confiarem automaticamente nele.

---

## 8. Secrets Manager, Parameter Store e AppConfig

### Qual escolher?

| Serviço | Finalidade principal | Pista no enunciado |
|---|---|---|
| **Secrets Manager** | Armazenar, recuperar e gerenciar o ciclo de vida de segredos | Senha de banco e rotação integrada/automatizada |
| **Systems Manager Parameter Store** | Configuração hierárquica e valores, inclusive `SecureString` | URLs, nomes, parâmetros por ambiente e configuração centralizada |
| **AppConfig** | Distribuir configuração da aplicação com validação e implantação controlada | Feature flags, liberação gradual e rollback de configuração |
| **KMS** | Gerenciar chaves e operações de criptografia | Controle da chave que protege os dados |

Parameter Store também pode armazenar segredos como `SecureString`; a diferença de cenário costuma ser a necessidade de gerenciamento e rotação de credenciais. [Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html), [Parameter Store](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-parameter-store.html).

### Boas práticas de código

- Não coloque senhas no repositório, na imagem de contêiner ou no arquivo público do frontend.
- Conceda à aplicação acesso apenas aos segredos necessários.
- Recupere o segredo em runtime e use cache com renovação compatível com sua rotação.
- Evite registrar tokens, senhas, cabeçalhos de autenticação e dados pessoais completos.
- Use parâmetros separados para desenvolvimento, teste e produção.

**Variável de ambiente criptografada em repouso não torna seu conteúdo invisível durante a execução.** O código pode acessar o valor e até imprimi-lo por engano.

### AppConfig

Permite validar configuração e liberá-la de modo progressivo. A aplicação precisa buscar/consumir a configuração, diretamente ou por mecanismos como agentes e extensões. Alarmes podem orientar rollback de uma implantação de configuração.

**Cenário:** habilitar uma funcionalidade para parte dos usuários, observar erros e poder desativá-la sem recompilar o sistema: feature flag e implantação controlada com AppConfig. [Visão geral do AppConfig](https://docs.aws.amazon.com/appconfig/latest/userguide/what-is-appconfig.html).

### Dados sensíveis e mascaramento

Classifique informações como dados pessoais e dados de saúde, limite coleta e retenção e masque o que aparece nos logs. Um identificador de correlação costuma ser mais útil ao diagnóstico do que imprimir todo o documento do usuário.

**Atenção:** remover um segredo de um commit recente não apaga seu histórico nem invalida a credencial. Uma credencial exposta precisa ser tratada como comprometida e substituída.

---

## 9. AWS Lambda

Executa código em resposta a chamadas e eventos, sem exigir que você administre servidores. É central para APIs, automações, processamento de arquivos e consumidores de eventos.

### Configurações que você precisa reconhecer

- **Handler:** ponto de entrada do código.
- **Runtime:** ambiente de execução da linguagem.
- **Memória:** também influencia a CPU disponível.
- **Timeout:** tempo máximo de uma execução.
- **Variáveis de ambiente:** configuração, com cuidado especial para segredos.
- **Layers:** compartilhamento de bibliotecas e dependências para funções empacotadas em ZIP.
- **Extensions:** integração de funcionalidades como observabilidade e recuperação de configuração.
- **Execution role:** permissões que o código usa para acessar recursos.
- **Triggers e event source mappings:** formas de conectar a função aos eventos.

Uma invocação convencional pode executar por até **15 minutos**. Os pacotes podem ser ZIP ou imagem de contêiner; uma imagem não elimina esse limite. Existem recursos mais novos de execução durável, mas não confunda a duração de um workflow com a duração de uma invocação. [Quotas do Lambda](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-limits.html).

**Empacotamento:** inclua o handler no caminho esperado e as dependências necessárias. Em Node.js com ZIP, é comum o arquivo do handler e `node_modules` ficarem na raiz do pacote. Dependências nativas precisam ser compatíveis com sistema e arquitetura do ambiente Lambda; um pacote compilado apenas para Windows pode não funcionar. [Pacotes ZIP para Node.js](https://docs.aws.amazon.com/lambda/latest/dg/nodejs-package.html).

### Cold start e reutilização

**Cold start** é o trabalho de preparar um novo ambiente, incluindo inicialização. Uma execução posterior pode reutilizar o ambiente, mas isso não é garantido.

- Inicialize clientes de SDK fora do handler quando apropriado.
- Reutilize conexões com os cuidados de validade e keep-alive.
- Reduza dependências e inicialização desnecessárias.
- `/tmp` serve para dados temporários e cache local; não é armazenamento durável ou compartilhado entre todos os ambientes.
- Não mantenha dados sensíveis de usuários em variáveis globais reutilizáveis.
- Meça duração e memória; mais memória pode reduzir tempo suficiente para melhorar custo e desempenho.

**Cenário:** a função demora porque recria conexões e carrega bibliotecas pesadas em toda chamada. Corrija a inicialização antes de apenas aumentar seu timeout. [Boas práticas de Lambda](https://docs.aws.amazon.com/lambda/latest/dg/best-practices.html).

### Reserved concurrency × provisioned concurrency

| Configuração | Resolve principalmente | Pegadinha |
|---|---|---|
| **Reserved concurrency** | Reserva uma parcela da concorrência para a função e estabelece seu teto | Não prepara ambientes antecipadamente |
| **Provisioned concurrency** | Mantém ambientes inicializados para reduzir latência de inicialização | Tem cobrança própria e o excedente pode usar ambientes sob demanda |

**Proteger um banco contra excesso de conexões:** considere limitar concorrência. **Reduzir cold starts em uma API sensível à latência:** considere provisioned concurrency.

Provisioned concurrency deve estar associada à versão/alias adequado, e as chamadas precisam chegar a esse destino. Ela não é configurada para `$LATEST`. [Concorrência reservada](https://docs.aws.amazon.com/lambda/latest/dg/configuration-concurrency.html), [concorrência provisionada](https://docs.aws.amazon.com/lambda/latest/dg/provisioned-concurrency.html).

Uma estimativa útil é: **concorrência média ≈ requisições por segundo × duração média em segundos**. Exemplo: 120 req/s × 0,5 s ≈ 60 execuções simultâneas, antes da margem necessária para picos.

### Lambda SnapStart

Reduz a inicialização restaurando um snapshot do ambiente preparado, nos runtimes e configurações compatíveis. É diferente de manter ambientes previamente inicializados com provisioned concurrency. A aplicação deve cuidar de valores que precisam ser únicos, conexões e credenciais temporárias restauradas do snapshot. SnapStart não pode ser combinado com provisioned concurrency na mesma versão. [Lambda SnapStart](https://docs.aws.amazon.com/lambda/latest/dg/snapstart.html).

### Síncrono, assíncrono e polling

| Forma | Exemplo | Tratamento de erro |
|---|---|---|
| **Síncrona** | API Gateway chamando Lambda | O erro retorna ao chamador; a função não ganha automaticamente retries de erro de negócio |
| **Assíncrona** | Notificação de S3 invocando Lambda | Lambda enfileira internamente e gerencia tentativas conforme a configuração |
| **Event source mapping de fila** | SQS | Depende de visibilidade, processamento do lote e política de redrive da fila |
| **Event source mapping de stream** | Kinesis ou DynamoDB Streams | Checkpoints, ordem e política de falhas influenciam o avanço do consumidor |

Para erro de função em invocação assíncrona, o padrão inclui **até duas novas tentativas**. Throttling, indisponibilidade e expiração do evento têm regras próprias. Não aplique “duas tentativas” a toda integração do Lambda. [Comportamentos de retry](https://docs.aws.amazon.com/lambda/latest/dg/invocation-retries.html).

### DLQ, destinations e lotes

- **DLQ de invocação assíncrona:** retém eventos que não puderam ser processados conforme as regras configuradas.
- **Lambda destinations:** podem encaminhar registros de resultado de sucesso ou falha das invocações assíncronas.
- **SQS como origem:** a DLQ e `maxReceiveCount` pertencem à configuração da fila; a DLQ assíncrona da função não substitui isso.
- **Falha parcial no lote SQS:** habilite `ReportBatchItemFailures` e retorne os identificadores que falharam.
- Se o código lançar uma exceção que encerra a execução do lote, ele pode ser considerado totalmente malsucedido.

**Cenário:** de 10 mensagens, 9 deram certo. A resposta parcial permite tentar novamente a mensagem com falha sem exigir o reprocessamento das 9 bem-sucedidas. Continue garantindo idempotência. Em FIFO, respeite a ordem e sinalize também mensagens não processadas após a falha. [Falhas parciais de SQS](https://docs.aws.amazon.com/lambda/latest/dg/services-sqs-errorhandling.html).

### Lambda na VPC

Use a configuração de VPC quando a função precisar alcançar recursos privados, como um RDS sem endpoint público.

**Pegadinha fundamental:** associar Lambda a uma subnet pública não lhe dá um IP público nem garante internet. Para saída IPv4 à internet, a configuração usual usa subnets privadas com rota para NAT. Para serviços compatíveis, VPC endpoints podem oferecer acesso privado.

**Cenário:** Lambda acessa RDS, mas dá timeout ao chamar uma API pública depois que foi conectada à VPC. Investigue rotas, NAT, regras de rede e DNS. [Acesso de Lambda à VPC](https://docs.aws.amazon.com/lambda/latest/dg/configuration-vpc.html).

### Versões e aliases

- **`$LATEST`:** versão de trabalho que pode ser alterada.
- **Versão publicada:** snapshot do código e da configuração versionada.
- **Alias:** nome estável, como `prod`, que aponta para versão publicada e pode participar de roteamento ponderado.

**Cenário:** liberar uma nova versão para uma pequena fração das chamadas e depois reverter rapidamente. Publique a versão e use alias/implantação controlada, com alarmes. [Versões](https://docs.aws.amazon.com/lambda/latest/dg/configuration-versions.html), [aliases](https://docs.aws.amazon.com/lambda/latest/dg/configuration-aliases.html).

---

## 10. Amazon API Gateway

Publica e gerencia APIs, integrando rotas a Lambda, serviços AWS e backends HTTP. Você precisa entender o percurso da requisição, autenticação, transformação e tratamento de erros.

### REST API × HTTP API × WebSocket API

| Tipo | Uso típico | Recursos que ajudam a distinguir |
|---|---|---|
| **REST API** | APIs que exigem funcionalidades mais amplas de gestão | API keys e usage plans, cache gerenciado e validação de requisições |
| **HTTP API** | APIs HTTP com recursos mais enxutos | Menor custo em cenários compatíveis e suporte nativo a JWT authorizers |
| **WebSocket API** | Comunicação bidirecional persistente | Atualizações em tempo real e gerenciamento de conexões |

**Não escolha apenas pelo nome REST.** HTTP APIs também podem implementar uma interface RESTful. A decisão depende das funcionalidades exigidas. [Comparação oficial de REST e HTTP APIs](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-vs-rest.html).

### Formas de autorização

- **IAM:** requisições assinadas; adequada para chamadores com identidade AWS.
- **Cognito User Pool authorizer:** integração de autenticação para REST APIs.
- **JWT authorizer:** validação de JWT em HTTP APIs compatíveis.
- **Lambda authorizer:** lógica customizada de autorização.

**API key não é autenticação de usuário.** Usage plans e API keys ajudam a identificar consumidores e aplicar controles de uso; não substituem IAM, Cognito ou authorizers.

Quotas e throttling de usage plans são aplicados em regime de melhor esforço; não são um teto rígido garantido de consumo. [Usage plans e API keys](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-api-usage-plans.html).

Em authorizers com cache, cuidado com chave e escopo da decisão armazenada. Uma decisão reutilizada incorretamente pode bloquear rotas válidas ou permitir acessos indevidos. [Lambda authorizers](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-use-lambda-authorizer.html).

### Integração proxy × transformação

Na integração Lambda proxy, a função recebe dados da requisição e deve produzir uma resposta no formato esperado pela versão de payload da integração.

Em REST API com proxy, reconheça `statusCode`, `headers` e `body`, normalmente serializado como string. Uma resposta em formato incompatível pode gerar **502**, mesmo que a função tenha executado.

Integrações e transformações permitem mapear parâmetros, corpo e códigos de status. Validação no gateway ajuda a rejeitar entradas inválidas cedo; regras de negócio e autorização continuam necessárias no backend. [Integração Lambda proxy](https://docs.aws.amazon.com/apigateway/latest/developerguide/set-up-lambda-proxy-integrations.html).

### Stages e implantação

- Stages representam ambientes ou pontos de publicação, como `dev`, `homolog` e `prod`.
- Em REST APIs, alterações que exigem nova implantação precisam ser publicadas no stage correto.
- Stage variables podem parametrizar integrações; não devem funcionar como cofre de segredos.
- Aliases Lambda ajudam a manter um destino estável para cada ambiente.
- Domínios personalizados usam mapeamentos e certificados compatíveis.

### CORS

Controla quais origens o navegador aceita para chamadas entre sites. Confira preflight `OPTIONS`, métodos, cabeçalhos e origem permitida.

**CORS não concede permissão IAM e não protege uma API contra chamadas fora do navegador.** Uma chamada funcionar no Postman e falhar no browser pode indicar configuração CORS, mas também vale verificar a resposta real de erro.

### Throttling, cache e erros

- **429:** pode indicar limite de taxa, burst ou outro controle de capacidade; investigue sua origem.
- **4xx:** indica uma falha associada à requisição ou ao acesso; examine o código específico.
- **5xx:** examine integração, execução e resposta do backend.
- **Timeout:** analise toda a cadeia. Aumentar o timeout do Lambda não altera automaticamente o limite da integração.
- **Cache:** use parâmetros que realmente distinguem respostas; não misture conteúdo de usuários diferentes.

**Cenário:** uma operação demorada não precisa responder imediatamente com o resultado final. A API pode validar, publicar em uma fila e retornar um identificador para consulta posterior.

---

## 11. Amazon DynamoDB

Banco NoSQL gerenciado para acesso por chaves, com escalabilidade e baixa latência. O estudo deve partir das **operações que a aplicação precisa executar**.

### Chaves e distribuição

- **Partition key — PK:** determina a distribuição dos itens.
- **Sort key — SK:** organiza itens que compartilham a mesma PK e permite condições adicionais de consulta.
- A chave primária pode ser somente PK ou o par PK + SK.
- Alta cardinalidade ajuda, mas não garante boa distribuição de tráfego.

**Exemplo:** usar apenas `status = PENDENTE` como PK pode concentrar acessos. Um identificador de entidade com distribuição mais equilibrada costuma ser melhor, conforme o padrão de consulta.

**Hot partition:** uma partição recebe carga desproporcional. Aumentar a capacidade total não corrige automaticamente uma chave muito concentrada.

### GetItem × Query × Scan

| Operação | Como encontra os dados | Uso apropriado |
|---|---|---|
| **GetItem** | Chave primária completa | Buscar um item conhecido |
| **Query** | Igualdade da PK e condições opcionais sobre a SK | Buscar itens de uma chave ou intervalo ordenado |
| **Scan** | Examina itens da tabela ou índice | Varredura quando o acesso por chave não atende |

**Cenário:** a tabela usa `clienteId` como PK e `dataPedido` como SK. Para os pedidos de um cliente em um intervalo de datas, use `Query` com a condição de chave adequada.

### FilterExpression não reduz o trabalho já realizado

O filtro é aplicado **depois da leitura** dos itens selecionados pela operação. Retornar menos itens não significa consumir menos capacidade.

- `KeyConditionExpression`: restringe o acesso pela chave no `Query`.
- `FilterExpression`: elimina itens depois da leitura.
- `ProjectionExpression`: restringe atributos retornados, mas não reduz automaticamente o custo de leitura do item na tabela.
- `Limit` limita itens avaliados; com filtro, o resultado pode ter menos itens ou até vir vazio.
- Continue a paginação quando houver `LastEvaluatedKey`.

**Pegadinha:** trocar um `Scan` por `Scan + filtro` não o transforma em uma consulta eficiente por chave. [Query e capacidade](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Query.Other.html), [Scan](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Scan.html).

### GSI × LSI

| Característica | Global Secondary Index — GSI | Local Secondary Index — LSI |
|---|---|---|
| Partition key | Pode ser diferente da tabela | Mesma PK da tabela |
| Sort key | Pode ser diferente | SK alternativa |
| Criação | Pode ser acrescentado à tabela existente | Definido ao criar a tabela |
| Leitura forte | Não | Sim, quando solicitada |
| Capacidade provisionada | Própria do índice | Compartilhada com a tabela |
| Objetivo | Novo padrão de acesso | Outra ordenação sob a mesma PK |

**Cenário:** tabela por `clienteId`, mas a aplicação passa a procurar pedidos por `numeroPedido`. Um GSI pode atender ao novo acesso. “Global” no nome GSI **não significa Multi-Region**. [Índices secundários](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/SecondaryIndexes.html).

### Consistência

- **Eventual:** uma leitura logo após a escrita pode ainda não mostrar a versão mais recente.
- **Forte:** a leitura reflete escritas bem-sucedidas anteriores, dentro das garantias da operação.
- A tabela e os LSIs permitem solicitar leitura forte.
- GSIs e leituras de DynamoDB Streams são eventualmente consistentes.
- Ler com consistência forte não bloqueia futuras alterações no item.

**Cenário:** a aplicação grava e imediatamente consulta um GSI, mas o registro ainda não aparece. Pode ser propagação do índice. Definir `ConsistentRead` no GSI não resolve porque ele não oferece essa opção. [Consistência de leitura](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.ReadConsistency.html).

### On-demand × provisioned

- **On-demand:** cobrança por solicitações, útil quando a carga é incerta ou varia bastante.
- **Provisioned:** define capacidade e pode usar auto scaling; útil quando existe um perfil de uso conhecido.
- Ambos continuam sujeitos a limites e distribuição de tráfego.
- Um GSI com capacidade insuficiente pode afetar escritas que precisam atualizar esse índice.

### Cálculos essenciais de capacidade

Para operações unitárias convencionais no modo provisionado:

| Operação | Unidade de referência |
|---|---|
| Leitura forte | 1 RCU por leitura/s de até **4 KB** |
| Leitura eventual | Metade do consumo da leitura forte equivalente |
| Escrita padrão | 1 WCU por escrita/s de até **1 KB** |
| Operação transacional | Dobro da capacidade correspondente à operação padrão forte/escrita |

Arredonde o tamanho do item para cima em blocos da operação.

**Exemplo 1:** 10 leituras unitárias fortes/s de itens de 6 KB: cada item ocupa 2 blocos de 4 KB, resultando em **20 RCUs**. Se forem eventuais, **10 RCUs**; se forem leituras transacionais, **40 RCUs**.

**Exemplo 2:** 10 escritas padrão/s de itens de 1,5 KB: 2 blocos de 1 KB por item, totalizando **20 WCUs**. Escritas transacionais equivalentes: **40 WCUs**.

Não extrapole essa conta simplificada sem considerar o tipo de operação, índices e características de lotes. [Operações e capacidade](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/read-write-operations.html).

### Escritas condicionais, transações e concorrência

- **ConditionExpression:** só modifica o item se a condição for satisfeita.
- **`attribute_not_exists` na chave:** ajuda a impedir a criação duplicada de um item.
- **Optimistic locking:** utiliza uma versão para detectar alterações concorrentes.
- **TransactWriteItems:** agrupa escritas com comportamento atômico dentro do escopo suportado.
- **BatchWriteItem:** melhora o envio em lote, mas não transforma o lote inteiro em transação atômica.

**Cenário:** dois workers tentam reservar a última unidade de estoque. Uma atualização condicional evita que ambos confirmem a reserva com base em uma leitura antiga.

Transações não incluem automaticamente chamadas externas nem oferecem uma transação única entre Regions. [Transações no DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/transaction-apis.html).

### TTL, Streams e Global Tables

- **TTL:** remove itens expirados de forma assíncrona, usando um timestamp numérico em segundos. Não é um agendador de exclusão exata. Se a expiração controla acesso, valide também no código. [TTL](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/TTL.html).
- **DynamoDB Streams:** registra alterações de itens e pode acionar consumidores, como Lambda. É útil para reagir a alterações sem fazer varreduras periódicas da tabela.
- **Global Tables:** replica dados entre Regions. A documentação atual distingue modos de consistência eventual e forte entre Regions; disponibilidade e restrições dependem do modo. Evite decorar que Global Tables “sempre é eventual”. [Global Tables](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/globaltables_HowItWorks.html).

### DAX

O **DynamoDB Accelerator — DAX** é um cache gerenciado específico para DynamoDB, útil em leituras repetidas com consistência eventual. Leituras fortes são encaminhadas ao DynamoDB, em vez de serem atendidas pelo cache.

**DAX não resolve uma carga dominada por escritas nem corrige uma chave ruim.** [DAX](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DAX.html).

### Tamanho dos dados

O limite de um item DynamoDB é **400 KB**, incluindo nomes e valores dos atributos. Para arquivos grandes, considere objeto no S3 e metadados/referência no DynamoDB, tratando a consistência entre essas gravações. [Limites do DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Constraints.html).

---

## 12. Amazon S3

Armazenamento de objetos para arquivos, imagens, documentos, artefatos e dados de aplicações. Um objeto é identificado por sua chave dentro de um bucket; “pastas” normalmente são prefixos dessas chaves.

### O que saber para desenvolvimento

- Operações de objetos, como PUT, GET e DELETE, e listagem têm consistência forte nas condições documentadas do S3.
- Isso não faz a replicação entre Regions ser instantânea e não invalida automaticamente caches externos.
- **Versioning:** mantém versões e auxilia a recuperar sobrescritas e exclusões.
- **Multipart upload:** divide o envio em partes, permitindo paralelismo e repetição de partes com falha.
- **Range GET:** busca intervalos de bytes de um objeto.
- **ETag:** não deve ser tratado como MD5 universal, especialmente em uploads multipart e diferentes modalidades de criptografia.

**Cenário:** o objeto foi atualizado no S3, mas o usuário recebe a versão antiga pelo CloudFront. Verifique o cache; não presuma consistência eventual do S3. [Visão geral e consistência do S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html).

### Presigned URLs

URLs pré-assinadas permitem uma operação específica por um período limitado, usando a autorização de quem as gerou.

**Exemplo:** o backend autoriza um upload e entrega uma URL de PUT. O navegador envia o arquivo diretamente ao S3, sem atravessar o processamento do backend.

- Não é preciso deixar o bucket público.
- A URL não concede mais permissões do que o assinante possui.
- Se foi gerada com credenciais temporárias, sua validade também depende dessas credenciais.
- Não trate a URL como descartável após um único uso por padrão.
- Para navegador em outra origem, configure também CORS quando necessário.

**Pegadinha:** definir uma expiração longa não mantém a URL válida depois que a sessão usada para assiná-la expira. [URLs pré-assinadas](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-presigned-url.html).

### Segurança e criptografia

- IAM e bucket policies controlam acesso; Block Public Access ajuda a impedir exposição pública.
- **SSE-S3:** criptografia de armazenamento gerenciada pelo S3.
- **SSE-KMS:** integração com KMS para controle da chave e das operações associadas.
- **Client-side encryption:** a aplicação envia o conteúdo já criptografado.
- HTTPS protege o tráfego; SSE protege o armazenamento. São camadas distintas.

Para SSE-KMS, considere permissões na chave, como `kms:Decrypt` para leitura e `kms:GenerateDataKey` para fluxos de escrita, conforme a operação. [S3 com SSE-KMS](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingKMSEncryption.html).

### Eventos

Eventos de objetos podem integrar S3 com Lambda, SNS, SQS e EventBridge, conforme a configuração.

- Processe considerando possíveis duplicidades e diferenças na ordem de chegada.
- Use prefixos/sufixos ou buckets separados para evitar um loop em que a saída da Lambda gera outra entrada para ela mesma.
- Notificação direta para SQS FIFO não é suportada; o caminho via EventBridge pode atender ao encaminhamento para FIFO.

**Cenário:** ao receber uma imagem, gerar uma miniatura. A miniatura deve ser salva em um destino que não realimente indevidamente o mesmo gatilho. [Eventos do S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/EventNotifications.html).

### Lifecycle e classes

Regras de lifecycle podem transicionar ou expirar objetos e tratar versões não atuais e uploads multipart incompletos.

**Cenário:** documentos precisam de acesso imediato por um período e depois serão arquivados. Avalie frequência, latência de recuperação, retenção e custos das classes. Nem toda classe de arquivo oferece recuperação imediata. [Lifecycle do S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lifecycle-mgmt.html).

---

## 13. Amazon SQS

Fila gerenciada para desacoplar produtor e consumidor, absorver picos e permitir processamento assíncrono.

**Exemplo:** a API registra uma solicitação e envia uma mensagem; workers processam na velocidade suportada pelo banco e pelo serviço externo.

### Standard × FIFO

| Aspecto | Standard | FIFO |
|---|---|---|
| Ordem | Melhor esforço | Ordem dentro de cada `MessageGroupId` |
| Duplicidade | Pode haver entrega repetida | Deduplicação do envio dentro da janela aplicável |
| Paralelismo | Consumidores concorrentes | Grupos diferentes podem avançar em paralelo |
| Uso típico | Trabalho independente com alta escala | Sequência de eventos por pedido, conta ou entidade |

**Pegadinha:** FIFO não remove a necessidade de idempotência no negócio. Se o consumidor executa a operação e falha antes de confirmar/remover a mensagem, poderá haver reprocessamento.

No envio FIFO, reconheça `MessageGroupId` e `MessageDeduplicationId`. Na deduplicação baseada em conteúdo, a identificação considera o corpo, não os atributos. [Deduplicação FIFO](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/FIFO-queues-exactly-once-processing.html).

### Ciclo de vida da mensagem

1. Produtor envia a mensagem.
2. Consumidor recebe a mensagem, que fica temporariamente invisível.
3. O consumidor processa.
4. Após sucesso, remove a mensagem com o mecanismo apropriado.
5. Se não houver remoção e a visibilidade expirar, a mensagem poderá reaparecer.

Na leitura direta pelo SDK, reconheça o **receipt handle** da entrega, utilizado para remover a mensagem. No event source mapping do Lambda, a integração cuida da remoção dos itens considerados processados com sucesso.

### Visibility timeout × delay × retention

| Configuração | Controla | Exemplo |
|---|---|---|
| **Visibility timeout** | Tempo de invisibilidade após receber | Evitar que outro worker pegue o trabalho ainda em execução |
| **Delay** | Espera antes da primeira disponibilização | Adiar o início do processamento |
| **Retention** | Tempo de permanência da mensagem na fila | Manter trabalho pendente enquanto consumidores se recuperam |
| **Long polling** | Quanto a leitura espera por mensagens | Reduzir respostas vazias e chamadas desnecessárias |

**Cenário:** o worker leva 80 segundos e a visibilidade é 30. Outro consumidor pode receber a mensagem antes do término. Ajuste a visibilidade ou estenda-a durante o processamento, mantendo idempotência. [Visibility timeout](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-visibility-timeout.html).

### DLQ e redrive

Uma **Dead-Letter Queue** recebe mensagens após o limite configurado de recebimentos malsucedidos. Ela ajuda a separar mensagens problemáticas, investigar a causa e reprocessá-las depois da correção.

- Monitore a chegada de mensagens à DLQ.
- Não descarte falhas silenciosamente só para fazer a fila diminuir.
- Em FIFO, mover mensagens para DLQ pode afetar a sequência de negócio; avalie esse requisito.
- Escolha um `maxReceiveCount` compatível com falhas transitórias e tempo de recuperação.

### SQS com Lambda

O Lambda consulta a fila por event source mapping e entrega lotes à função. Dimensionamento, tamanho do lote, timeout e concorrência precisam combinar com o backend.

**Cenário:** a fila cresce porque o banco está saturado. Aumentar consumidores sem limite pode piorar o problema. Avalie duração, concorrência, capacidade do banco e falhas, além do volume de chegada. [Lambda com SQS](https://docs.aws.amazon.com/lambda/latest/dg/with-sqs.html).

### Mensagens grandes

O limite atual documentado por mensagem é **1 MiB**. Para payloads maiores, uma abordagem é guardar o conteúdo no S3 e enviar sua referência, controlando permissão e lifecycle do objeto. Evite decorar o antigo valor de 256 KB como limite atual do SQS. [Quotas de mensagens SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/quotas-messages.html).

---

## 14. SNS e EventBridge

### Amazon SNS

Serviço de publicação e assinatura. Um produtor publica em um tópico e os assinantes recebem as mensagens correspondentes.

**Fanout:** um pedido precisa gerar faturamento, envio de e-mail e separação de estoque. Uma solução é um tópico SNS com **uma fila SQS para cada consumidor lógico**. Cada fluxo recebe sua cópia e processa de forma independente.

**Pegadinha:** três workers lendo a mesma fila dividem o trabalho; não garantem uma cópia para cada finalidade de negócio.

### Filtros de assinatura

Uma filter policy na assinatura pode selecionar mensagens por atributos ou pelo corpo JSON, conforme o escopo configurado.

**Cenário:** o consumidor de entrega só precisa de pedidos físicos. Filtrar na assinatura evita invocações e processamento de mensagens irrelevantes. [Filtros do SNS](https://docs.aws.amazon.com/sns/latest/dg/sns-message-filtering.html).

### Amazon EventBridge

Roteia eventos usando regras sobre seu conteúdo e integra fontes AWS, aplicações e fontes compatíveis de terceiros.

Reconheça:

- **Event bus:** recebe eventos.
- **Rules/event patterns:** selecionam quais eventos seguem para quais destinos.
- **Targets:** serviços que recebem o evento.
- **Archive/replay:** armazenamento e reenvio de eventos quando configurados.
- **Scheduler:** agendamento de execuções.
- **Pipes:** conecta fontes a destinos com filtragem e enriquecimento compatíveis.

**Cenário:** um evento com `source` e `detail-type` específicos deve acionar determinados consumidores. EventBridge atende bem ao roteamento por conteúdo. [Visão geral do EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html).

### SQS × SNS × EventBridge

| Necessidade principal | Primeira opção a avaliar |
|---|---|
| Reter trabalho até um consumidor processá-lo | **SQS** |
| Distribuir uma publicação para vários assinantes | **SNS** |
| Rotear eventos por regras e integrar fontes variadas | **EventBridge** |
| Distribuir para vários fluxos com retenção independente | **SNS + uma SQS por fluxo** |
| Encadear etapas, decisões, esperas e compensações | **Step Functions** |

**Não decida apenas pela palavra “evento”:** observe se o problema principal é retenção, distribuição, roteamento ou coordenação. Esses serviços também podem ser combinados.

### Falha de entrega × falha de processamento

EventBridge entregar com sucesso uma invocação assíncrona ao Lambda não prova que o código terminou corretamente. O serviço de origem e o destino podem ter retries e DLQs próprios, atuando em etapas diferentes.

**Cenário:** não há falha de entrega no EventBridge, mas o efeito de negócio não aconteceu. Examine a execução da Lambda e sua política de tratamento assíncrono.

---

## 15. Step Functions

Orquestra fluxos com estados, decisões, paralelismo, esperas e tratamento de falhas. É útil quando a lógica de coordenação ficaria espalhada por várias funções.

### Estados e mecanismos

- **Task:** executa uma atividade ou integração.
- **Choice:** escolhe o próximo caminho.
- **Parallel:** executa ramificações em paralelo.
- **Map:** processa uma coleção de itens.
- **Wait:** aguarda sem manter uma Lambda ocupada apenas esperando.
- **Retry e Catch:** definem novas tentativas e caminhos de erro.
- **Callback com task token:** permite aguardar conclusão externa nas modalidades compatíveis.

### Standard × Express

| Aspecto | Standard | Express |
|---|---|---|
| Perfil | Fluxos duráveis e de longa duração | Fluxos curtos, com grande volume |
| Duração máxima | Até **1 ano** | Até **5 minutos** |
| Modelo de execução | Garantia de execução única do workflow, ressalvadas novas tentativas explicitamente configuradas | Assíncrono: pelo menos uma vez; síncrono: no máximo uma vez |
| Integrações de espera | Suporta padrões como `.sync` e callback compatíveis | Não oferece todos esses padrões |
| Cobrança principal | Transições de estado | Execuções, duração e memória |

As garantias do workflow não transformam automaticamente efeitos externos em transações exatamente uma vez. Configure retries com cuidado. [Tipos de workflow](https://docs.aws.amazon.com/step-functions/latest/dg/choosing-workflow-type.html).

**Cenário:** validar um pedido, reservar estoque, solicitar pagamento e desfazer a reserva se o pagamento falhar. Step Functions pode coordenar as etapas e compensações. A compensação precisa ser implementada; não é um rollback de banco automático entre todos os serviços.

**Pegadinha:** funções que ficam esperando e chamando repetidamente a próxima função podem ser substituídas por estados e integrações gerenciadas, dependendo do requisito.

---

## 16. Kinesis e processamento de streams

**Kinesis Data Streams** recebe sequências de registros que consumidores podem ler e reprocessar dentro do período de retenção.

### Conceitos

- **Stream:** sequência lógica de dados.
- **Shard:** unidade de capacidade e distribuição no modelo provisionado.
- **Partition key:** influencia para qual shard o registro vai.
- **Sequence number:** identifica a posição do registro dentro de seu contexto de stream/shard.
- **Checkpoint:** posição já processada pelo consumidor.
- **Enhanced fan-out:** oferece capacidade dedicada de leitura por consumidor compatível.

**Cenário:** diferentes aplicações precisam consumir os mesmos eventos de navegação e voltar a lê-los depois. Um stream pode atender melhor do que uma fila na qual mensagens são removidas após processamento.

**Ordem:** não presuma uma ordem global entre todos os shards. Considere chave, shard e comportamento do produtor. [Conceitos de Kinesis Data Streams](https://docs.aws.amazon.com/streams/latest/dev/key-concepts.html).

### Lambda com streams

- O event source mapping lê lotes e acompanha o progresso.
- Um registro problemático pode atrasar o processamento do shard afetado.
- Avalie divisão de lotes, idade máxima, tentativas e destino de falhas conforme as opções da integração.
- **IteratorAge** alto indica que o processamento está ficando para trás.
- Ajustar paralelismo precisa preservar as garantias de ordem necessárias.

### Data Streams × Data Firehose × SQS

| Serviço | Problema principal |
|---|---|
| **Kinesis Data Streams** | Stream com consumidores próprios e possibilidade de releitura |
| **Amazon Data Firehose** | Entrega gerenciada de dados a destinos compatíveis, com buffering e transformação opcionais |
| **SQS** | Distribuição de trabalhos que consumidores processam e removem |

Firehose é apresentado como comparação útil à família de streaming; a lista do guia usa o nome Amazon Kinesis. Não trate buffering como garantia de entrega instantânea.

---

## 17. RDS, Aurora e caches

### RDS e Aurora

Use bancos relacionais quando a aplicação precisa de SQL, relacionamentos e transações compatíveis com o modelo relacional.

- **RDS:** serviço gerenciado que oferece diferentes engines.
- **Aurora:** banco relacional da AWS compatível com MySQL ou PostgreSQL, conforme a engine selecionada.
- **Writer endpoint:** direciona operações ao escritor no cenário de cluster correspondente.
- **Reader endpoint:** distribui conexões de leitura entre réplicas elegíveis; não garante leitura imediata de toda escrita recente.
- Aplicações precisam lidar com reconexão, failover, timeouts, transações e credenciais.

### Multi-AZ × read replica

| Recurso | Objetivo principal | Atenção |
|---|---|---|
| **RDS Multi-AZ DB instance**, com um standby | Disponibilidade e failover | O standby não atende consultas de leitura |
| **Read replica** | Escalar leituras e atender outros cenários de replicação | Pode haver atraso de replicação |
| **RDS Multi-AZ DB cluster** | Disponibilidade com arquitetura de cluster | As instâncias de standby também podem servir leituras |

**Pegadinha:** “Multi-AZ nunca atende leitura” é uma generalização incorreta. Identifique se a questão fala da implantação de **instância com standby** ou de **cluster Multi-AZ**. [Modalidades Multi-AZ do RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html).

### RDS Proxy

Gerencia e reutiliza conexões com bancos compatíveis. É útil quando muitas execuções curtas, como Lambdas, abrem conexões simultaneamente.

**Cenário:** o banco ainda tem capacidade para consultas, mas o número de conexões explode durante um pico. RDS Proxy pode reduzir a pressão de conexão. Limitar concorrência e corrigir o uso de conexões pela aplicação também pode ser necessário.

**Pegadinha:** RDS Proxy não é cache de resultados SQL e não torna uma consulta ineficiente automaticamente rápida. [RDS Proxy](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/rds-proxy.html).

### ElastiCache

Cache em memória para reduzir latência e trabalho repetido. Reconheça opções baseadas em Valkey/Redis OSS e Memcached, com recursos e modos de implantação diferentes.

| Padrão | Funcionamento | Cuidado |
|---|---|---|
| **Cache-aside / lazy loading** | Consulta cache; se faltar, lê a origem e preenche | A primeira leitura tem cache miss |
| **Write-through** | A escrita também atualiza o cache | Maior trabalho de escrita; trate falhas entre as camadas |
| **TTL** | Entrada expira após um período | Pode haver dados antigos até a expiração |
| **Invalidação** | Remove/atualiza entradas quando o dado muda | Todas as formas relevantes de mudança precisam ser consideradas |

**Cache stampede:** várias requisições repopulam a mesma entrada ao mesmo tempo. TTLs com variação, coordenação e estratégias de atualização podem reduzir esse pico.

**Cenário:** consulta popular, lenta e que pode aceitar alguns segundos de defasagem. Cache pode ajudar. Para confirmar saldo imediatamente atualizado, não suponha que qualquer cache atende à consistência exigida.

As modalidades e engines do serviço estão descritas em [ElastiCache](https://docs.aws.amazon.com/AmazonElastiCache/latest/dg/WhatIs.html).

### DAX × ElastiCache

- **DAX:** integração específica com DynamoDB.
- **ElastiCache:** cache de aplicação, sessões, resultados calculados e outros usos compatíveis com o engine.
- Cache deve considerar **tenant, usuário, filtros e permissões** que alteram o resultado.

---

## 18. EC2, contêineres e armazenamento

### Amazon EC2

Máquinas virtuais para executar aplicações com maior controle do ambiente. Para DVA-C02, concentre-se no acesso a serviços por role, configuração, logs, health checks e implantação do código.

- **AMI:** imagem usada como base para instâncias.
- **User data:** pode executar tarefas de inicialização; não deve carregar segredos permanentes expostos.
- **Instance profile:** associa uma role à instância.
- **Auto Scaling:** ajusta quantidade de instâncias conforme políticas e necessidades.
- Externalize sessões e arquivos necessários à aplicação para não depender de uma instância específica.

### ECS × EKS × Fargate × ECR

| Serviço | Responsabilidade |
|---|---|
| **ECS** | Orquestra contêineres usando conceitos e APIs AWS |
| **EKS** | Fornece Kubernetes gerenciado |
| **Fargate** | Executa contêineres compatíveis sem administrar os servidores subjacentes |
| **ECR** | Armazena e distribui imagens de contêiner |

Fargate é uma opção de execução integrada a orquestradores; ECR é um registry. Guardar uma imagem no ECR não a coloca em execução.

### ECS: task definition, task e service

- **Task definition:** define imagem, recursos, portas, configuração e permissões associadas.
- **Task:** execução de uma definição.
- **Service:** mantém o número desejado de tasks e integra implantação, health checks e balanceamento conforme a configuração.

### Task role × task execution role

| Role | Quem usa | Exemplo |
|---|---|---|
| **Task role** | Código executado nos contêineres | Ler S3 ou escrever DynamoDB |
| **Task execution role** | Agente/runtime do ECS nas operações permitidas | Baixar imagem do ECR, enviar logs e obter segredos configurados para inicialização |

**Cenário:** o contêiner inicia normalmente, mas sua chamada ao DynamoDB recebe AccessDenied. Confira a **task role**. **Falha ao baixar a imagem** aponta para outro caminho, incluindo a execution role e a conectividade. [Task role](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-iam-roles.html), [task execution role](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_execution_IAM_role.html).

### Imagens e implantação

- Tags ajudam a identificar versões, mas uma tag pode ser alterada se o repositório permitir.
- Digest identifica o conteúdo da imagem e ajuda a garantir implantação reproduzível.
- Não coloque segredos em camadas da imagem.
- Considere compatibilidade de arquitetura de CPU e bibliotecas nativas.
- Examinar vulnerabilidades da imagem complementa testes da aplicação.

### EKS: o que reconhecer

Entenda pod, deployment, service e configuração de workloads. O acesso de um pod aos serviços AWS pode usar mecanismos próprios de identidade, como EKS Pod Identity ou IAM Roles for Service Accounts, conforme o ambiente.

No EKS, associe permissões ao workload apropriado, evitando depender de acesso amplo do nó. [Identidade de pods](https://docs.aws.amazon.com/eks/latest/userguide/pod-identities.html).

**Readiness probe:** indica se o workload está pronto para receber tráfego. **Liveness probe:** ajuda a identificar quando precisa ser reiniciado. Uma aplicação pode estar em execução sem estar pronta para atender.

### EBS × EFS × S3

| Serviço | Modelo | Uso típico |
|---|---|---|
| **EBS** | Volume de blocos vinculado à AZ | Disco de uma instância EC2 |
| **EFS** | Sistema de arquivos compartilhado | Arquivos acessados por vários clientes Linux compatíveis, inclusive integrações com Lambda |
| **S3** | Objetos acessados por API | Uploads, documentos, imagens e artefatos |

**Cenário:** múltiplas execuções precisam de um diretório compartilhado com semântica de sistema de arquivos. EFS pode atender; `/tmp` de cada Lambda não fornece esse compartilhamento.

---

## 19. VPC, balanceadores, Route 53 e CloudFront

### Rede no nível necessário para a prova

Você deve conseguir diagnosticar por que a aplicação não chega ao destino, mesmo sem precisar projetar uma rede corporativa completa.

- **Subnet pública:** tem rota adequada para Internet Gateway; uma EC2 ainda precisa de endereçamento e regras compatíveis para acesso público.
- **Subnet privada:** não tem essa rota direta de internet; pode ter saída por NAT conforme a configuração.
- **Route table:** define para onde o tráfego é encaminhado.
- **Security group:** controle stateful associado às interfaces/recursos compatíveis, com regras de permissão.
- **Network ACL:** controle stateless no limite da subnet, com regras de permitir e negar.
- **DNS:** resolve o nome do destino; uma resolução incorreta pode parecer falha da aplicação.

**Stateful:** o tráfego de resposta de uma conexão permitida é rastreado pelo security group. **Stateless:** no NACL, ida e volta precisam de regras compatíveis, incluindo portas efêmeras quando necessário.

### NAT × VPC endpoint

| Recurso | Objetivo |
|---|---|
| **NAT Gateway** | Permitir saída de tráfego de recursos privados conforme o tipo e o roteamento configurados |
| **Gateway endpoint** | Acesso privado a S3 e DynamoDB pelo mecanismo de rotas compatível |
| **Interface endpoint** | Acesso privado a serviços compatíveis por interfaces e PrivateLink |

**Cenário:** a aplicação só precisa acessar S3 a partir da VPC sem passar pela internet. Um gateway endpoint pode atender. Se precisa chamar uma API pública de terceiro, um endpoint de S3 não resolve esse acesso.

**Pegadinha:** rota funcionando não concede autorização IAM; policy correta não cria conectividade de rede.

O caso de S3/DynamoDB é detalhado em [gateway endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/gateway-endpoints.html).

### ALB × NLB

- **Application Load Balancer — ALB:** balanceamento de aplicação, com regras HTTP/HTTPS como host e caminho.
- **Network Load Balancer — NLB:** cenários de transporte, como TCP/UDP/TLS, com características próprias de desempenho e endereçamento.
- **Target group:** grupo de destinos do balanceador.
- **Health check:** decide se um destino está apto conforme o critério configurado.

**Cenário:** `/api/*` e `/imagens/*` devem seguir para serviços diferentes. O roteamento por caminho do ALB é uma pista importante.

Listeners, regras e target groups compõem esse roteamento. [Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html).

**Aplicação inicia, mas não recebe tráfego:** verifique porta, caminho do health check, código de resposta, readiness e regras de rede.

### Route 53

Serviço DNS. Reconheça roteamento simples, ponderado, por latência e failover.

**Cenário:** encaminhar usuários para outro endpoint quando o principal falha. Route 53 pode participar do failover, mas precisa de destinos válidos e estratégia de dados. Cache DNS e TTL influenciam a propagação; DNS não copia o banco nem garante troca instantânea para todos os clientes.

Escolha a modalidade de acordo com o objetivo descrito. [Políticas de roteamento do Route 53](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html).

### CloudFront

CDN que distribui conteúdo por pontos de presença. Pode reduzir latência e carga na origem.

- **Cache key:** identifica quais requisições compartilham uma resposta em cache.
- **Cache policy:** controla parâmetros do cache, incluindo seleções de headers, cookies e query strings.
- **Origin request policy:** define informações adicionais enviadas à origem; enviar um header não significa que ele faça parte da cache key.
- **TTL:** controla reutilização do conteúdo.
- **Invalidation ou nomes versionados:** maneiras de atualizar conteúdo distribuído.

**Cenário:** resposta varia pelo header de idioma, mas o cache ignora esse header. Usuários podem receber o idioma errado. Configure a chave de cache de acordo com a variação real. [Cache key no CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/controlling-the-cache-key.html).

### Conteúdo privado

- **Origin Access Control — OAC:** restringe o acesso a uma origem S3 compatível para que ela seja usada pela distribuição autorizada.
- **Signed URL:** autorização temporária para um recurso.
- **Signed cookies:** úteis quando o usuário precisa acessar um conjunto de arquivos protegidos.

OAC e autorização do usuário final resolvem controles diferentes. Para OAC, use uma origem S3 compatível; o website endpoint tem tratamento distinto como origem customizada. [Acesso privado à origem S3](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html).

---

## 20. CloudFormation, SAM e CDK

### Comparação

| Ferramenta | Como descreve a infraestrutura | Uso típico |
|---|---|---|
| **CloudFormation** | Templates declarativos YAML/JSON | Recursos AWS e stacks reproduzíveis |
| **AWS SAM** | Extensão de CloudFormation com recursos serverless simplificados | Lambda, APIs, eventos e ferramentas de desenvolvimento |
| **AWS CDK** | Código em linguagens suportadas, sintetizado em CloudFormation | Definição de infraestrutura com abstrações de programação |

**IaC — Infrastructure as Code:** permite versionar e reproduzir a infraestrutura. Mudar algo no console sem refletir no código pode criar divergências.

### CloudFormation

- **Template:** descrição dos recursos.
- **Stack:** conjunto de recursos gerenciado a partir do template.
- **Parameters:** valores de entrada, como ambiente.
- **Resources:** declarações dos recursos.
- **Outputs:** valores expostos pela stack.
- **Mappings e Conditions:** seleção e criação condicional de configurações/recursos.
- **`Ref`, `Fn::GetAtt`, `Fn::Sub`:** referências, atributos e composição de valores.

**Exemplo:** um template recebe `Environment=dev` ou `prod` para construir nomes e configurações apropriados. Isso evita editar manualmente cada recurso para cada ambiente. [Estrutura de templates](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html).

### Change sets, rollback e drift

- **Change set:** permite revisar mudanças propostas antes de executá-las, incluindo possíveis substituições.
- **Rollback:** trata falhas de implantação conforme as condições e configurações da stack.
- **Drift detection:** identifica divergências suportadas entre estado esperado e real.
- **DeletionPolicy/UpdateReplacePolicy:** ajudam a definir retenção e comportamento de recursos em exclusões ou substituições compatíveis.

**Pegadinha:** um change set não garante que a atualização terá sucesso. Dependências, permissões e limites ainda podem causar falha. [Change sets](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-changesets.html).

### AWS SAM

Reconheça recursos como `AWS::Serverless::Function` e o uso de `Transform` para processar a sintaxe SAM.

| Comando | Finalidade |
|---|---|
| `sam init` | Iniciar um projeto |
| `sam build` | Preparar código e dependências |
| `sam local invoke` | Executar função localmente com um evento |
| `sam local start-api` | Simular localmente endpoints compatíveis |
| `sam validate` | Validar o template conforme as opções usadas |
| `sam deploy` | Implantar a aplicação |

Recursos como `AutoPublishAlias` e `DeploymentPreference` ajudam a publicar versões e configurar implantação gradual nas modalidades suportadas.

**Pegadinha:** teste local não comprova que IAM, rede, quotas e integrações reais estão corretos. Faça testes no ambiente AWS apropriado. [AWS SAM](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html).

### AWS CDK

O CDK usa **constructs** para representar infraestrutura. `cdk synth` gera templates; `cdk diff` ajuda a comparar mudanças; `cdk deploy` executa a implantação.

**Cenário:** a equipe quer definir infraestrutura em TypeScript e reaproveitar componentes. CDK pode atender. Ainda é necessário entender o resultado e as permissões de CloudFormation. [AWS CDK](https://docs.aws.amazon.com/cdk/v2/guide/home.html).

---

## 21. CI/CD e estratégias de implantação

### Quem faz o quê?

| Serviço ou artefato | Papel |
|---|---|
| **Repositório Git** | Histórico do código, branches, tags e gatilhos de mudanças |
| **CodePipeline** | Coordena estágios e ações do fluxo de entrega |
| **CodeBuild** | Executa build, testes e preparação de artefatos |
| **CodeDeploy** | Coordena implantação nos destinos compatíveis |
| **CodeArtifact** | Repositório de pacotes e dependências |
| **ECR** | Registry de imagens de contêiner |
| **S3** | Pode armazenar artefatos produzidos pelo pipeline |

Uma mudança no repositório pode iniciar o fluxo, que executa testes, produz um artefato versionado e o promove pelos ambientes. Aprovações e validações podem fazer parte dos estágios. [CodePipeline](https://docs.aws.amazon.com/codepipeline/latest/userguide/welcome.html).

### CodeBuild e `buildspec.yml`

O buildspec define comandos, variáveis, relatórios e artefatos do build. Reconheça fases como:

1. `install`: preparar ferramentas e dependências.
2. `pre_build`: executar tarefas anteriores ao build.
3. `build`: compilar, executar comandos e testes definidos.
4. `post_build`: tarefas finais, como preparar/publicar artefatos.

Os nomes das fases não impedem organizar testes em diferentes pontos conforme o projeto. **O que importa é reconhecer que o buildspec controla a execução do build.**

**Cenário:** o código compila, mas o próximo estágio não encontra o pacote. Verifique caminho e declaração dos artefatos, além das permissões. [Referência de buildspec](https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html).

### CodeDeploy e AppSpec

O **AppSpec** descreve como a aplicação será implantada e quais hooks serão usados, dependendo da plataforma.

- EC2/on-premises pode envolver arquivos, scripts e agente.
- Lambda usa versões, alias e mudança de tráfego.
- ECS pode usar substituição de task sets e mudança de tráfego.
- Hooks permitem validações antes ou depois de etapas compatíveis.

**Memorize:** **buildspec = build; AppSpec = deploy**. [AppSpec](https://docs.aws.amazon.com/codedeploy/latest/userguide/reference-appspec-file.html).

### Estratégias

| Estratégia | Funcionamento | Principal implicação |
|---|---|---|
| **All-at-once** | Troca tudo de uma vez | Maior exposição imediata à nova versão |
| **Rolling** | Atualiza grupos gradualmente | Pode haver mistura de versões e redução temporária de capacidade |
| **Rolling com lote adicional** | Acrescenta capacidade antes de atualizar | Ajuda a preservar capacidade durante o rollout |
| **Immutable** | Cria novos recursos para a versão nova | Facilita abandonar recursos novos se a implantação falhar |
| **Blue/green** | Mantém ambiente atual e novo, com troca de tráfego | Facilita validação e reversão de tráfego |
| **Canary** | Envia uma pequena parcela inicial à versão nova | Observa problemas antes de ampliar a exposição |
| **Linear** | Aumenta o tráfego em incrementos regulares | Liberação gradual em várias etapas |

**Canary 10% por 5 minutos:** primeiro 10%, depois o restante, se o fluxo prosseguir. **Linear 10% a cada minuto:** incrementos sucessivos de 10%. Não são a mesma estratégia. A disponibilidade depende do serviço e do tipo de destino. [Configurações de implantação do CodeDeploy](https://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-configurations.html).

### Elastic Beanstalk

Gerencia o ambiente de uma aplicação usando recursos como EC2, balanceamento e auto scaling. Você entrega uma versão e configura o ambiente.

Reconheça versões da aplicação, ambientes, variáveis, logs, health e políticas de implantação. Beanstalk oferece opções como all-at-once, rolling, rolling com lote adicional, immutable e traffic splitting, conforme a plataforma e a configuração. Blue/green pode usar ambientes separados e troca apropriada de endereçamento/tráfego.

**Cenário:** atualizar preservando capacidade tem exigências diferentes de atualizar com o menor uso temporário de recursos. A estratégia deve respeitar o requisito. [Políticas do Beanstalk](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.rolling-version-deploy.html).

### Rollback precisa abranger compatibilidade

- Alarmes e validações podem interromper uma implantação e acionar a reversão configurada.
- Reverter código não desfaz automaticamente alterações em dados.
- Uma migração incompatível de banco pode impedir que a versão anterior volte a funcionar.
- Durante releases graduais, versões antiga e nova podem coexistir.
- Branches, tags e artefatos imutáveis ajudam a identificar exatamente o que foi publicado.

**Cenário:** a nova Lambda começa a falhar após o canary. Reverta o alias conforme a estratégia existente e examine métricas/logs. Se a nova versão alterou o formato dos dados, a compatibilidade também precisa ser avaliada.

---

## 22. Testes, ambientes e Amazon Q Developer

### Tipos de teste

| Tipo | O que verifica | Exemplo |
|---|---|---|
| **Unitário** | Comportamento de uma unidade isolada | Validação de payload ou decisão de negócio |
| **Integração** | Comunicação entre componentes | Lambda gravando em uma tabela de teste |
| **Contrato** | Compatibilidade de formato e comportamento esperado | Evento publicado contém os campos e tipos combinados |
| **Ponta a ponta** | Fluxo completo | Chamar API e verificar o efeito final |
| **Carga** | Comportamento sob demanda | Concorrência, throttling e latência nos picos |
| **Smoke test** | Funcionamento básico após implantação | Endpoint principal responde e dependências essenciais funcionam |

Mocks ajudam a controlar dependências em testes, mas um SDK mockado não comprova que as permissões e a rede da AWS funcionam.

### Testes de aplicações orientadas a eventos

Teste além do caminho feliz:

- Evento duplicado.
- Campo ausente ou inválido.
- Evento fora de ordem, quando isso é possível.
- Dependência indisponível.
- Parte de um lote falhando.
- Timeout após o efeito de negócio ter ocorrido.
- Mensagem chegando à DLQ.
- Reprocessamento depois da correção.

**Cenário:** o teste dispara um evento e espera sucesso imediato na resposta da publicação. Isso confirma apenas uma parte do fluxo. O teste de integração precisa verificar o processamento e o resultado posterior, com limites de espera apropriados.

### Ambientes

- Separe configuração, dados e permissões de desenvolvimento, teste e produção.
- Use stages, aliases, stacks e imagens/versionamentos conforme cada serviço.
- Teste a mesma versão aprovada que será promovida.
- Evite recompilar silenciosamente um artefato diferente durante a promoção.
- Variáveis e parâmetros de ambiente devem apontar para os recursos corretos.

### Amazon Q Developer

Pode auxiliar na compreensão, geração e revisão de código, investigação de problemas e criação de testes. O PDF cita explicitamente seu uso no desenvolvimento e na geração de testes automatizados.

**Para a prova:** reconheça a finalidade da ferramenta e os cuidados ao utilizá-la. Código gerado precisa ser revisado e validado; testes gerados precisam verificar comportamentos relevantes. Proteja segredos e dados sensíveis e respeite as permissões concedidas. [Amazon Q Developer](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/what-is.html).

### Tópicos emergentes do PDF

O guia também descreve pré-testes sobre IA para revisão e otimização, automação de testes, apoio a CI/CD e diagnóstico. Inclui cuidados com entradas/saídas de modelos, interações de agentes e vazamento de dados em logs.

Estude a visão geral, mantendo a distinção entre essa seção de pré-teste e as habilidades explicitamente listadas nos domínios regulares.

---

## 23. CloudWatch, X-Ray e CloudTrail

### Logs × métricas × traces

| Informação | Pergunta que ajuda a responder |
|---|---|
| **Log** | O que aconteceu nesta execução? |
| **Métrica** | Qual a frequência, taxa, duração ou quantidade? |
| **Trace** | Por onde esta requisição passou e onde gastou tempo? |

**Monitoramento** acompanha sinais conhecidos. **Observabilidade** combina evidências para entender o comportamento interno, inclusive em problemas novos.

### CloudWatch

- **Metrics:** valores numéricos ao longo do tempo.
- **Logs:** registros produzidos pelos componentes.
- **Alarms:** avaliações de métricas e ações configuradas.
- **Dashboards:** visualização da saúde do sistema.
- **Logs Insights:** consulta e análise de logs.

Nas métricas, reconheça namespace, nome, dimensões, período e estatística. **p95** e **p99** ajudam a encontrar latência de cauda que a média esconde.

**Cenário:** a média é 100 ms, mas parte dos usuários espera vários segundos. Examine percentis e traces das requisições lentas. [Conceitos de métricas](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html).

### Logs estruturados

Use campos como `timestamp`, `level`, `service`, `requestId`, `operation`, `durationMs` e código do erro. Preserve um identificador de correlação entre serviços.

**Exemplo de investigação:** localizar os erros de uma operação e agrupar sua contagem por tipo. Logs estruturados permitem esse filtro sem depender de procurar frases variáveis. Controle também retenção e volume. [CloudWatch Logs Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html).

### Métricas customizadas e EMF

Você pode emitir métricas pela API ou usar **Embedded Metric Format — EMF**, escrevendo logs estruturados no formato reconhecido para extração de métricas.

**Cenário:** acompanhar quantos documentos foram processados e quanto demoraram, sem fazer uma chamada síncrona de métrica para cada evento.

**Pegadinha de custo:** colocar um valor único por requisição nas dimensões pode gerar alta cardinalidade. Um ID individual costuma pertencer ao log/trace, não a uma dimensão de métrica. [EMF](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Embedded_Metric_Format.html).

### AWS X-Ray

Ajuda a analisar traces de chamadas distribuídas e identificar serviços, dependências, falhas e latência.

- **Segment:** representa trabalho de um serviço.
- **Subsegment:** detalha uma parte desse trabalho, como uma chamada ao banco.
- **Annotation:** atributo indexado que pode ajudar a filtrar traces.
- **Metadata:** informação adicional que não é indexada como annotation.
- **Sampling:** seleciona parte das requisições para rastreamento.
- **Propagação de contexto:** conecta o trabalho dos vários componentes à mesma requisição.

**Cenário:** uma API fica lenta apenas quando chama um fornecedor. O trace permite separar tempo no código, banco e dependência externa. [Conceitos de X-Ray](https://docs.aws.amazon.com/xray/latest/devguide/xray-concepts.html).

Para instrumentação nova, conheça também OpenTelemetry/ADOT e a orientação atual de migração dos SDKs de instrumentação X-Ray. Isso não elimina a importância de entender o serviço X-Ray para o exame. [Instrumentação com OpenTelemetry](https://docs.aws.amazon.com/xray/latest/devguide/xray-sdk-migration.html).

### AWS CloudTrail

Registra atividades e chamadas de API para auditoria: identidade, ação, momento, origem e contexto disponível.

**Cenário:** descobrir quem alterou a configuração de uma função. CloudTrail é a ferramenta a avaliar. **Descobrir por que seu código levou 8 segundos:** logs, métricas e traces são mais diretamente úteis.

**Management events** e **data events** são categorias diferentes. Operações de dados, como determinados acessos a objetos S3, precisam da configuração correspondente; não presuma que toda leitura de objeto aparece automaticamente no histórico padrão. [Data events no CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/logging-data-events-with-cloudtrail.html).

### Métricas que merecem reconhecimento

| Componente | Sinais úteis |
|---|---|
| Lambda | Errors, Duration, Throttles, ConcurrentExecutions |
| Consumidores de streams | IteratorAge e falhas de lote |
| SQS | Quantidade de mensagens, idade da mais antiga e mensagens em processamento |
| DynamoDB | Throttling, capacidade consumida, latência e concentração de acesso |
| API Gateway | Latência, latência da integração e respostas 4xx/5xx |
| Aplicação | Taxa de sucesso, erros de negócio, latência por operação e saturação |

**Health check precisa representar o objetivo.** Um processo responder “200” não prova que está pronto para a operação principal. Ao mesmo tempo, atrelar toda liveness a qualquer dependência instável pode provocar reinícios desnecessários.

---

## 24. Outros serviços previstos no guia

### AWS AppSync

Fornece recursos gerenciados para APIs GraphQL e experiências em tempo real. Reconheça schema, resolvers, fontes de dados, queries, mutations, subscriptions e autorização.

**Cenário:** o frontend quer consultar os campos necessários por GraphQL, reunindo dados de diferentes fontes. AppSync é uma opção. [AppSync](https://docs.aws.amazon.com/appsync/latest/devguide/what-is-appsync.html).

### AWS Amplify

Ferramentas e serviços voltados à construção e entrega de aplicações web/mobile, com integração de backend e hospedagem conforme os recursos utilizados. Reconheça implantação ligada a branches, configuração por ambiente e integração com autenticação e APIs.

**Cenário:** publicar uma aplicação frontend a partir do repositório, com ambientes por branch e integração com serviços AWS.

A parte de hospedagem e entrega está descrita em [Amplify Hosting](https://docs.aws.amazon.com/amplify/latest/userguide/welcome.html).

### Amazon Athena

Consulta dados, especialmente no S3, usando SQL sem administrar servidores de consulta. Particionamento e formatos adequados podem reduzir dados lidos.

**Cenário:** analisar arquivos de logs no S3 com SQL. Athena é mais apropriado do que transformar esses arquivos em uma tabela transacional apenas para a análise. [Athena](https://docs.aws.amazon.com/athena/latest/ug/what-is.html).

### Amazon OpenSearch Service

Busca e análise sobre dados indexados, útil para pesquisa textual, filtros e análise de logs, entre outros padrões.

**Cenário:** usuários precisam buscar documentos por conteúdo textual e relevância. Um índice de busca pode atender melhor do que varrer todos os itens do DynamoDB. Considere o atraso e as falhas de sincronização entre origem e índice. [OpenSearch](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/what-is.html).

### AWS CodeArtifact

Repositório gerenciado de pacotes, como dependências utilizadas pelo build. Ajuda a controlar publicação e consumo de bibliotecas internas.

**Pegadinha:** biblioteca npm privada aponta para CodeArtifact; imagem de contêiner aponta para ECR; código-fonte versionado aponta para Git. [CodeArtifact](https://docs.aws.amazon.com/codeartifact/latest/ug/welcome.html).

### AWS WAF

Filtra requisições de aplicações web por regras em integrações suportadas. Reconheça regras para padrões de ataques, endereços e taxa de requisições.

**Cenário:** bloquear padrões de SQL injection ou tráfego abusivo no ponto de entrada compatível. WAF não substitui consultas parametrizadas, validação de negócio e autorização no código.

Confira as integrações e funções do [AWS WAF](https://docs.aws.amazon.com/waf/latest/developerguide/what-is-aws-waf.html).

### Systems Manager e CloudShell

Além de Parameter Store, Systems Manager oferece capacidades de operação e automação, como acesso gerenciado a instâncias pelo Session Manager quando configurado.

CloudShell é um ambiente de terminal no navegador. Nenhum deles elimina a necessidade de permissões corretas; uma interface mais simples não torna a identidade administradora.

---

## 25. Cenários integrados e diagnóstico

### Cenário A: geração assíncrona de documentos

**Requisitos:** usuário autenticado solicita um documento; a geração demora; o resultado pertence à organização do usuário; falhas temporárias não podem perder a solicitação.

Uma composição possível:

1. Cognito autentica o usuário.
2. API Gateway recebe a solicitação.
3. O backend valida a permissão e a organização, registra o pedido e entrega trabalho ao SQS com uma estratégia consistente de publicação.
4. Um worker Lambda ou contêiner processa de forma idempotente.
5. O documento vai para S3 privado e o status fica no armazenamento da aplicação.
6. A API autoriza a consulta e gera acesso temporário ao resultado.
7. Logs e métricas mostram o andamento; falhas persistentes seguem para DLQ.

**Pontos para raciocinar:** uma invocação Lambda convencional precisa caber em seu limite; o visibility timeout deve acomodar o processamento; salvar o pedido e publicar a mensagem são duas operações cujo intervalo de falha precisa ser tratado; a URL temporária só deve ser entregue após autorização.

### Cenário B: pico de chamadas a um banco relacional

**Sintoma:** a API funciona em carga baixa, mas os erros de conexão crescem quando muitas Lambdas executam ao mesmo tempo.

**Investigue:** quantidade e duração de conexões, concorrência, credenciais, métricas do banco e traces. Dependendo da causa, reutilização de conexões, RDS Proxy e limitação de concorrência podem ajudar. Se a lentidão está no SQL, examine consultas e índices; aumentar o pool não corrige automaticamente isso.

### Cenário C: implantação com risco controlado

**Requisito:** a nova versão deve receber uma pequena parcela inicial do tráfego e ser revertida se a taxa de erro aumentar.

**Direção:** artefato versionado, alias/estratégia de tráfego compatível, CodeDeploy ou mecanismo suportado do serviço, alarmes CloudWatch e validações. Canary é uma boa correspondência para a pequena exposição inicial. O banco e os contratos precisam aceitar a coexistência das versões.

### Cenário D: acesso entre contas a um arquivo criptografado

**Sintoma:** a aplicação assume a role esperada, mas não consegue ler um objeto SSE-KMS.

**Investigue separadamente:** acesso ao objeto, possíveis negativas de bucket/identidade, chave usada pelo objeto e autorização de descriptografia. Corrigir só a trust policy da role não concede automaticamente todas as permissões S3 e KMS necessárias.

### Tabela de diagnóstico rápido

| Situação do enunciado | O que avaliar primeiro | Raciocínio |
|---|---|---|
| Falha de uma AZ derruba toda a aplicação | Distribuição dos componentes entre AZs | Duplicar instâncias na mesma AZ não atende ao isolamento desejado |
| Perda da Region precisa ser tolerada | Recursos, dados e failover em outra Region | Multi-AZ permanece dentro da Region |
| Lambda precisa responder sem latência de inicialização | Provisioned concurrency e otimização da inicialização | Reserved concurrency controla capacidade, sem pré-inicializar |
| Banco sofre com concorrência excessiva de Lambda | Limite de concorrência e conexões | Escalar o produtor sem controlar o destino pode aumentar a falha |
| Lambda passou a dar timeout para API pública após entrar na VPC | Saída de rede, rotas, NAT e DNS | Subnet pública não fornece IP público à função |
| SQS entrega novamente um trabalho ainda em execução | Visibility timeout e duração | Mensagem pode reaparecer antes de o consumidor terminar |
| SQS entrega novamente trabalho já concluído | Confirmação/remoção e idempotência | Sucesso no negócio e remoção da mensagem são momentos diferentes |
| Só parte do lote Lambda/SQS falha | Resposta parcial de lote | Evita tratar itens bem-sucedidos como falhas |
| Cada serviço precisa receber uma cópia do evento | Fanout com destinos independentes | Consumidores da mesma fila competem pelo trabalho |
| Eventos precisam de roteamento por conteúdo | EventBridge ou filtro compatível do SNS | Escolha conforme fonte, destinos e necessidade de distribuição |
| É preciso esperar aprovação e continuar etapas | Workflow e callback compatíveis | Não mantenha uma função ocupada apenas aguardando |
| DynamoDB consulta um GSI e não vê escrita recém-concluída | Propagação eventual do GSI | O índice não suporta leitura forte |
| DynamoDB faz muitas leituras para retornar poucos itens | Padrão de chave, Query/Scan e filtro | Filtro depois da leitura não evita o consumo anterior |
| DynamoDB sofre throttling apesar de capacidade total disponível | Distribuição da chave e índices | Pode haver concentração de acesso |
| Código lista só parte dos dados | Paginação | Sucesso da chamada não significa resultado completo |
| Download SSE-KMS dá AccessDenied | S3 e KMS | Ler o objeto e usar sua chave são autorizações distintas |
| Senha do banco precisa de rotação gerenciada | Secrets Manager | Rotação de segredo não é rotação de chave KMS |
| Alterar funcionalidade sem recompilar | AppConfig/feature flag | Configuração pode ter rollout próprio |
| Usuário autenticado lê dados de outro cliente | Autorização por recurso e isolamento | Login não comprova propriedade dos dados |
| API retorna 502 após Lambda aparentemente executar | Formato de resposta e falhas da integração | Execução iniciada não garante resposta válida para o gateway |
| Funciona em cliente HTTP, falha no navegador | CORS e resposta real do backend | O navegador aplica regras de origem |
| Contêiner inicia, mas chamada AWS recebe AccessDenied | Task role | A execution role pode ter permitido apenas a inicialização |
| Testes passam e implantação falha | Permissões, artefatos, template e logs do deploy | Teste unitário não valida o ambiente inteiro |
| Descobrir quem alterou um recurso | CloudTrail | É uma pergunta de auditoria |
| Descobrir onde a requisição ficou lenta | Trace/X-Ray e métricas | É uma pergunta de desempenho distribuído |
| Mensagens de stream ficam antigas | IteratorAge, lotes e registros problemáticos | O consumidor pode estar atrasado ou bloqueado por falhas |
| Arquivo atualizado continua antigo para o usuário | Cache e versionamento/invalidation | Cache pode conservar uma resposta apesar da origem atualizada |
| Cache devolve informação de outro cliente | Cache key e autorização | A chave precisa refletir a separação dos dados |

---

## 26. Números e pegadinhas para revisar

### Números úteis

Valores conferidos na documentação na data deste resumo. Distinga limite rígido, quota ajustável e configuração padrão.

| Assunto | Valor ou regra |
|---|---|
| Lambda: duração de uma invocação convencional | **15 minutos** |
| Lambda: memória configurável | **128 a 10.240 MB**, respeitando quotas aplicáveis à conta |
| Lambda: variáveis de ambiente | **4 KB** no agregado |
| Lambda: ZIP descompactado | **250 MB**, incluindo layers e runtime customizado |
| Lambda: imagem de contêiner | **10 GB** descompactada |
| Lambda: payload síncrono convencional | **6 MB** para requisição e **6 MB** para resposta; streaming tem regras diferentes |
| Lambda: payload assíncrono | **1 MB** |
| SQS: tamanho máximo da mensagem | **1 MiB** |
| SQS: retenção | Padrão **4 dias**; máximo **14 dias** |
| SQS: delay | Até **15 minutos** |
| SQS: visibility timeout | Padrão **30 segundos**; máximo **12 horas** desde o recebimento |
| SQS: long polling | Espera de até **20 segundos** |
| SQS FIFO: janela de deduplicação do envio | **5 minutos** |
| DynamoDB: item | Até **400 KB** |
| DynamoDB: unidade de leitura forte convencional | Blocos de **4 KB** |
| DynamoDB: unidade de escrita convencional | Blocos de **1 KB** |
| DynamoDB Streams: retenção de registros | **24 horas** |
| Step Functions Standard | Execuções de até **1 ano** |
| Step Functions Express | Execuções de até **5 minutos** |

As regras acima são documentadas em [Lambda](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-limits.html), [SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/quotas-messages.html), [long polling](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-short-and-long-polling.html), [DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Constraints.html), [DynamoDB Streams](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.html) e [Step Functions](https://docs.aws.amazon.com/step-functions/latest/dg/choosing-workflow-type.html).

**Lambda + SQS:** a recomendação documentada é visibility timeout de pelo menos **6 vezes o timeout da função**, acrescentando a janela de formação de lote quando utilizada. A validação exige que o timeout da função não ultrapasse o da fila; exigência de validação e recomendação de margem não são a mesma coisa. [Configuração de SQS para Lambda](https://docs.aws.amazon.com/lambda/latest/dg/services-sqs-configure.html).

### Pegadinhas que mudam a resposta

1. **Multi-AZ não é Multi-Region.**
2. **Subnet pertence a uma AZ; VPC pertence a uma Region.**
3. **Reserved concurrency não elimina cold start.**
4. **Provisioned concurrency não significa capacidade ilimitada.**
5. **Lambda em subnet pública não recebe internet automaticamente.**
6. **Retries dependem de quem invoca e de como a integração funciona.**
7. **FIFO não substitui idempotência do efeito de negócio.**
8. **Uma fila com vários workers não entrega uma cópia para cada finalidade.**
9. **DLQ de fila, falha de entrega e destino de falha assíncrona não são equivalentes.**
10. **Query usa chave; FilterExpression atua depois da leitura.**
11. **GSI não oferece leitura forte e não significa replicação global.**
12. **TTL não garante exclusão no segundo exato.**
13. **DAX não atende leituras fortes a partir do cache.**
14. **API key não substitui autenticação/autorização.**
15. **CORS não é autorização do backend.**
16. **Autenticar usuário não concede acesso a qualquer tenant.**
17. **Allow adicional não vence Deny explícito.**
18. **Trust policy e permissions policy têm funções diferentes.**
19. **Task role e task execution role são diferentes.**
20. **Permissão S3 não garante permissão KMS.**
21. **Criptografia em repouso não impede vazamento em logs.**
22. **Rotação de chave KMS não é rotação da senha do banco.**
23. **S3 tem consistência forte; caches e replicação têm regras próprias.**
24. **Presigned URL pode expirar antes do prazo configurado quando a sessão expira.**
25. **RDS Proxy gerencia conexões; não é cache de consultas.**
26. **Read replica e standby Multi-AZ de instância têm objetivos distintos.**
27. **Buildspec é build; AppSpec é implantação.**
28. **Canary e linear distribuem o tráfego de formas diferentes.**
29. **Rollback de código não desfaz automaticamente os dados.**
30. **CloudTrail audita ações; X-Ray ajuda a seguir requisições; CloudWatch reúne sinais operacionais.**

### Como ler uma questão

1. Identifique o requisito obrigatório: ordem, latência, durabilidade, autenticação, prazo ou recuperação.
2. Identifique a restrição: menor esforço operacional, menor custo, compatibilidade ou configuração existente.
3. Localize o problema: código, permissão, rede, capacidade, contrato ou implantação.
4. Elimine soluções que não atendem ao requisito, mesmo que sejam serviços conhecidos.
5. Entre opções viáveis, escolha a que melhor respeita a restrição solicitada.

**Exemplo:** “menor esforço operacional” favorece uma integração gerenciada quando ela atende a todos os requisitos. Isso não autoriza ignorar ordem, consistência, segurança ou tempo máximo de execução.

---

## 27. Roteiro de estudo e prática

### Ordem sugerida

| Etapa | Conteúdo | Resultado esperado |
|---|---|---|
| 1 | Regions/AZs, IAM/STS e SDK | Entender onde o recurso existe e quem pode acessá-lo |
| 2 | Lambda e API Gateway | Entender execução, integração, autenticação e falhas |
| 3 | DynamoDB e S3 | Escolher operações e proteger dados corretamente |
| 4 | SQS, SNS, EventBridge, Step Functions e Kinesis | Prever distribuição, ordem, retries e reprocessamento |
| 5 | Cognito, KMS, segredos e multi-tenant | Separar identidade, autorização e proteção de dados |
| 6 | SAM/CloudFormation/CDK, CI/CD e testes | Publicar versões reproduzíveis e reverter com controle |
| 7 | Observabilidade, VPC, bancos, caches e contêineres | Diagnosticar e otimizar a aplicação completa |

### Laboratórios que ajudam a fixar

**1. API com autenticação:** API Gateway + Lambda + DynamoDB, com login e autorização por recurso. Verifique o que acontece com token inválido, acesso proibido e payload incorreto.

**2. Fila com falha controlada:** publique mensagens no SQS, processe com Lambda, provoque uma falha e acompanhe retry, DLQ e resposta parcial de lote.

**3. Upload privado:** gere URL pré-assinada e envie um arquivo diretamente ao S3. Compare permissão, expiração e CORS.

**4. Implantação gradual:** use uma aplicação SAM com versão/alias, publique uma atualização e observe como validar e reverter o tráfego.

**5. Diagnóstico:** adicione logs estruturados, métrica de negócio e tracing; provoque um erro de permissão e uma lentidão de dependência. Identifique a diferença nos sinais.

Ao concluir os laboratórios, remova stacks e recursos que não precisa manter.

### Revisão ativa

Tente explicar, sem consultar:

- Por que uma mensagem foi processada duas vezes?
- Quem é responsável por tentar novamente neste fluxo?
- Qual policy permite ao código acessar o recurso?
- Existe uma segunda autorização em KMS?
- A informação está na Region e no ambiente certos?
- O cache pode devolver um valor antigo ou de outro usuário?
- Qual configuração limita a taxa de trabalho?
- Como identificar a etapa lenta?
- Como publicar e voltar à versão anterior?
- O que um teste unitário não consegue provar neste cenário?

Se consegue justificar as escolhas e explicar por que as alternativas não atendem ao requisito, o estudo está avançando além da memorização de nomes.

---

## 28. Mapa de cobertura do guia

### Todas as tarefas dos quatro domínios

| Tarefa do PDF | Conteúdo abordado aqui | Seções |
|---|---|---|
| **1.1 Desenvolver código para aplicações hospedadas na AWS** | Padrões, resiliência, APIs, SDK, testes, mensageria, streaming, Q e terceiros | 3, 4, 10, 13–16, 22, 24 |
| **1.2 Desenvolver código para Lambda** | VPC, configuração, eventos, erros, testes, integrações e otimização | 9, 13, 16, 19, 22, 23 |
| **1.3 Usar armazenamentos de dados** | Chaves, consistência, Query/Scan, índices, serialização, lifecycle, cache e busca | 3, 11, 12, 17, 18, 24 |
| **2.1 Implementar autenticação/autorização** | Federação, tokens, acesso programático, roles, permissões e comunicação entre serviços | 4–6, 10, 18 |
| **2.2 Implementar criptografia** | Repouso/trânsito, certificados, chaves, cliente/servidor, acesso entre contas e rotação | 7, 12 |
| **2.3 Gerenciar dados sensíveis** | Classificação, variáveis, segredos, sanitização, mascaramento e multi-tenant | 6, 8, 23 |
| **3.1 Preparar artefatos** | Dependências, pacotes, imagens, recursos, repositórios e configuração por ambiente | 8, 9, 18, 20, 21 |
| **3.2 Testar em desenvolvimento** | Testes reais e simulados, endpoints de ambiente, stacks e eventos | 10, 20, 22 |
| **3.3 Automatizar testes de implantação** | Eventos de teste, ambientes, versões aprovadas, IaC e Q | 9, 20–22 |
| **3.4 Implantar por CI/CD** | Pacotes, stages, templates, pipeline, branches/tags, rollout e rollback | 9, 10, 18, 20, 21 |
| **4.1 Auxiliar na análise de causa raiz** | Depuração, logs, métricas, traces, EMF e falhas de integração/implantação | 4, 9–13, 19, 21, 23, 25 |
| **4.2 Instrumentar observabilidade** | Logs estruturados, métricas, anotações, alertas, tracing e health checks | 18, 19, 23 |
| **4.3 Otimizar aplicações** | Concorrência, memória, recursos, filtros, caches e gargalos | 9, 11, 13, 14, 16–19, 23, 25 |

### Serviços listados dentro do escopo no PDF

| Categoria do guia | Serviços e localização no resumo |
|---|---|
| Analytics | Athena (24), Kinesis (16), OpenSearch (24) |
| Integração | AppSync (24), EventBridge (14), SNS (14), SQS (13), Step Functions (15) |
| Computação | EC2 (18), Elastic Beanstalk (21), Lambda (9) |
| Contêineres | ECR, ECS e EKS (18) |
| Banco de dados | Aurora e RDS (17), DynamoDB (11), ElastiCache (17) |
| Ferramentas do desenvolvedor | Amplify (24), CloudShell (4/24), CodeArtifact (21/24), CodeBuild/CodeDeploy/CodePipeline (21), X-Ray (23), Amazon Q Developer (22) |
| Gerenciamento e governança | AppConfig (8), CDK/CloudFormation (20), CloudTrail/CloudWatch (23), CLI (4), Systems Manager (8/24) |
| Redes e entrega | API Gateway (10), CloudFront/ELB/Route 53/VPC (19) |
| Segurança | Cognito (6), IAM/STS (5), KMS (7), Secrets Manager (8), WAF (24) |
| Armazenamento | EBS/EFS (18), S3 (12) |

SAM, Fargate, certificados e outros recursos complementares aparecem onde ajudam a entender as tarefas e integrações. A própria lista de serviços do guia é declarada não exaustiva.

### O que o PDF coloca fora do escopo

Não use o catálogo amplo do SAA-C03 como se todos os serviços tivessem a mesma prioridade para DVA-C02. O PDF lista fora do escopo: QuickSight, Chime, Connect, WorkMail, AppStream 2.0, WorkSpaces, GameLift, Polly, Rekognition, AWS Managed Services, Elastic Transcoder, Application Discovery Service, Application Migration Service, Shield Advanced/Standard, Snow Family e Storage Gateway.

Na seção de revisões, o PDF informa que **Amazon Q Developer foi adicionado** à lista de serviços dentro do escopo e que **AWS Copilot e Amazon CodeGuru foram removidos dessa lista**. Isso é relevante ao comparar com cursos e resumos antigos.

Use as tabelas de cenários e pegadinhas para a revisão rápida e volte às seções dos serviços quando não conseguir explicar o motivo da escolha.
