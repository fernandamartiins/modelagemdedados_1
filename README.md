# Projeto ERP - PetShop Amigo Fiel

Projeto Integrador - Modelagem de Dados | Primeira entrega: do problema real ao Modelo Conceitual de Dados


## 1. Identificação da equipe

| Integrante | Matrícula | Contribuição no projeto |
|---|---|---|
| [Nome do integrante 1] | [matrícula] | [contribuição] |
| [Nome do integrante 2] | [matrícula] | [contribuição] |
| [Nome do integrante 3] | [matrícula] | [contribuição] |
| [Nome do integrante 4] | [matrícula] | [contribuição] |

Disciplina: Projeto Integrador - Modelagem de Dados | Primeira entrega | Data: 30/09/2026


## 2. Caracterização da empresa

A PetShop Amigo Fiel é um pet shop de pequeno porte que combina duas frentes: serviços de estética animal (banho e tosa, com serviço opcional de busca e entrega do animal, o 'táxi dog') e uma loja de produtos (rações, brinquedos, acessórios e itens de higiene). O público são tutores de cães e gatos da região, em sua maioria clientes recorrentes que levam o animal todo mês.

A empresa possui atendimento ao cliente, agenda de serviços, transporte de animais, vendas, controle de estoque, compras de fornecedores e controle financeiro. Atualmente a agenda fica em caderno e mensagens de WhatsApp, os cadastros de clientes e produtos em planilhas diferentes, o estoque é contado manualmente e o caixa é fechado em anotações. Isso dificulta acompanhar vendas, estoque e resultado.

| Setor | O que faz | Informações que gerencia |
|---|---|---|
| Recepção / atendimento | Cadastra tutores e animais, agenda serviços, registra vendas | Clientes, animais, agendamentos, vendas |
| Banho e tosa | Executa os serviços nos animais | Atendimentos e serviços executados |
| Transporte (táxi dog) | Busca e entrega os animais | Transportes, endereços, motoristas, valores |
| Loja | Vende produtos no balcão | Produtos, itens de venda, preços |
| Estoque e compras | Controla lotes e repõe com fornecedores | Lotes, validades, fornecedores, compras |
| Financeiro | Controla contas e caixa | Contas a receber/pagar, movimentações de caixa |

Informações importantes para o negócio: dados do tutor e do animal (inclusive saúde e vacinação), histórico de serviços, agenda, preços praticados, saldo de estoque por lote e validade, custos de compra, contas em aberto e fluxo de caixa.

Observação: os dados desta empresa (rotina, valores e limites) são premissas definidas pela equipe para o projeto e devem ser validadas/ajustadas conforme a realidade do negócio escolhido.


## 3. Justificativa da escolha

O pet shop foi escolhido por reunir, em uma empresa pequena, processos de naturezas diferentes que precisam trocar informação entre si, o que o torna adequado para modelagem de dados e para um ERP:

- Processos analisáveis: agendamento, atendimento, transporte, venda, compra, estoque e financeiro têm início, fim e regras claras.
- Problemas de organização: agenda, cadastros, estoque e caixa estão em fontes separadas, gerando duplicidade, erro e retrabalho.
- Necessidade de integração: uma única venda afeta atendimento, estoque (por lote), contas a receber e caixa; uma compra afeta lotes, contas a pagar e caixa.
- Aplicabilidade de ERP: combina módulos típicos (serviços, vendas, estoque, compras e financeiro) em escala pequena e compreensível.
- Riqueza de modelagem: possui N:N com atributos (serviços no atendimento, itens de venda, baixa por lote), entidades opcionais (transporte) e controle de validade.


## 4. Problemas identificados

| Problema | Consequência | Requisitos que respondem |
|---|---|---|
| Agenda mantida em caderno e WhatsApp | Conflito de horários, esquecimentos e dificuldade de saber quem faltou | RF05, RF06, RF07 |
| Cadastro de clientes e animais em planilhas diferentes | Duplicidade de clientes e dados de animais desatualizados | RF01, RF02 |
| Histórico do animal não é registrado de forma organizada | Impossível consultar serviços anteriores e restrições de saúde | RF09, RF23 |
| Transporte combinado verbalmente | Falhas de rota, sem controle de motorista nem cobrança do valor | RF10 |
| Venda anotada em caderno, sem ligação com o estoque | Estoque desatualizado e venda de produto sem saldo | RF13, RF14, RF15 |
| Estoque controlado manualmente, sem lotes | Produtos vencidos, perdas e erros de quantidade | RF15, RF18, RF24 |
| Compras de fornecedores sem registro formal | Desconhecimento do custo real e dificuldade para reposição | RF16, RF17 |
| Contas a receber e a pagar em anotações separadas | Atrasos, esquecimento de vencimentos e cobrança falha | RF19, RF20, RF21 |
| Caixa fechado manualmente ao fim do dia | Divergências de valores e falta de visão do resultado | RF21, RF22, RF25 |
| Dados do negócio espalhados em várias fontes | Relatórios demorados e decisões sem base em dados | RF25 |


## 5. Processos de negócio


### P1 - Cadastro de cliente e animal

| Aspecto | Descrição |
|---|---|
| Quem participa | Cliente (tutor) e atendente |
| O que inicia | Primeiro contato do tutor ou novo animal a ser atendido |
| O que acontece | O atendente cadastra o tutor e vincula cada animal a ele, registrando dados de contato e de saúde |
| Informação gerada | Cliente e animal |
| Resultado | Tutor e animal aptos a agendar serviços e realizar compras |


### P2 - Agendamento

| Aspecto | Descrição |
|---|---|
| Quem participa | Cliente, atendente |
| O que inicia | Solicitação de horário pelo cliente |
| O que acontece | O atendente registra o agendamento (animal, serviço, data e hora). No dia, verifica se o cliente compareceu. Se não, cancela ou reagenda com motivo |
| Informação gerada | Agendamento e seu status |
| Resultado | Horário reservado, cancelado ou reagendado |


### P3 - Atendimento e transporte

| Aspecto | Descrição |
|---|---|
| Quem participa | Atendente, banhista/tosador, motorista, cliente |
| O que inicia | Comparecimento do cliente ao agendamento |
| O que acontece | Gera-se o atendimento. Se necessário, registra-se o transporte (busca/entrega). O serviço é executado e os serviços e valores são registrados |
| Informação gerada | Atendimento, serviços executados e transporte |
| Resultado | Serviço prestado e pronto para cobrança |


### P4 - Venda

| Aspecto | Descrição |
|---|---|
| Quem participa | Cliente, atendente |
| O que inicia | Fim do atendimento ou compra de produtos no balcão |
| O que acontece | Registra-se a venda e incluem-se os itens. O sistema verifica o estoque. Se há saldo, baixa por lote e gera a conta a receber |
| Informação gerada | Venda, itens, baixa de lote e conta a receber |
| Resultado | Venda concluída e valor a receber registrado |


### P5 - Estoque e compras

| Aspecto | Descrição |
|---|---|
| Quem participa | Gerente/comprador e fornecedor |
| O que inicia | Estoque insuficiente para uma venda ou abaixo do mínimo |
| O que acontece | Registra-se a compra ao fornecedor, lançam-se os itens e cria-se um lote por item. Gera-se a conta a pagar |
| Informação gerada | Compra, itens, lotes e conta a pagar |
| Resultado | Estoque reposto com rastreabilidade de lote |


### P6 - Financeiro e caixa

| Aspecto | Descrição |
|---|---|
| Quem participa | Financeiro/gerente |
| O que inicia | Recebimento de uma venda ou pagamento de uma compra |
| O que acontece | Registra-se a movimentação de caixa (entrada ou saída) vinculada à conta receber/pagar e atualiza-se o status da conta |
| Informação gerada | Movimentação de caixa |
| Resultado | Caixa e contas atualizados; base para o fluxo de caixa |


## 6. Requisitos funcionais

| ID | Requisito | Processo |
|---|---|---|
| RF01 | O sistema deverá cadastrar, consultar e atualizar clientes. | P1 |
| RF02 | O sistema deverá cadastrar animais vinculados a um cliente, com espécie, raça, porte e informações de saúde. | P1 |
| RF03 | O sistema deverá cadastrar funcionários e definir o perfil de acesso de cada um. | Todos |
| RF04 | O sistema deverá manter o catálogo de serviços com preço base e duração estimada. | P2, P3 |
| RF05 | O sistema deverá registrar agendamentos informando animal, serviço, data e hora e o funcionário que registrou. | P2 |
| RF06 | O sistema deverá permitir consultar a agenda por dia, status e serviço. | P2 |
| RF07 | O sistema deverá permitir cancelar ou reagendar um agendamento, registrando o motivo. | P2 |
| RF08 | O sistema deverá gerar o atendimento a partir de um agendamento quando o cliente comparecer. | P3 |
| RF09 | O sistema deverá registrar os serviços executados no atendimento, com quantidade e valor cobrado. | P3 |
| RF10 | O sistema deverá registrar o transporte (busca/entrega) de um atendimento, com motorista, endereços e valor. | P3 |
| RF11 | O sistema deverá cadastrar produtos com categoria, preço de venda e estoque mínimo. | P4, P5 |
| RF12 | O sistema deverá registrar vendas vinculadas a um cliente e a um funcionário. | P4 |
| RF13 | O sistema deverá incluir itens na venda com quantidade, preço unitário e desconto. | P4 |
| RF14 | O sistema deverá verificar se há estoque suficiente para os itens da venda. | P4, P5 |
| RF15 | O sistema deverá baixar o estoque por lote ao concluir a venda, priorizando a menor validade. | P4, P5 |
| RF16 | O sistema deverá cadastrar fornecedores. | P5 |
| RF17 | O sistema deverá registrar compras de fornecedores com itens, quantidades e custo unitário. | P5 |
| RF18 | O sistema deverá criar um lote, com validade e quantidade, para cada item comprado. | P5 |
| RF19 | O sistema deverá gerar contas a receber (parcelas) a partir da venda. | P4, P6 |
| RF20 | O sistema deverá gerar contas a pagar (parcelas) a partir da compra. | P5, P6 |
| RF21 | O sistema deverá registrar a movimentação de caixa e atualizar a conta a receber ou a pagar correspondente. | P6 |
| RF22 | O sistema deverá registrar movimentações de caixa avulsas (sangria e suprimento). | P6 |
| RF23 | O sistema deverá permitir consultar o histórico de atendimentos e compras de um cliente e de um animal. | P3, P4 |
| RF24 | O sistema deverá alertar sobre produtos abaixo do estoque mínimo e lotes próximos do vencimento. | P5 |
| RF25 | O sistema deverá emitir relatórios de vendas por período, estoque atual, contas em aberto e fluxo de caixa. | P4, P5, P6 |


## 7. Requisitos não funcionais

| ID | Categoria | Requisito |
|---|---|---|
| RNF01 | Segurança | O sistema deverá controlar o acesso por perfil (gerente, atendente, banhista/tosador, motorista e financeiro). |
| RNF02 | Auditoria | O sistema deverá registrar quem realizou e quando, em vendas, cancelamentos, baixas de estoque e movimentações de caixa. |
| RNF03 | Usabilidade | As telas de agendamento e venda deverão permitir operação por funcionários sem treinamento técnico, com poucos passos. |
| RNF04 | Desempenho | As consultas operacionais (agenda, cliente, produto) deverão responder em até 3 segundos. |
| RNF05 | Disponibilidade | O sistema deverá estar disponível durante o horário de funcionamento da loja. |
| RNF06 | Integridade | A conclusão da venda (itens, baixa de lote e conta a receber) deverá ocorrer de forma atômica: tudo é registrado ou nada é. |
| RNF07 | Confiabilidade | O sistema deverá possuir cópia de segurança (backup) diária dos dados. |
| RNF08 | Privacidade (LGPD) | Os dados pessoais de clientes e funcionários deverão ter acesso restrito e uso limitado às finalidades do negócio. |
| RNF09 | Padronização | CPF, CNPJ, telefone e CEP deverão ser validados e padronizados no cadastro. |
| RNF10 | Extensibilidade | O modelo de dados deverá permitir incluir novos serviços, categorias e formas de pagamento sem reestruturação. |
| RNF11 | Portabilidade | O sistema deverá ser acessível por navegador em computador e tablet. |


## 8. Regras de negócio

| ID | Regra | Entidades envolvidas |
|---|---|---|
| RN01 | Um cliente pode possuir vários animais; um cliente pode estar cadastrado sem animal. | Cliente, Animal |
| RN02 | O CPF do cliente deve ser único no cadastro. | Cliente |
| RN03 | Todo animal pertence a exatamente um cliente (tutor); não existe animal sem tutor. | Animal, Cliente |
| RN04 | Um agendamento refere-se a um único animal e a um serviço principal. | Agendamento, Animal, Serviço |
| RN05 | Todo agendamento é registrado por um funcionário. | Agendamento, Funcionário |
| RN06 | O agendamento nasce com status Agendado e pode passar para Confirmado, Reagendado, Cancelado ou Realizado. | Agendamento |
| RN07 | Se o cliente não comparecer, o agendamento é cancelado ou reagendado com motivo obrigatório e nenhum atendimento é gerado. | Agendamento |
| RN08 | O atendimento só é gerado a partir de um agendamento com comparecimento; cada agendamento gera no máximo um atendimento. | Agendamento, Atendimento |
| RN09 | Todo atendimento possui um funcionário responsável. | Atendimento, Funcionário |
| RN10 | Um atendimento deve incluir ao menos um serviço; um serviço pode estar em vários atendimentos; o valor cobrado é registrado em cada ocorrência, preservando o histórico. | Atendimento, Serviço |
| RN11 | O transporte existe apenas vinculado a um atendimento, e cada atendimento possui no máximo um transporte (busca, entrega ou ambos). | Transporte, Atendimento |
| RN12 | Todo transporte possui um motorista (funcionário), endereço de origem e endereço de destino. | Transporte, Funcionário |
| RN13 | Toda venda é realizada por exatamente um cliente e registrada por exatamente um funcionário. | Venda, Cliente, Funcionário |
| RN14 | Uma venda pode se originar de um atendimento (cobrança do serviço) ou ser de balcão; cada atendimento origina no máximo uma venda. | Venda, Atendimento |
| RN15 | Uma venda deve conter ao menos um item de produto ou estar vinculada a um atendimento. | Venda, Item de venda |
| RN16 | Um produto aparece uma única vez por venda (a quantidade ajusta o total) e a quantidade deve ser maior que zero. | Item de venda, Produto |
| RN17 | Antes da baixa, o sistema verifica se a soma dos saldos dos lotes do produto atende à quantidade do item. | Item de venda, Lote, Produto |
| RN18 | Se o estoque for insuficiente, deve ser registrada uma compra ao fornecedor e a venda fica Aguardando reposição até a entrada do lote, quando a baixa é efetuada. | Venda, Compra, Lote |
| RN19 | A baixa de estoque é feita por lote, começando pelo de menor validade; um item pode consumir vários lotes e um lote pode atender vários itens. | Item de venda, Lote |
| RN20 | O saldo de um lote nunca pode ser negativo; lotes vencidos não podem ser baixados em vendas. | Lote |
| RN21 | Toda compra possui um fornecedor, um funcionário responsável e ao menos um item. | Compra, Fornecedor, Funcionário |
| RN22 | Cada item comprado gera exatamente um lote com a quantidade comprada; produtos perecíveis exigem data de validade. | Item de compra, Lote |
| RN23 | Um produto pode ser comprado de vários fornecedores ao longo do tempo; não há vínculo fixo entre produto e fornecedor. | Produto, Fornecedor, Compra |
| RN24 | Toda venda concluída gera ao menos uma conta a receber (parcelas) e a soma das parcelas é igual ao valor total da venda. | Venda, Conta a receber |
| RN25 | Toda compra recebida gera ao menos uma conta a pagar e a soma das parcelas é igual ao valor total da compra. | Compra, Conta a pagar |
| RN26 | Cada recebimento ou pagamento gera uma movimentação de caixa vinculada a exatamente uma conta (a receber ou a pagar); apenas movimentos avulsos não têm conta. | Movimento de caixa, Contas |
| RN27 | Conta paga e movimentação de caixa não podem ser alteradas ou excluídas; correções são feitas por estorno (novo registro). | Contas, Movimento de caixa |
| RN28 | Venda concluída não é excluída, apenas cancelada, com estorno do estoque e do financeiro. | Venda |
| RN29 | Todo registro de venda, compra, agendamento e movimentação identifica o funcionário autor. | Funcionário |
| RN30 | Valor total da venda = soma dos itens (quantidade x preço - desconto do item) + serviços e transporte do atendimento vinculado - desconto total. | Venda, Item de venda, Atendimento |
| RN31 | Cliente, animal, produto, fornecedor e funcionário com histórico não são excluídos; são marcados como inativos. | Cadastros em geral |


## 9. Restrições e políticas organizacionais

| ID | Tema | Política / restrição | Responsável |
|---|---|---|---|
| POL01 | Desconto | Atendente pode conceder até 5% de desconto; acima disso exige aprovação do gerente. | Gerente |
| POL02 | Cancelamento de agendamento | Cancelamentos devem ser avisados com ao menos 2 horas de antecedência; faltas sem aviso são registradas. | Atendente |
| POL03 | Cancelamento de venda | Apenas o gerente pode cancelar venda já concluída. | Gerente |
| POL04 | Preços | Somente gerente ou financeiro altera preço de produtos e serviços; o histórico fica nos itens e atendimentos. | Gerente |
| POL05 | Pagamento | Formas aceitas: dinheiro, PIX, débito e crédito. Parcelamento apenas no crédito, em até 3 parcelas. | Financeiro |
| POL06 | Aprovação de compras | Compras acima de R$ 1.000,00 exigem aprovação do gerente. | Gerente |
| POL07 | Estoque | Produto abaixo do estoque mínimo gera alerta; lote vencido fica bloqueado para venda. | Gerente |
| POL08 | Acesso à informação | Contas e caixa são acessados apenas por gerente e financeiro; o motorista visualiza apenas transportes. | Gerente |
| POL09 | Transporte | O transporte atende raio de até 10 km e o valor é definido por faixa de distância. | Gerente |
| POL10 | Saúde do animal | Animais sem vacinação em dia não são aceitos para banho e tosa. | Atendente |
| POL11 | Proteção de dados | Dados pessoais só podem ser consultados por funcionários autorizados para atendimento ou cobrança. | Gerente |


## 10. Fluxogramas

O fluxograma abaixo representa o processo integrado do pet shop, do cadastro ao fechamento financeiro, ligando os processos P1 a P6.

![Figura 1 - Fluxograma integrado do pet shop](imagens/fluxograma_petshop.png)

*Figura 1 - Fluxograma integrado do pet shop*

Leitura do fluxo: o cliente e o animal são cadastrados e o agendamento é registrado. Se o cliente não comparece, o agendamento é cancelado ou reagendado. Se comparece, gera-se o atendimento, registra-se o transporte quando necessário e executa-se o serviço. Em seguida registra-se a venda e incluem-se os itens. Se há estoque, baixa-se por lote e gera-se a conta a receber. Se não há, registra-se a compra ao fornecedor, lançam-se os itens, cria-se o lote e gera-se a conta a pagar. Ambos os ramos terminam na movimentação de caixa. Pela RN18, a venda fica Aguardando reposição até a entrada do lote, quando a baixa por lote é feita.


### Coerência do fluxograma com requisitos e regras

| Etapa do fluxograma | Requisitos | Regras / políticas |
|---|---|---|
| Cadastrar cliente e animal | RF01, RF02 | RN01-RN03 |
| Registrar agendamento | RF05, RF06 | RN04-RN06; POL10 |
| Cliente compareceu? (Não) / Cancelar ou reagendar | RF07 | RN07; POL02 |
| Cliente compareceu? (Sim) / Gerar atendimento | RF08 | RN08, RN09 |
| Precisa de transporte? / Registrar transporte | RF10 | RN11, RN12; POL09 |
| Executar serviço | RF09 | RN10 |
| Registrar venda / Incluir itens da venda | RF12, RF13 | RN13-RN16, RN30; POL01 |
| Estoque suficiente? | RF14 | RN17 |
| Sim: Baixar estoque (por lote) | RF15 | RN19, RN20 |
| Sim: Gerar conta a receber | RF19 | RN24; POL05 |
| Não: Registrar compra (fornecedor) | RF16, RF17 | RN18, RN21, RN23; POL06 |
| Não: Lançar itens e criar lote | RF18 | RN22 |
| Não: Gerar conta a pagar | RF20 | RN25 |
| Registrar movimentação de caixa | RF21 | RN26, RN27 |


### Integração entre processos

- P1 alimenta P2: o agendamento exige um animal já cadastrado (R02).
- P2 alimenta P3: o atendimento nasce do agendamento com comparecimento (R05).
- P3 alimenta P4: o atendimento origina a venda que cobra serviços e transporte (R10).
- P4 e P5 se comunicam pelo estoque: a venda baixa lotes (R15) e a falta de saldo dispara a compra, que cria novos lotes (R20).
- P4 e P5 alimentam P6: venda gera conta a receber (R21), compra gera conta a pagar (R22) e ambas geram movimentação de caixa (R23, R24).


## 11. Entidades

Cada entidade foi identificada a partir dos substantivos dos requisitos e das regras, mantendo apenas o que precisa existir como informação no banco. ITEM_VENDA e ITEM_COMPRA são entidades associativas.

| Entidade | Por que existe | Origem (processo / requisito / regra) |
|---|---|---|
| CLIENTE | Tutor que contrata serviços e compra produtos; é o responsável financeiro pelas operações. | P1, P4; RF01; RN01, RN02, RN13 |
| ANIMAL | Pet que recebe os serviços. Possui dados próprios (espécie, porte, saúde) e um mesmo cliente pode ter vários animais. | P1, P2; RF02; RN01, RN03 |
| FUNCIONARIO | Pessoa que opera o negócio (atende, executa serviços, conduz transporte, vende, compra e movimenta o caixa). Permite rastrear autoria das operações. | Todos os processos; RF03; RN05, RN09, RN29 |
| SERVICO | Catálogo dos serviços oferecidos (banho, tosa etc.). Existe separado do atendimento porque é reutilizado em vários agendamentos e atendimentos. | P2, P3; RF04; RN04, RN10 |
| AGENDAMENTO | Reserva de horário feita antes do comparecimento. Separado de Atendimento porque pode ser cancelado, reagendado ou não gerar atendimento. | P2; RF05-RF07; RN04-RN07 |
| ATENDIMENTO | Execução real do serviço quando o cliente comparece. Reúne serviços executados, responsável e eventual transporte. | P3; RF08, RF09; RN08-RN10 |
| TRANSPORTE | Serviço de busca e/ou entrega do animal. Tem motorista, endereços e valor próprios e só ocorre em alguns atendimentos. | P3; RF10; RN11, RN12; POL09 |
| PRODUTO | Item de loja vendido ao cliente (ração, brinquedo, higiene, acessórios). Base do controle de estoque. | P4, P5; RF11; RN16, RN17 |
| FORNECEDOR | Empresa de quem o pet shop compra produtos para reposição. | P5; RF16; RN21, RN23 |
| COMPRA | Pedido de reposição feito a um fornecedor. Dá origem aos lotes e à conta a pagar. | P5; RF17, RF20; RN18, RN21, RN25 |
| ITEM_COMPRA | Entidade associativa de Compra e Produto. Guarda quantidade e custo de cada produto comprado e origina um lote. | P5; RF17, RF18; RN21, RN22 |
| LOTE | Conjunto de unidades de um produto recebido em uma compra, com validade própria. Permite baixa por lote e rastreabilidade. | P5; RF15, RF18; RN19, RN20, RN22 |
| VENDA | Transação comercial com o cliente, que pode cobrar serviços de um atendimento e/ou produtos. Origem do contas a receber. | P4; RF12, RF13; RN13-RN16, RN30 |
| ITEM_VENDA | Entidade associativa de Venda e Produto. Guarda quantidade, preço praticado e desconto, e é o que consome lotes na baixa de estoque. | P4, P5; RF13-RF15; RN16, RN17, RN19 |
| CONTA_RECEBER | Valor a receber do cliente (por parcela) decorrente de uma venda. | P6; RF19, RF21; RN24, RN26, RN27 |
| CONTA_PAGAR | Valor a pagar ao fornecedor (por parcela) decorrente de uma compra. | P6; RF20, RF21; RN25, RN26, RN27 |
| MOVIMENTO_CAIXA | Registro de cada entrada ou saída de dinheiro. Consolida os efeitos financeiros das vendas e compras e permite o fluxo de caixa. | P6; RF21, RF22; RN26, RN27 |


## 12. Atributos

Classificação: Identificador (sublinhado no DER), Simples, Composto (comp.), Multivalorado ({ }) e Derivado (/ em itálico). O detalhamento de cada atributo está na seção 15.

| Entidade | Atributos |
|---|---|
| CLIENTE | id_cliente, nome, cpf, telefone, email, endereco, data_cadastro, ativo |
| ANIMAL | id_animal, nome, especie, raca, porte, data_nascimento, /idade, vacinacao_em_dia, observacoes_saude, ativo |
| FUNCIONARIO | id_funcionario, nome, cpf, cargo, telefone, data_admissao, perfil_acesso, ativo |
| SERVICO | id_servico, nome, descricao, preco_base, duracao_estimada_min, ativo |
| AGENDAMENTO | id_agendamento, data_hora_agendada, status, motivo_cancelamento, observacoes, data_registro |
| ATENDIMENTO | id_atendimento, data_hora_inicio, data_hora_fim, status, observacoes, /valor_total_servicos |
| TRANSPORTE | id_transporte, tipo, endereco_origem, endereco_destino, data_hora_prevista, valor, status |
| PRODUTO | id_produto, nome, descricao, categoria, unidade_medida, preco_venda, estoque_minimo, /estoque_atual, ativo |
| FORNECEDOR | id_fornecedor, razao_social, nome_fantasia, cnpj, telefone, email, endereco, ativo |
| COMPRA | id_compra, data_compra, status, numero_nota_fiscal, /valor_total, observacoes |
| ITEM_COMPRA | (compra, produto), quantidade, custo_unitario |
| LOTE | id_lote, codigo_lote, data_entrada, data_validade, quantidade_inicial, /quantidade_atual |
| VENDA | id_venda, data_hora_venda, status, desconto_total, /valor_total, observacoes |
| ITEM_VENDA | (venda, produto), quantidade, preco_unitario, desconto_item |
| CONTA_RECEBER | id_conta_receber, numero_parcela, valor, data_emissao, data_vencimento, data_pagamento, forma_pagamento, status |
| CONTA_PAGAR | id_conta_pagar, numero_parcela, valor, data_emissao, data_vencimento, data_pagamento, status |
| MOVIMENTO_CAIXA | id_movimento, data_hora, tipo, valor, forma_pagamento, descricao |

Atributos de relacionamento: INCLUI (Atendimento - Serviço): quantidade, valor_cobrado. BAIXA (Item de venda - Lote): quantidade_baixada, data_hora_baixa.


## 13. Relacionamentos

| ID | Relacionamento | Regra |
|---|---|---|
| R01 | CLIENTE POSSUI ANIMAL | RN01, RN03 |
| R02 | ANIMAL TEM AGENDAMENTO | RN04 |
| R03 | FUNCIONARIO REGISTRA AGENDAMENTO | RN05 |
| R04 | SERVICO É PREVISTO EM AGENDAMENTO | RN04 |
| R05 | AGENDAMENTO GERA ATENDIMENTO | RN07, RN08 |
| R06 | FUNCIONARIO EXECUTA ATENDIMENTO | RN09 |
| R07 | ATENDIMENTO INCLUI SERVICO | RN10 (N:N com atributos) |
| R08 | ATENDIMENTO REQUER TRANSPORTE | RN11 |
| R09 | FUNCIONARIO CONDUZ TRANSPORTE | RN12 |
| R10 | ATENDIMENTO ORIGINA VENDA | RN14 |
| R11 | CLIENTE REALIZA VENDA | RN13 |
| R12 | FUNCIONARIO REGISTRA VENDA | RN13, RN29 |
| R13 | VENDA COMPÕE ITEM_VENDA | RN15, RN16 |
| R14 | PRODUTO É VENDIDO EM ITEM_VENDA | RN16 |
| R15 | ITEM_VENDA BAIXA LOTE | RN18, RN19 (N:N com atributos) |
| R16 | FORNECEDOR FORNECE COMPRA | RN21 |
| R17 | FUNCIONARIO REGISTRA COMPRA | RN21, RN29 |
| R18 | COMPRA COMPÕE ITEM_COMPRA | RN21 |
| R19 | PRODUTO É COMPRADO EM ITEM_COMPRA | RN21, RN23 |
| R20 | ITEM_COMPRA GERA LOTE | RN22 |
| R21 | VENDA GERA CONTA_RECEBER | RN24 |
| R22 | COMPRA GERA CONTA_PAGAR | RN25 |
| R23 | CONTA_RECEBER GERA MOVIMENTO_CAIXA | RN26 |
| R24 | CONTA_PAGAR GERA MOVIMENTO_CAIXA | RN26 |
| R25 | FUNCIONARIO REGISTRA MOVIMENTO_CAIXA | RN26, RN29 |


## 14. Cardinalidades

Notação (min,max) ao lado da entidade: quantas vezes uma ocorrência dessa entidade participa do relacionamento. Cada relacionamento foi analisado nos dois sentidos (método 'vá e volte').

| ID | Relacionamento | Ida | Volta |
|---|---|---|---|
| R01 | CLIENTE (0,N) POSSUI ANIMAL (1,1) | Quantos animais um cliente possui? De 0 a N (pode estar cadastrado sem animal). | Quantos clientes um animal tem? Exatamente 1 (tutor responsável). |
| R02 | ANIMAL (0,N) TEM AGENDAMENTO (1,1) | Quantos agendamentos um animal tem? De 0 a N. | De quantos animais é um agendamento? Exatamente 1. |
| R03 | FUNCIONARIO (0,N) REGISTRA AGENDAMENTO (1,1) | Quantos agendamentos um funcionário registra? De 0 a N. | Quantos funcionários registram um agendamento? Exatamente 1. |
| R04 | SERVICO (0,N) É PREVISTO EM AGENDAMENTO (1,1) | Em quantos agendamentos um serviço é previsto? De 0 a N. | Quantos serviços principais tem um agendamento? Exatamente 1. |
| R05 | AGENDAMENTO (0,1) GERA ATENDIMENTO (1,1) | Quantos atendimentos um agendamento gera? De 0 a 1 (0 se o cliente faltou). | De quantos agendamentos nasce um atendimento? Exatamente 1. |
| R06 | FUNCIONARIO (0,N) EXECUTA ATENDIMENTO (1,1) | Quantos atendimentos um funcionário executa? De 0 a N. | Quantos funcionários respondem por um atendimento? Exatamente 1. |
| R07 | ATENDIMENTO (1,N) INCLUI SERVICO (0,N) | Quantos serviços um atendimento inclui? De 1 a N. | Em quantos atendimentos um serviço é executado? De 0 a N. |
| R08 | ATENDIMENTO (0,1) REQUER TRANSPORTE (1,1) | Quantos transportes um atendimento requer? De 0 a 1. | A quantos atendimentos pertence um transporte? Exatamente 1. |
| R09 | FUNCIONARIO (0,N) CONDUZ TRANSPORTE (1,1) | Quantos transportes um funcionário conduz? De 0 a N. | Quantos motoristas tem um transporte? Exatamente 1. |
| R10 | ATENDIMENTO (0,1) ORIGINA VENDA (0,1) | Quantas vendas um atendimento origina? De 0 a 1 (0 se ainda não cobrado). | De quantos atendimentos vem uma venda? De 0 a 1 (0 se for venda de balcão). |
| R11 | CLIENTE (0,N) REALIZA VENDA (1,1) | Quantas vendas um cliente realiza? De 0 a N. | Quantos clientes realizam uma venda? Exatamente 1. |
| R12 | FUNCIONARIO (0,N) REGISTRA VENDA (1,1) | Quantas vendas um funcionário registra? De 0 a N. | Quantos funcionários registram uma venda? Exatamente 1. |
| R13 | VENDA (0,N) COMPÕE ITEM_VENDA (1,1) | Quantos itens tem uma venda? De 0 a N (0 se só cobra serviço). | A quantas vendas pertence um item? Exatamente 1. |
| R14 | PRODUTO (0,N) É VENDIDO EM ITEM_VENDA (1,1) | Em quantos itens de venda um produto aparece? De 0 a N. | Quantos produtos tem um item de venda? Exatamente 1. |
| R15 | ITEM_VENDA (0,N) BAIXA LOTE (0,N) | De quantos lotes um item consome unidades? De 0 a N (0 enquanto aguarda reposição). | Para quantos itens de venda um lote fornece unidades? De 0 a N. |
| R16 | FORNECEDOR (0,N) FORNECE COMPRA (1,1) | Quantas compras um fornecedor atende? De 0 a N. | Quantos fornecedores tem uma compra? Exatamente 1. |
| R17 | FUNCIONARIO (0,N) REGISTRA COMPRA (1,1) | Quantas compras um funcionário registra? De 0 a N. | Quantos funcionários registram uma compra? Exatamente 1. |
| R18 | COMPRA (1,N) COMPÕE ITEM_COMPRA (1,1) | Quantos itens tem uma compra? De 1 a N. | A quantas compras pertence um item? Exatamente 1. |
| R19 | PRODUTO (0,N) É COMPRADO EM ITEM_COMPRA (1,1) | Em quantos itens de compra um produto aparece? De 0 a N. | Quantos produtos tem um item de compra? Exatamente 1. |
| R20 | ITEM_COMPRA (1,1) GERA LOTE (1,1) | Quantos lotes um item de compra gera? Exatamente 1. | De quantos itens de compra vem um lote? Exatamente 1. |
| R21 | VENDA (0,N) GERA CONTA_RECEBER (1,1) | Quantas contas (parcelas) uma venda gera? De 0 a N (0 até a baixa de estoque). | A quantas vendas pertence uma conta a receber? Exatamente 1. |
| R22 | COMPRA (0,N) GERA CONTA_PAGAR (1,1) | Quantas contas (parcelas) uma compra gera? De 0 a N (0 até o lançamento dos itens). | A quantas compras pertence uma conta a pagar? Exatamente 1. |
| R23 | CONTA_RECEBER (0,N) GERA MOVIMENTO_CAIXA (0,1) | Quantas movimentações uma conta a receber gera? De 0 a N (recebimentos parciais). | De quantas contas a receber vem uma movimentação? De 0 a 1 (0 se avulsa). |
| R24 | CONTA_PAGAR (0,N) GERA MOVIMENTO_CAIXA (0,1) | Quantas movimentações uma conta a pagar gera? De 0 a N (pagamentos parciais). | De quantas contas a pagar vem uma movimentação? De 0 a 1 (0 se avulsa). |
| R25 | FUNCIONARIO (0,N) REGISTRA MOVIMENTO_CAIXA (1,1) | Quantas movimentações um funcionário registra? De 0 a N. | Quantos funcionários registram uma movimentação? Exatamente 1. |


### 14.1 Verificação de relacionamentos N:N e atributos de relacionamento

| Par | A para B é N? | B para A é N? | Possui atributos próprios? | Decisão |
|---|---|---|---|---|
| Atendimento x Serviço | Sim | Sim | Sim: quantidade, valor_cobrado | N:N mantido como relacionamento INCLUI com atributos próprios (RN10). |
| Venda x Produto | Sim | Sim | Sim: quantidade, preço, desconto | N:N promovido à entidade associativa ITEM_VENDA, pois participa de outro relacionamento (BAIXA com Lote). |
| Compra x Produto | Sim | Sim | Sim: quantidade, custo unitário | N:N promovido à entidade associativa ITEM_COMPRA, pois origina o Lote. |
| Item de venda x Lote | Sim | Sim | Sim: quantidade_baixada, data_hora_baixa | N:N mantido como relacionamento BAIXA com atributos (RN19). |
| Produto x Fornecedor | Sim | Sim | Não | N:N existe de fato, mas é representado pelas Compras (RN23); sem relacionamento direto para evitar redundância. |
| Cliente x Animal | Sim (vários animais) | Não (um tutor) | - | Relacionamento 1:N (RN03); tutores múltiplos estão fora do escopo. |
| Agendamento x Serviço | - | Não (um serviço principal) | - | Relacionamento 1:N (RN04); serviços adicionais entram no Atendimento. |


## 15. Dicionário de dados conceitual

Legenda: Obrig. = preenchimento obrigatório. Os identificadores são lógicos e serão detalhados (tipos, tamanhos, chaves) nas etapas seguintes.


### Entidade: CLIENTE

| Atributo | Descrição | Classificação | Obrig. | Regra / observação |
|---|---|---|---|---|
| id_cliente | Identificador do cliente | Identificador | Sim | Identificação única, gerada pelo sistema |
| nome | Nome completo do cliente | Simples | Sim |  |
| cpf | CPF do cliente | Simples | Sim | Único e válido (RN02) |
| telefone | Telefones de contato | Multivalorado | Sim | Ao menos um; usado para confirmar agendamentos |
| email | E-mail do cliente | Simples | Não | Pode ser informado posteriormente |
| endereco | Logradouro, número, bairro, cidade e CEP | Composto | Não | Obrigatório quando houver transporte (RN12) |
| data_cadastro | Data em que o cliente foi cadastrado | Simples | Sim | Preenchida automaticamente |
| ativo | Situação do cadastro | Simples | Sim | Cadastro com histórico é inativado, não excluído (RN31) |


### Entidade: ANIMAL

| Atributo | Descrição | Classificação | Obrig. | Regra / observação |
|---|---|---|---|---|
| id_animal | Identificador do animal | Identificador | Sim | Identificação única |
| nome | Nome do animal | Simples | Sim |  |
| especie | Espécie (cão, gato etc.) | Simples | Sim |  |
| raca | Raça do animal | Simples | Não | Pode ser SRD / desconhecida |
| porte | Pequeno, médio ou grande | Simples | Sim | Pode influenciar duração e preço do serviço |
| data_nascimento | Data de nascimento (ou estimada) | Simples | Não |  |
| idade | Idade atual do animal | Derivado | Não | Calculada a partir de data_nascimento |
| vacinacao_em_dia | Indica se a vacinação está em dia | Simples | Sim | Verificada no agendamento (POL10) |
| observacoes_saude | Alergias, restrições e cuidados | Simples | Não |  |
| ativo | Situação do cadastro | Simples | Sim | RN31 |


### Entidade: FUNCIONARIO

| Atributo | Descrição | Classificação | Obrig. | Regra / observação |
|---|---|---|---|---|
| id_funcionario | Identificador do funcionário | Identificador | Sim | Identificação única |
| nome | Nome completo | Simples | Sim |  |
| cpf | CPF do funcionário | Simples | Sim | Único e válido |
| cargo | Atendente, banhista, tosador, motorista, gerente | Simples | Sim |  |
| telefone | Telefone de contato | Simples | Sim |  |
| data_admissao | Data de admissão | Simples | Sim |  |
| perfil_acesso | Perfil de permissões no sistema | Simples | Sim | Base do controle de acesso (RNF01, POL08) |
| ativo | Situação do cadastro | Simples | Sim | RN31 |


### Entidade: SERVICO

| Atributo | Descrição | Classificação | Obrig. | Regra / observação |
|---|---|---|---|---|
| id_servico | Identificador do serviço | Identificador | Sim | Identificação única |
| nome | Nome do serviço | Simples | Sim | Ex.: banho, tosa, banho e tosa |
| descricao | Descrição do que está incluso | Simples | Não |  |
| preco_base | Preço de tabela do serviço | Simples | Sim | Maior ou igual a zero; alteração só pelo gerente (POL04) |
| duracao_estimada_min | Duração estimada em minutos | Simples | Sim | Usada para ocupar a agenda |
| ativo | Serviço disponível para novos agendamentos | Simples | Sim |  |


### Entidade: AGENDAMENTO

| Atributo | Descrição | Classificação | Obrig. | Regra / observação |
|---|---|---|---|---|
| id_agendamento | Identificador do agendamento | Identificador | Sim | Identificação única |
| data_hora_agendada | Data e hora marcadas | Simples | Sim |  |
| status | Agendado, confirmado, reagendado, cancelado ou realizado | Simples | Sim | RN06 |
| motivo_cancelamento | Motivo do cancelamento/reagendamento | Simples | Não | Obrigatório se cancelado ou reagendado (RN07) |
| observacoes | Observações do agendamento | Simples | Não |  |
| data_registro | Data/hora em que foi registrado | Simples | Sim | Preenchida automaticamente |


### Entidade: ATENDIMENTO

| Atributo | Descrição | Classificação | Obrig. | Regra / observação |
|---|---|---|---|---|
| id_atendimento | Identificador do atendimento | Identificador | Sim | Identificação única |
| data_hora_inicio | Início do atendimento | Simples | Sim |  |
| data_hora_fim | Fim do atendimento | Simples | Não | Preenchida na conclusão |
| status | Em andamento, concluído ou cancelado | Simples | Sim |  |
| observacoes | Observações durante o atendimento | Simples | Não |  |
| valor_total_servicos | Total dos serviços executados | Derivado | Não | Soma de quantidade x valor_cobrado |


### Entidade: TRANSPORTE

| Atributo | Descrição | Classificação | Obrig. | Regra / observação |
|---|---|---|---|---|
| id_transporte | Identificador do transporte | Identificador | Sim | Identificação única |
| tipo | Busca, entrega ou busca e entrega | Simples | Sim |  |
| endereco_origem | Endereço de origem | Composto | Sim |  |
| endereco_destino | Endereço de destino | Composto | Sim |  |
| data_hora_prevista | Data e hora previstas | Simples | Sim |  |
| valor | Valor cobrado pelo transporte | Simples | Sim | Conforme faixa de distância (POL09) |
| status | Agendado, em rota ou concluído | Simples | Sim |  |


### Entidade: PRODUTO

| Atributo | Descrição | Classificação | Obrig. | Regra / observação |
|---|---|---|---|---|
| id_produto | Identificador do produto | Identificador | Sim | Identificação única |
| nome | Nome do produto | Simples | Sim |  |
| descricao | Descrição / marca / apresentação | Simples | Não |  |
| categoria | Ração, brinquedo, higiene, acessório etc. | Simples | Sim |  |
| unidade_medida | Unidade (un, kg, pacote) | Simples | Sim |  |
| preco_venda | Preço de venda atual | Simples | Sim | Maior ou igual a zero; alteração só pelo gerente (POL04) |
| estoque_minimo | Quantidade mínima desejada em estoque | Simples | Sim | Base do alerta de reposição (RF24, POL07) |
| estoque_atual | Quantidade disponível | Derivado | Não | Soma da quantidade_atual dos lotes do produto |
| ativo | Produto disponível para venda | Simples | Sim |  |


### Entidade: FORNECEDOR

| Atributo | Descrição | Classificação | Obrig. | Regra / observação |
|---|---|---|---|---|
| id_fornecedor | Identificador do fornecedor | Identificador | Sim | Identificação única |
| razao_social | Razão social | Simples | Sim |  |
| nome_fantasia | Nome fantasia | Simples | Não |  |
| cnpj | CNPJ do fornecedor | Simples | Sim | Único e válido |
| telefone | Telefones de contato | Multivalorado | Sim | Ao menos um |
| email | E-mail comercial | Simples | Não |  |
| endereco | Endereço do fornecedor | Composto | Não |  |
| ativo | Situação do cadastro | Simples | Sim | RN31 |


### Entidade: COMPRA

| Atributo | Descrição | Classificação | Obrig. | Regra / observação |
|---|---|---|---|---|
| id_compra | Identificador da compra | Identificador | Sim | Identificação única |
| data_compra | Data da compra | Simples | Sim |  |
| status | Aberta, recebida ou cancelada | Simples | Sim |  |
| numero_nota_fiscal | Número da nota do fornecedor | Simples | Não |  |
| valor_total | Valor total da compra | Derivado | Não | Soma dos itens da compra |
| observacoes | Observações | Simples | Não |  |


### Entidade: ITEM_COMPRA

| Atributo | Descrição | Classificação | Obrig. | Regra / observação |
|---|---|---|---|---|
| (compra, produto) | Identificação composta pelas entidades relacionadas | Identificador | Sim | Um produto aparece uma única vez por compra |
| quantidade | Quantidade comprada | Simples | Sim | Maior que zero |
| custo_unitario | Custo unitário praticado na compra | Simples | Sim | Mantém o histórico de custo |


### Entidade: LOTE

| Atributo | Descrição | Classificação | Obrig. | Regra / observação |
|---|---|---|---|---|
| id_lote | Identificador do lote | Identificador | Sim | Identificação única |
| codigo_lote | Código do lote informado pelo fabricante | Simples | Sim |  |
| data_entrada | Data de entrada no estoque | Simples | Sim |  |
| data_validade | Data de validade | Simples | Não | Obrigatória para produtos perecíveis (RN22) |
| quantidade_inicial | Quantidade recebida | Simples | Sim | Igual à quantidade do item comprado |
| quantidade_atual | Saldo atual do lote | Derivado | Não | Inicial menos baixas; nunca negativa (RN20) |


### Entidade: VENDA

| Atributo | Descrição | Classificação | Obrig. | Regra / observação |
|---|---|---|---|---|
| id_venda | Identificador da venda | Identificador | Sim | Identificação única |
| data_hora_venda | Data e hora da venda | Simples | Sim |  |
| status | Aberta, aguardando reposição, concluída ou cancelada | Simples | Sim | RN18, RN28 |
| desconto_total | Desconto concedido sobre a venda | Simples | Não | Limites por perfil (POL01) |
| valor_total | Valor final da venda | Derivado | Não | Calculado conforme RN30 |
| observacoes | Observações | Simples | Não |  |


### Entidade: ITEM_VENDA

| Atributo | Descrição | Classificação | Obrig. | Regra / observação |
|---|---|---|---|---|
| (venda, produto) | Identificação composta pelas entidades relacionadas | Identificador | Sim | Um produto aparece uma única vez por venda |
| quantidade | Quantidade vendida | Simples | Sim | Maior que zero |
| preco_unitario | Preço unitário no momento da venda | Simples | Sim | Preserva o histórico de preços |
| desconto_item | Desconto aplicado ao item | Simples | Não | POL01 |


### Entidade: CONTA_RECEBER

| Atributo | Descrição | Classificação | Obrig. | Regra / observação |
|---|---|---|---|---|
| id_conta_receber | Identificador da conta a receber | Identificador | Sim | Identificação única |
| numero_parcela | Número da parcela | Simples | Sim | 1 se pagamento à vista |
| valor | Valor da parcela | Simples | Sim | Maior que zero; soma das parcelas = valor da venda |
| data_emissao | Data de emissão | Simples | Sim |  |
| data_vencimento | Data de vencimento | Simples | Sim |  |
| data_pagamento | Data em que foi recebida | Simples | Não | Preenchida na quitação |
| forma_pagamento | Dinheiro, PIX, débito ou crédito | Simples | Sim | POL05 |
| status | Aberta, paga, vencida ou cancelada | Simples | Sim | RN27 |


### Entidade: CONTA_PAGAR

| Atributo | Descrição | Classificação | Obrig. | Regra / observação |
|---|---|---|---|---|
| id_conta_pagar | Identificador da conta a pagar | Identificador | Sim | Identificação única |
| numero_parcela | Número da parcela | Simples | Sim |  |
| valor | Valor da parcela | Simples | Sim | Maior que zero; soma das parcelas = valor da compra |
| data_emissao | Data de emissão | Simples | Sim |  |
| data_vencimento | Data de vencimento | Simples | Sim |  |
| data_pagamento | Data em que foi paga | Simples | Não | Preenchida na quitação |
| status | Aberta, paga, vencida ou cancelada | Simples | Sim | RN27 |


### Entidade: MOVIMENTO_CAIXA

| Atributo | Descrição | Classificação | Obrig. | Regra / observação |
|---|---|---|---|---|
| id_movimento | Identificador da movimentação | Identificador | Sim | Identificação única |
| data_hora | Data e hora da movimentação | Simples | Sim |  |
| tipo | Entrada ou saída | Simples | Sim |  |
| valor | Valor movimentado | Simples | Sim | Maior que zero |
| forma_pagamento | Forma de pagamento utilizada | Simples | Sim | POL05 |
| descricao | Descrição da movimentação | Simples | Não | Obrigatória em movimentos avulsos (sangria/suprimento) |


### Relacionamento R07: INCLUI (Atendimento - Serviço)

| Atributo | Descrição | Classificação | Obrig. | Regra / observação |
|---|---|---|---|---|
| quantidade | Quantas vezes o serviço foi executado no atendimento | Simples | Sim | Maior que zero |
| valor_cobrado | Valor praticado naquela execução | Simples | Sim | Pode diferir do preco_base; preserva o histórico |


### Relacionamento R15: BAIXA (Item de venda - Lote)

| Atributo | Descrição | Classificação | Obrig. | Regra / observação |
|---|---|---|---|---|
| quantidade_baixada | Quantidade retirada do lote para o item | Simples | Sim | Maior que zero; não excede o saldo do lote |
| data_hora_baixa | Momento da baixa | Simples | Sim | Preenchida automaticamente |


## 16. DER

Legenda: retângulo azul = entidade; laranja = entidade associativa; losango = relacionamento; caixa tracejada = atributos do relacionamento; sublinhado = identificador; /itálico = derivado; {x} = multivalorado; (comp.) = composto. Cardinalidades em vermelho junto à entidade.

![Figura 2 - Diagrama Entidade-Relacionamento (DER) conceitual. Arquivos em imagens/der_petshop.png e .svg](imagens/der_petshop.png)

*Figura 2 - Diagrama Entidade-Relacionamento (DER) conceitual. Arquivos em imagens/der_petshop.png e .svg*


## 17. Justificativas técnicas

| Decisão | Por que | Regra que sustenta |
|---|---|---|
| Animal é entidade separada de Cliente. | Animal tem dados próprios (espécie, porte, saúde, vacinação) e um cliente pode ter vários animais. Guardá-lo dentro de Cliente repetiria dados do cliente. | RN01, RN03 |
| Agendamento e Atendimento são entidades distintas, com cardinalidade 0,1 do lado do agendamento. | O agendamento é uma intenção; o atendimento é a execução. O fluxograma mostra que o cliente pode faltar, e nesse caso não existe atendimento. | RN07, RN08 |
| Atendimento x Serviço é N:N com atributos quantidade e valor_cobrado no relacionamento. | Um atendimento pode ter vários serviços e um serviço aparece em muitos atendimentos. O valor cobrado descreve aquela ocorrência (não o serviço nem o atendimento) e preserva o histórico se o preço mudar. | RN10 |
| Transporte é entidade própria, ligada a Atendimento com 0,1 e 1,1. | Nem todo atendimento precisa de transporte (decisão do fluxograma). Transporte tem motorista, endereços, tipo e valor próprios. | RN11, RN12 |
| Venda x Produto foi resolvido com a entidade associativa ITEM_VENDA. | É um N:N com atributos, mas o item participa de outro relacionamento (baixa de lotes). Como relacionamento não se liga a outro relacionamento no DER, o item vira entidade. | RN16, RN19 |
| Existe a entidade LOTE e o relacionamento BAIXA (N:N) com atributo quantidade_baixada. | O fluxograma exige baixar estoque por lote. Um item pode consumir vários lotes e um lote atende vários itens; a quantidade retirada pertence à relação entre os dois. | RN19, RN20 |
| estoque_atual e quantidade_atual são atributos derivados. | O saldo pode ser calculado a partir dos lotes e baixas. Armazená-lo como dado independente criaria risco de divergência. | RN17, RN20 |
| Não há relacionamento direto entre Produto e Fornecedor. | O vínculo é circunstancial: o fornecedor aparece nas compras. Um relacionamento direto duplicaria a informação e poderia contradizer o histórico. | RN23 |
| ITEM_COMPRA gera exatamente um LOTE (1,1 nos dois lados). | O fluxograma diz lançar itens e criar lote. Cada item recebido forma um lote com validade e quantidade próprias. | RN22 |
| CONTA_RECEBER e CONTA_PAGAR são entidades separadas, com cardinalidade 0,N a partir de Venda e Compra. | Têm origens, partes e políticas diferentes. A cardinalidade mínima 0 reflete que a conta só nasce após a baixa de estoque (venda) ou o lançamento dos itens (compra). Parcelas justificam o máximo N. | RN24, RN25 |
| MOVIMENTO_CAIXA se liga a CONTA_RECEBER e CONTA_PAGAR por dois relacionamentos, cada um com 0,1 do lado do movimento. | Um movimento quita uma conta de um tipo ou outro, nunca as duas, e movimentos avulsos (sangria) não têm conta. Fica registrada a regra de exclusividade. | RN26 |
| Cliente se liga diretamente à Venda. | A venda de balcão não passa por atendimento nem animal. Sem a ligação direta, não se saberia quem comprou. | RN13, RN14 |
| Funcionário aparece em vários relacionamentos, cada um com significado distinto (registra, executa, conduz). | Cada papel é uma regra diferente e sustenta a auditoria (RNF02). Um único relacionamento genérico perderia essas informações. | RN05, RN09, RN12, RN29 |
| Preço, custo e valor são copiados para itens e atendimentos. | É redundância controlada: o preço do catálogo muda, mas o valor da transação passada não pode mudar. | RN10, RN16 |
| Modelo preparado para evolução. | Atributos como forma_pagamento, status e categoria foram mantidos como atributos simples nesta etapa e podem ser normalizados em tabelas na etapa lógica. Novos serviços, produtos e filiais não exigem mudar a estrutura. | RNF10 |

Exemplo de leitura: foi definida a cardinalidade 0,N para Cliente em POSSUI porque a regra RN01 permite que um cliente esteja cadastrado sem animais e possua vários, e 1,1 para Animal porque a RN03 exige um tutor para cada animal.


## 18. Conclusão

O modelo conceitual parte dos processos do pet shop (cadastro, agendamento, atendimento, venda, compra e financeiro) e foi derivado dos requisitos e das regras de negócio: 17 entidades, 25 relacionamentos, 4 relacionamentos N:N tratados com atributos próprios e cardinalidades justificadas nos dois sentidos. O DER integra serviços, vendas, estoque por lote e financeiro, de modo que uma venda ou compra reflete em todos eles. A estrutura está preparada para a próxima etapa (modelo lógico e normalização), em que os N:N, os atributos multivalorados e compostos e os domínios de status serão tratados.
