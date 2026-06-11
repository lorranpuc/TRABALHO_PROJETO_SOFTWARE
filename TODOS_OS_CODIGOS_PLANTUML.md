# OficinaPro — Códigos PlantUML dos Diagramas

Este arquivo reúne todos os códigos PlantUML gerados para o projeto OficinaPro.

Total de diagramas: 17.


## 01_casos_de_uso.puml

```plantuml
@startuml
title OficinaPro — Diagrama de Casos de Uso
left to right direction

skinparam shadowing false
skinparam packageStyle rectangle
skinparam actorStyle awesome
skinparam usecase {
  BorderColor #333333
  BackgroundColor #F8F8F8
}

actor Cliente
actor Atendente
actor "Mecânico" as Mecanico
actor Gerente
actor Administrador
actor "Sistema de Pagamento" as SistemaPagamento
actor "Sistema de Notificação" as SistemaNotificacao

rectangle "OficinaPro" {
  usecase "UC-01\nCadastrar cliente\ne veículo" as UC01
  usecase "UC-02\nAbrir ordem\nde serviço" as UC02
  usecase "UC-03\nRegistrar\ndiagnóstico" as UC03
  usecase "UC-04\nElaborar\norçamento" as UC04
  usecase "UC-05\nAprovar ou rejeitar\norçamento" as UC05
  usecase "UC-06\nExecutar\nserviço" as UC06
  usecase "UC-07\nAtualizar status\nda ordem" as UC07
  usecase "UC-08\nRegistrar uso\nde peças" as UC08
  usecase "UC-09\nFinalizar ordem\nde serviço" as UC09
  usecase "UC-10\nRegistrar\npagamento" as UC10
  usecase "UC-11\nConsultar histórico\ndo veículo" as UC11
  usecase "UC-12\nGerenciar peças\ne serviços" as UC12
  usecase "UC-13\nEmitir relatórios\ngerenciais" as UC13
}

Atendente --> UC01
Atendente --> UC02
Atendente --> UC04
Atendente --> UC07
Atendente --> UC09
Atendente --> UC10
Atendente --> UC11

Mecanico --> UC03
Mecanico --> UC04
Mecanico --> UC06
Mecanico --> UC07
Mecanico --> UC08
Mecanico --> UC09
Mecanico --> UC11

Cliente --> UC05
Cliente --> UC10

Gerente --> UC11
Gerente --> UC12
Gerente --> UC13

Administrador --> UC12

SistemaPagamento --> UC10
SistemaNotificacao <-- UC05
SistemaNotificacao <-- UC07
SistemaNotificacao <-- UC09

UC02 .> UC01 : <<include>>\nse necessário
UC04 .> UC03 : <<include>>\nquando exige diagnóstico
UC06 .> UC08 : <<include>>\nquando usa peça
UC09 .> UC10 : <<extend>>\napós finalização

note right of UC13
  Acesso restrito a
  Gerente e Administrador
end note

@enduml

```


## 02_sistema_sequence_uc02_abrir_ordem.puml

```plantuml
@startuml
title UC-02 — Diagrama de Sequência do Sistema — Abrir Ordem de Serviço

skinparam shadowing false
skinparam sequenceMessageAlign center
autonumber

actor Atendente
boundary "Sistema OficinaPro" as Sistema
database "Repositórios Internos" as Repos
control "Controle de Regras" as Regras

Atendente -> Sistema : informar cliente, veículo,\ndescrição do problema e prioridade
activate Sistema

Sistema -> Repos : buscarCliente(clienteId)
Repos --> Sistema : cliente encontrado

Sistema -> Repos : buscarVeiculo(veiculoId)
Repos --> Sistema : veículo encontrado

Sistema -> Regras : validarVinculoClienteVeiculo(cliente, veículo)
Regras --> Sistema : vínculo válido

Sistema -> Repos : verificarOrdemAberta(veiculoId)
Repos --> Sistema : resultado da verificação

alt não existe ordem aberta
  Sistema -> Repos : criarOrdemServico(dados)
  Repos --> Sistema : ordem criada
  Sistema -> Repos : registrarHistoricoStatus("Aberta")
  Repos --> Sistema : histórico registrado
  Sistema --> Atendente : exibir número da ordem criada
else existe ordem aberta sem autorização
  Sistema --> Atendente : informar impedimento\npara nova ordem
end

deactivate Sistema
@enduml

```


## 03_sistema_sequence_uc05_aprovar_orcamento.puml

```plantuml
@startuml
title UC-05 — Diagrama de Sequência do Sistema — Aprovar ou Rejeitar Orçamento

skinparam shadowing false
skinparam sequenceMessageAlign center
autonumber

actor Cliente
boundary "Sistema OficinaPro" as Sistema
database "Repositórios Internos" as Repos
control "Controle de Regras" as Regras
participant "Sistema de Notificação" as Notificacao

Cliente -> Sistema : consultar orçamento(orcamentoId)
activate Sistema

Sistema -> Repos : buscarOrcamento(orcamentoId)
Repos --> Sistema : orçamento pendente
Sistema -> Repos : buscarOrdemServico(ordemId)
Repos --> Sistema : ordem vinculada
Sistema --> Cliente : apresentar serviços, peças,\nvalores e validade

Cliente -> Sistema : registrar decisão(aprovado/rejeitado)
Sistema -> Regras : validarOrcamentoPendente(orcamento)
Regras --> Sistema : orçamento válido

alt orçamento aprovado
  Sistema -> Repos : atualizarStatusOrcamento("Aprovado")
  Sistema -> Repos : atualizarStatusOrdem("Aprovada")
  Sistema -> Repos : registrarHistoricoStatus("Aprovada")
  Sistema -> Notificacao : enviarConfirmacaoAprovacao(cliente)
  Notificacao --> Sistema : notificação enviada
  Sistema --> Cliente : informar aprovação registrada
else orçamento rejeitado
  Sistema -> Repos : atualizarStatusOrcamento("Rejeitado")
  Sistema -> Repos : atualizarStatusOrdem("Cancelada")
  Sistema -> Repos : registrarHistoricoStatus("Cancelada")
  Sistema -> Notificacao : enviarConfirmacaoRejeicao(cliente)
  Notificacao --> Sistema : notificação enviada
  Sistema --> Cliente : informar rejeição registrada
end

deactivate Sistema
@enduml

```


## 04_sistema_sequence_uc10_registrar_pagamento.puml

```plantuml
@startuml
title UC-10 — Diagrama de Sequência do Sistema — Registrar Pagamento

skinparam shadowing false
skinparam sequenceMessageAlign center
autonumber

actor Atendente
boundary "Sistema OficinaPro" as Sistema
database "Repositórios Internos" as Repos
participant "Sistema de Pagamento" as GatewayPagamento

Atendente -> Sistema : informar ordem, forma de pagamento,\nvalor e identificador da transação
activate Sistema

Sistema -> Repos : buscarOrdemServico(ordemServicoId)
Repos --> Sistema : ordem finalizada ou liberada

Sistema -> Sistema : validarValorPagamento(valorPago)
Sistema -> GatewayPagamento : confirmarPagamento(dadosPagamento)
activate GatewayPagamento
GatewayPagamento --> Sistema : resultado da confirmação
deactivate GatewayPagamento

alt pagamento confirmado
  Sistema -> Repos : registrarPagamento("Confirmado")
  Sistema -> Repos : atualizarStatusOrdem("Paga")
  Sistema -> Repos : registrarHistoricoStatus("Paga")
  Sistema --> Atendente : informar pagamento confirmado
else pagamento recusado ou pendente
  Sistema -> Repos : registrarPagamento("Recusado/Pendente")
  Sistema --> Atendente : informar pendência ou recusa
end

deactivate Sistema
@enduml

```


## 05_arquitetura.puml

```plantuml
@startuml
title OficinaPro — Diagrama de Arquitetura em Camadas

left to right direction
skinparam shadowing false
skinparam packageStyle rectangle
skinparam linetype ortho

actor "Usuários\nCliente, Atendente,\nMecânico, Gerente,\nAdministrador" as Usuarios

rectangle "OficinaPro" as Sistema {
  package "Camada de Apresentação" as Apresentacao {
    [Interface Web] as UI
  }

  package "Camada de Aplicação" as Aplicacao {
    [Serviços de Aplicação] as AppServices
    [Controle de Autenticação\ne Permissões] as Auth
  }

  package "Camada de Domínio" as Dominio {
    [Entidades de Domínio] as Entidades
    [Regras de Negócio] as Regras
  }

  package "Camada de Persistência" as Persistencia {
    [Repositórios] as Repos
    database "OficinaProDB" as DB
  }
}

cloud "Integrações Externas Simuladas" as Integracoes {
  [Sistema de Pagamento] as PagamentoExt
  [Sistema de Notificação] as NotificacaoExt
}

Usuarios --> UI : acessa
UI --> AppServices : requisita casos de uso
AppServices --> Auth : valida perfil
AppServices --> Entidades : manipula
AppServices --> Regras : aplica
AppServices --> Repos : persiste/consulta
Repos --> DB : SQL
AppServices ..> PagamentoExt : confirma pagamento
AppServices ..> NotificacaoExt : envia mensagens

note bottom of Sistema
  Arquitetura conceitual em camadas.
  O objetivo é separar interface, aplicação,
  domínio, persistência e integrações.
end note

@enduml

```


## 06_componentes.puml

```plantuml
@startuml
title OficinaPro — Diagrama de Componentes

left to right direction
skinparam shadowing false
skinparam packageStyle rectangle
skinparam componentStyle rectangle
skinparam linetype ortho

actor "Usuário" as Usuario

package "Cliente Web" {
  [Interface Web] as Web
}

package "Servidor de Aplicação — OficinaPro" {
  [API OficinaPro] as API
  [Autenticação e Permissões] as Auth
  [Módulo de Clientes e Veículos] as ClientesVeiculos
  [Módulo de Ordem de Serviço] as OrdemServico
  [Módulo de Orçamento] as Orcamento
  [Módulo de Estoque] as Estoque
  [Módulo de Pagamento] as Pagamento
  [Módulo de Relatórios] as Relatorios
  [Serviço de Notificação] as Notificacao
}

database "OficinaProDB" as DB

cloud "Serviços Externos Simulados" {
  [Sistema de Pagamento] as SistemaPagamento
  [Sistema de Notificação] as SistemaNotificacao
}

Usuario --> Web : utiliza
Web --> API : HTTP/JSON
API --> Auth : autentica/autoriza

API --> ClientesVeiculos
API --> OrdemServico
API --> Orcamento
API --> Estoque
API --> Pagamento
API --> Relatorios

OrdemServico --> ClientesVeiculos : consulta cliente/veículo
OrdemServico --> Orcamento : solicita orçamento
OrdemServico --> Estoque : registra peças
OrdemServico --> Notificacao : comunica status

Orcamento --> Estoque : consulta peças
Orcamento --> Notificacao : notifica decisão
Pagamento --> SistemaPagamento : confirma transação
Notificacao --> SistemaNotificacao : envia mensagem

ClientesVeiculos --> DB
OrdemServico --> DB
Orcamento --> DB
Estoque --> DB
Pagamento --> DB
Relatorios --> DB

@enduml

```


## 07_implantacao.puml

```plantuml
@startuml
title OficinaPro — Diagrama de Implantação

left to right direction
skinparam shadowing false
skinparam linetype ortho

node "Cliente / Navegador" as ClienteNode {
  artifact "Interface Web\nHTML/CSS/JS" as WebApp
}

node "Servidor de Aplicação" as AppServer {
  artifact "API OficinaPro" as API
  artifact "Módulos de Negócio" as Modulos
  artifact "Autenticação e Permissões" as Auth
}

database "Servidor de Banco de Dados" as DbServer {
  artifact "OficinaProDB\nBanco Relacional" as DB
}

cloud "Serviços Externos Simulados" as Externos {
  artifact "Sistema de Pagamento" as PagamentoExt
  artifact "Sistema de Notificação" as NotificacaoExt
}

WebApp --> API : HTTPS
API --> Auth : valida sessão/perfil
API --> Modulos : executa casos de uso
Modulos --> DB : SQL
Modulos ..> PagamentoExt : API simulada
Modulos ..> NotificacaoExt : API simulada

note bottom of AppServer
  Implantação acadêmica simplificada:
  navegador, servidor de aplicação,
  banco relacional e integrações simuladas.
end note

@enduml

```


## 08_classes.puml

```plantuml
@startuml
title OficinaPro — Diagrama de Classes

skinparam shadowing false
skinparam linetype ortho
hide empty members

class Cliente {
  - id : Long
  - nome : String
  - documento : String
  - telefone : String
  - email : String
  - endereco : String
  + atualizarContato(telefone, email)
}

class Veiculo {
  - id : Long
  - placa : String
  - marca : String
  - modelo : String
  - ano : Integer
  - cor : String
  + atualizarDados()
}

class OrdemServico {
  - id : Long
  - numero : String
  - dataAbertura : Date
  - dataFechamento : Date
  - descricaoProblema : String
  - prioridade : String
  - status : StatusOrdem
  + abrir()
  + alterarStatus(novoStatus)
  + calcularValorFinal() : Decimal
  + finalizar()
}

class Diagnostico {
  - id : Long
  - descricao : String
  - dataRegistro : DateTime
  + registrarDescricao(descricao)
}

class Orcamento {
  - id : Long
  - valorTotal : Decimal
  - validade : Date
  - status : StatusOrcamento
  - dataCriacao : DateTime
  + calcularTotal() : Decimal
  + aprovar()
  + rejeitar()
  + estaValido() : Boolean
}

class Servico {
  - id : Long
  - nome : String
  - descricao : String
  - valorBase : Decimal
  - ativo : Boolean
  + inativar()
}

class ItemServico {
  - id : Long
  - quantidade : Integer
  - valorUnitario : Decimal
  - subtotal : Decimal
  + calcularSubtotal() : Decimal
}

class Peca {
  - id : Long
  - codigo : String
  - nome : String
  - quantidadeEstoque : Integer
  - valorUnitario : Decimal
  - estoqueMinimo : Integer
  - ativo : Boolean
  + baixarEstoque(quantidade)
  + possuiEstoque(quantidade) : Boolean
}

class ItemPeca {
  - id : Long
  - quantidade : Integer
  - valorUnitario : Decimal
  - subtotal : Decimal
  + calcularSubtotal() : Decimal
}

class Pagamento {
  - id : Long
  - formaPagamento : FormaPagamento
  - valorPago : Decimal
  - status : StatusPagamento
  - dataPagamento : DateTime
  - identificadorTransacao : String
  + confirmar()
  + recusar()
}

class Usuario {
  - id : Long
  - nome : String
  - email : String
  - senhaHash : String
  - perfil : PerfilUsuario
  - ativo : Boolean
  + possuiPermissao(acao) : Boolean
}

class HistoricoStatus {
  - id : Long
  - statusAnterior : StatusOrdem
  - statusNovo : StatusOrdem
  - dataAlteracao : DateTime
  - observacao : String
}

enum StatusOrdem {
  Aberta
  EmDiagnostico
  AguardandoOrcamento
  AguardandoAprovacao
  Aprovada
  EmExecucao
  ServicoConcluido
  AguardandoPagamento
  Paga
  Entregue
  Cancelada
}

enum StatusOrcamento {
  EmElaboracao
  PendenteAprovacao
  Aprovado
  Rejeitado
  Expirado
}

enum StatusPagamento {
  Pendente
  EmProcessamento
  Confirmado
  Recusado
  Estornado
}

enum PerfilUsuario {
  Atendente
  Mecanico
  Gerente
  Administrador
}

enum FormaPagamento {
  Dinheiro
  Pix
  CartaoCredito
  CartaoDebito
}

Cliente "1" -- "0..*" Veiculo : possui
Veiculo "1" -- "0..*" OrdemServico : gera

OrdemServico "1" *-- "0..1" Diagnostico : diagnóstico
OrdemServico "1" *-- "0..1" Orcamento : orçamento
OrdemServico "1" *-- "0..*" ItemServico : serviços executados
OrdemServico "1" *-- "0..*" ItemPeca : peças utilizadas
OrdemServico "1" o-- "0..*" Pagamento : pagamentos
OrdemServico "1" *-- "0..*" HistoricoStatus : histórico

Orcamento "1" *-- "0..*" ItemServico : serviços orçados
Orcamento "1" *-- "0..*" ItemPeca : peças orçadas

Servico "1" <-- "0..*" ItemServico : referencia
Peca "1" <-- "0..*" ItemPeca : referencia

Usuario "1" -- "0..*" Diagnostico : registra
Usuario "1" -- "0..*" HistoricoStatus : altera
Usuario --> PerfilUsuario

OrdemServico --> StatusOrdem
Orcamento --> StatusOrcamento
Pagamento --> StatusPagamento
Pagamento --> FormaPagamento

note bottom of ItemServico
  ItemServico representa tanto serviço orçado
  quanto serviço efetivamente executado,
  conforme o vínculo com Orcamento ou OrdemServico.
end note

note bottom of ItemPeca
  ItemPeca representa tanto peça orçada
  quanto peça efetivamente utilizada.
end note

@enduml

```


## 09_projeto_sequence_uc02_abrir_ordem.puml

```plantuml
@startuml
title UC-02 — Diagrama de Sequência de Projeto — Abrir Ordem de Serviço

skinparam shadowing false
skinparam sequenceMessageAlign center
autonumber

actor Atendente
boundary "TelaOrdemServico" as Tela
control "OrdemServicoController" as Controller
control "OrdemServicoService" as Service
database "ClienteRepository" as ClienteRepo
database "VeiculoRepository" as VeiculoRepo
database "OrdemServicoRepository" as OrdemRepo
database "HistoricoStatusRepository" as HistoricoRepo
entity "OrdemServico" as Ordem

Atendente -> Tela : preencher dados da ordem
Tela -> Controller : abrirOrdemServico(dados)
activate Controller

Controller -> Service : abrirOrdemServico(clienteId, veiculoId,\ndescricaoProblema, prioridade)
activate Service

Service -> ClienteRepo : findById(clienteId)
ClienteRepo --> Service : Cliente

Service -> VeiculoRepo : findById(veiculoId)
VeiculoRepo --> Service : Veiculo

Service -> OrdemRepo : existeOrdemAberta(veiculoId)
OrdemRepo --> Service : false

create Ordem
Service -> Ordem : criar(veiculo, descricao, prioridade)
Ordem --> Service : ordem com status Aberta

Service -> OrdemRepo : salvar(ordem)
OrdemRepo --> Service : ordem salva

Service -> HistoricoRepo : salvar(status inicial "Aberta")
HistoricoRepo --> Service : histórico salvo

Service --> Controller : dados da ordem criada
deactivate Service

Controller --> Tela : resposta de sucesso
deactivate Controller
Tela --> Atendente : exibir número da ordem

@enduml

```


## 10_projeto_sequence_uc04_uc05_orcamento_aprovacao.puml

```plantuml
@startuml
title UC-04/UC-05 — Diagrama de Sequência de Projeto — Orçamento e Aprovação

skinparam shadowing false
skinparam sequenceMessageAlign center
autonumber

actor "Atendente/Mecânico" as UsuarioInterno
actor Cliente
boundary "TelaOrcamento" as Tela
control "OrcamentoController" as Controller
control "OrcamentoService" as Service
database "OrdemServicoRepository" as OrdemRepo
database "OrcamentoRepository" as OrcamentoRepo
entity "OrdemServico" as Ordem
entity "Orcamento" as Orcamento
control "NotificacaoService" as Notificacao

UsuarioInterno -> Tela : informar serviços, peças e validade
Tela -> Controller : gerarOrcamento(ordemId, itens, validade)
activate Controller

Controller -> Service : gerarOrcamento(ordemId, itens, validade)
activate Service

Service -> OrdemRepo : findById(ordemId)
OrdemRepo --> Service : ordem

Service -> Service : validarDiagnosticoOuServicoTabelado()
create Orcamento
Service -> Orcamento : criar(ordem, itens, validade)
Orcamento -> Orcamento : calcularTotal()
Orcamento --> Service : orçamento pendente

Service -> OrcamentoRepo : salvar(orcamento)
OrcamentoRepo --> Service : orçamento salvo

Service -> Ordem : alterarStatus("Aguardando aprovação")
Service -> OrdemRepo : salvar(ordem)
Service -> Notificacao : notificarOrcamentoDisponivel(cliente, orçamento)

Service --> Controller : orçamento gerado
deactivate Service
Controller --> Tela : exibir orçamento
deactivate Controller

Cliente -> Tela : aprovar ou rejeitar orçamento
Tela -> Controller : registrarDecisao(orcamentoId, decisao)
activate Controller
Controller -> Service : registrarDecisao(orcamentoId, clienteId, decisao)
activate Service

Service -> OrcamentoRepo : findById(orcamentoId)
OrcamentoRepo --> Service : orçamento pendente

alt cliente aprova
  Service -> Orcamento : aprovar()
  Service -> Ordem : alterarStatus("Aprovada")
  Service -> Notificacao : notificarOrcamentoAprovado(cliente)
else cliente rejeita
  Service -> Orcamento : rejeitar()
  Service -> Ordem : alterarStatus("Cancelada")
  Service -> Notificacao : notificarOrcamentoRejeitado(cliente)
end

Service -> OrcamentoRepo : salvar(orcamento)
Service -> OrdemRepo : salvar(ordem)
Service --> Controller : decisão registrada
deactivate Service
Controller --> Tela : exibir resultado
deactivate Controller
Tela --> Cliente : confirmar decisão

@enduml

```


## 11_projeto_sequence_uc10_registrar_pagamento.puml

```plantuml
@startuml
title UC-10 — Diagrama de Sequência de Projeto — Registrar Pagamento

skinparam shadowing false
skinparam sequenceMessageAlign center
autonumber

actor Atendente
boundary "TelaPagamento" as Tela
control "PagamentoController" as Controller
control "PagamentoService" as Service
participant "GatewayPagamento" as Gateway
database "PagamentoRepository" as PagamentoRepo
database "OrdemServicoRepository" as OrdemRepo
database "HistoricoStatusRepository" as HistoricoRepo
entity "Pagamento" as Pagamento
entity "OrdemServico" as Ordem

Atendente -> Tela : informar dados do pagamento
Tela -> Controller : registrarPagamento(ordemId, forma, valor, transacao)
activate Controller

Controller -> Service : registrarPagamento(ordemId, forma, valor, transacao)
activate Service

Service -> OrdemRepo : findById(ordemId)
OrdemRepo --> Service : ordem

Service -> Service : validarOrdemLiberadaParaPagamento()
Service -> Gateway : confirmarPagamento(forma, valor, transacao)
Gateway --> Service : confirmação/recusa

create Pagamento
Service -> Pagamento : criar(ordem, forma, valor, status)

alt confirmado
  Service -> Ordem : alterarStatus("Paga")
  Service -> Pagamento : confirmar()
  Service -> PagamentoRepo : salvar(pagamento)
  Service -> OrdemRepo : salvar(ordem)
  Service -> HistoricoRepo : salvar(status "Paga")
  Service --> Controller : pagamento confirmado
else recusado ou pendente
  Service -> Pagamento : recusar()
  Service -> PagamentoRepo : salvar(pagamento)
  Service --> Controller : pagamento não confirmado
end

deactivate Service
Controller --> Tela : apresentar resultado
deactivate Controller
Tela --> Atendente : exibir status do pagamento

@enduml

```


## 12_comunicacao_uc02_abrir_ordem.puml

```plantuml
@startuml
title UC-02 — Diagrama de Comunicação — Abrir Ordem de Serviço

left to right direction
skinparam shadowing false
skinparam linetype ortho

object "atendente:Atendente" as Atendente
object "tela:TelaOrdemServico" as Tela
object "controller:OrdemServicoController" as Controller
object "service:OrdemServicoService" as Service
object "clienteRepo:ClienteRepository" as ClienteRepo
object "veiculoRepo:VeiculoRepository" as VeiculoRepo
object "ordemRepo:OrdemServicoRepository" as OrdemRepo
object "historicoRepo:HistoricoStatusRepository" as HistoricoRepo
object "ordem:OrdemServico" as Ordem

Atendente --> Tela : 1. preencherDados()
Tela --> Controller : 2. abrirOrdemServico(dados)
Controller --> Service : 3. abrirOrdemServico(...)
Service --> ClienteRepo : 3.1 buscarCliente()
Service --> VeiculoRepo : 3.2 buscarVeiculo()
Service --> OrdemRepo : 3.3 verificarOrdemAberta()
Service --> Ordem : 3.4 criarOrdem(status Aberta)
Service --> OrdemRepo : 3.5 salvar(ordem)
Service --> HistoricoRepo : 3.6 registrarStatusInicial()
Service --> Controller : 4. retornarOrdemCriada()
Controller --> Tela : 5. exibirResultado()
Tela --> Atendente : 6. informarNumeroDaOrdem()

@enduml

```


## 13_comunicacao_uc05_aprovar_orcamento.puml

```plantuml
@startuml
title UC-05 — Diagrama de Comunicação — Aprovar Orçamento

left to right direction
skinparam shadowing false
skinparam linetype ortho

object "cliente:Cliente" as Cliente
object "tela:TelaOrcamento" as Tela
object "controller:OrcamentoController" as Controller
object "service:OrcamentoService" as Service
object "orcamentoRepo:OrcamentoRepository" as OrcamentoRepo
object "ordemRepo:OrdemServicoRepository" as OrdemRepo
object "orcamento:Orcamento" as Orcamento
object "ordem:OrdemServico" as Ordem
object "notificacao:NotificacaoService" as Notificacao

Cliente --> Tela : 1. escolherAprovarOuRejeitar()
Tela --> Controller : 2. registrarDecisao(orcamentoId, decisao)
Controller --> Service : 3. registrarDecisao(...)
Service --> OrcamentoRepo : 3.1 buscarOrcamento()
Service --> OrdemRepo : 3.2 buscarOrdem()
Service --> Orcamento : 3.3 aprovar() ou rejeitar()
Service --> Ordem : 3.4 alterarStatus()
Service --> OrcamentoRepo : 3.5 salvar(orcamento)
Service --> OrdemRepo : 3.6 salvar(ordem)
Service --> Notificacao : 3.7 enviarNotificacao(cliente)
Service --> Controller : 4. retornarResultado()
Controller --> Tela : 5. exibirConfirmacao()
Tela --> Cliente : 6. informarDecisaoRegistrada()

@enduml

```


## 14_comunicacao_uc10_registrar_pagamento.puml

```plantuml
@startuml
title UC-10 — Diagrama de Comunicação — Registrar Pagamento

left to right direction
skinparam shadowing false
skinparam linetype ortho

object "atendente:Atendente" as Atendente
object "tela:TelaPagamento" as Tela
object "controller:PagamentoController" as Controller
object "service:PagamentoService" as Service
object "gateway:GatewayPagamento" as Gateway
object "pagamentoRepo:PagamentoRepository" as PagamentoRepo
object "ordemRepo:OrdemServicoRepository" as OrdemRepo
object "historicoRepo:HistoricoStatusRepository" as HistoricoRepo
object "pagamento:Pagamento" as Pagamento
object "ordem:OrdemServico" as Ordem

Atendente --> Tela : 1. informarPagamento()
Tela --> Controller : 2. registrarPagamento(dados)
Controller --> Service : 3. registrarPagamento(...)
Service --> OrdemRepo : 3.1 buscarOrdem()
Service --> Gateway : 3.2 confirmarPagamento()
Gateway --> Service : 3.3 retornarResultado()
Service --> Pagamento : 3.4 criarPagamento(status)
Service --> PagamentoRepo : 3.5 salvar(pagamento)
Service --> Ordem : 3.6 alterarStatus("Paga")
Service --> OrdemRepo : 3.7 salvar(ordem)
Service --> HistoricoRepo : 3.8 registrarStatus("Paga")
Service --> Controller : 4. retornarResultado()
Controller --> Tela : 5. exibirStatus()
Tela --> Atendente : 6. informarConfirmação()

@enduml

```


## 15_estados_ordem_servico.puml

```plantuml
@startuml
title OficinaPro — Diagrama de Estados — Ordem de Serviço

skinparam shadowing false

[*] --> Aberta : abrir ordem

state "Aberta" as Aberta
state "Em diagnóstico" as EmDiagnostico
state "Aguardando orçamento" as AguardandoOrcamento
state "Aguardando aprovação" as AguardandoAprovacao
state "Aprovada" as Aprovada
state "Em execução" as EmExecucao
state "Serviço concluído" as ServicoConcluido
state "Aguardando pagamento" as AguardandoPagamento
state "Paga" as Paga
state "Entregue" as Entregue
state "Cancelada" as Cancelada

Aberta --> EmDiagnostico : encaminhar para análise
Aberta --> AguardandoOrcamento : serviço simples tabelado
EmDiagnostico --> AguardandoOrcamento : registrar diagnóstico
AguardandoOrcamento --> AguardandoAprovacao : gerar orçamento
AguardandoAprovacao --> Aprovada : cliente aprova
AguardandoAprovacao --> Cancelada : cliente rejeita
Aprovada --> EmExecucao : iniciar serviço
EmExecucao --> ServicoConcluido : concluir execução
ServicoConcluido --> AguardandoPagamento : liberar pagamento
AguardandoPagamento --> Paga : pagamento confirmado
Paga --> Entregue : entregar veículo

Aberta --> Cancelada : cancelamento administrativo
EmDiagnostico --> Cancelada : cancelamento administrativo
AguardandoOrcamento --> Cancelada : cancelamento administrativo
Aprovada --> Cancelada : cancelamento autorizado

Entregue --> [*]
Cancelada --> [*]

note right of AguardandoAprovacao
  A execução só pode iniciar
  após aprovação do orçamento.
end note

@enduml

```


## 16_estados_orcamento_pagamento.puml

```plantuml
@startuml
title OficinaPro — Diagrama de Estados — Orçamento e Pagamento

skinparam shadowing false

state "Ciclo do Orçamento" as CicloOrcamento {
  [*] --> ORC_Elaboracao : iniciar orçamento

  state "Em elaboração" as ORC_Elaboracao
  state "Pendente de aprovação" as ORC_Pendente
  state "Aprovado" as ORC_Aprovado
  state "Rejeitado" as ORC_Rejeitado
  state "Expirado" as ORC_Expirado

  ORC_Elaboracao --> ORC_Pendente : gerar orçamento
  ORC_Pendente --> ORC_Aprovado : cliente aprova
  ORC_Pendente --> ORC_Rejeitado : cliente rejeita
  ORC_Pendente --> ORC_Expirado : validade encerrada

  ORC_Aprovado --> [*]
  ORC_Rejeitado --> [*]
  ORC_Expirado --> [*]
}

state "Ciclo do Pagamento" as CicloPagamento {
  [*] --> PAY_Pendente : liberar pagamento

  state "Pendente" as PAY_Pendente
  state "Em processamento" as PAY_Processando
  state "Confirmado" as PAY_Confirmado
  state "Recusado" as PAY_Recusado
  state "Estornado" as PAY_Estornado

  PAY_Pendente --> PAY_Processando : enviar para validação
  PAY_Processando --> PAY_Confirmado : pagamento aceito
  PAY_Processando --> PAY_Recusado : pagamento recusado
  PAY_Confirmado --> PAY_Estornado : estornar pagamento
  PAY_Recusado --> PAY_Pendente : tentar novamente

  PAY_Confirmado --> [*]
  PAY_Estornado --> [*]
}

ORC_Aprovado --> PAY_Pendente : ordem finalizada\nlibera cobrança

@enduml

```


## 17_modelo_dados_der.puml

```plantuml
@startuml
title OficinaPro — Modelo de Dados / DER

skinparam shadowing false
skinparam linetype ortho
hide circle

entity "clientes" as clientes {
  * id_cliente : BIGINT <<PK>>
  --
  * nome : VARCHAR
  * documento : VARCHAR
  telefone : VARCHAR
  email : VARCHAR
  endereco : VARCHAR
  data_cadastro : DATETIME
}

entity "veiculos" as veiculos {
  * id_veiculo : BIGINT <<PK>>
  --
  * id_cliente : BIGINT <<FK>>
  * placa : VARCHAR
  marca : VARCHAR
  modelo : VARCHAR
  ano : INTEGER
  cor : VARCHAR
  observacoes : TEXT
}

entity "usuarios" as usuarios {
  * id_usuario : BIGINT <<PK>>
  --
  * nome : VARCHAR
  * email : VARCHAR
  * senha_hash : VARCHAR
  * perfil : VARCHAR
  * ativo : BOOLEAN
}

entity "ordens_servico" as ordens {
  * id_ordem : BIGINT <<PK>>
  --
  * id_veiculo : BIGINT <<FK>>
  * numero : VARCHAR
  * descricao_problema : TEXT
  prioridade : VARCHAR
  * status : VARCHAR
  * data_abertura : DATETIME
  data_fechamento : DATETIME
}

entity "diagnosticos" as diagnosticos {
  * id_diagnostico : BIGINT <<PK>>
  --
  * id_ordem : BIGINT <<FK>>
  * id_mecanico : BIGINT <<FK>>
  * descricao : TEXT
  * data_registro : DATETIME
}

entity "servicos" as servicos {
  * id_servico : BIGINT <<PK>>
  --
  * nome : VARCHAR
  descricao : TEXT
  * valor_base : DECIMAL
  * ativo : BOOLEAN
}

entity "pecas" as pecas {
  * id_peca : BIGINT <<PK>>
  --
  * codigo : VARCHAR
  * nome : VARCHAR
  * quantidade_estoque : INTEGER
  * valor_unitario : DECIMAL
  estoque_minimo : INTEGER
  * ativo : BOOLEAN
}

entity "orcamentos" as orcamentos {
  * id_orcamento : BIGINT <<PK>>
  --
  * id_ordem : BIGINT <<FK>>
  * valor_total : DECIMAL
  * status : VARCHAR
  * validade : DATE
  * data_criacao : DATETIME
}

entity "orcamento_servicos" as orcamento_servicos {
  * id_orcamento_servico : BIGINT <<PK>>
  --
  * id_orcamento : BIGINT <<FK>>
  * id_servico : BIGINT <<FK>>
  * quantidade : INTEGER
  * valor_unitario : DECIMAL
  * subtotal : DECIMAL
}

entity "orcamento_pecas" as orcamento_pecas {
  * id_orcamento_peca : BIGINT <<PK>>
  --
  * id_orcamento : BIGINT <<FK>>
  * id_peca : BIGINT <<FK>>
  * quantidade : INTEGER
  * valor_unitario : DECIMAL
  * subtotal : DECIMAL
}

entity "ordem_servicos_executados" as ordem_servicos {
  * id_item_servico : BIGINT <<PK>>
  --
  * id_ordem : BIGINT <<FK>>
  * id_servico : BIGINT <<FK>>
  * quantidade : INTEGER
  * valor_unitario : DECIMAL
  * subtotal : DECIMAL
}

entity "ordem_pecas_utilizadas" as ordem_pecas {
  * id_item_peca : BIGINT <<PK>>
  --
  * id_ordem : BIGINT <<FK>>
  * id_peca : BIGINT <<FK>>
  * quantidade : INTEGER
  * valor_unitario : DECIMAL
  * subtotal : DECIMAL
}

entity "pagamentos" as pagamentos {
  * id_pagamento : BIGINT <<PK>>
  --
  * id_ordem : BIGINT <<FK>>
  * forma_pagamento : VARCHAR
  * valor_pago : DECIMAL
  * status : VARCHAR
  data_pagamento : DATETIME
  identificador_transacao : VARCHAR
}

entity "historico_status_ordem" as historico {
  * id_historico : BIGINT <<PK>>
  --
  * id_ordem : BIGINT <<FK>>
  status_anterior : VARCHAR
  * status_novo : VARCHAR
  * data_alteracao : DATETIME
  * id_usuario : BIGINT <<FK>>
  observacao : TEXT
}

clientes ||--o{ veiculos : possui
veiculos ||--o{ ordens : gera

ordens ||--o| diagnosticos : possui
usuarios ||--o{ diagnosticos : registra

ordens ||--o| orcamentos : possui
orcamentos ||--o{ orcamento_servicos : contém
servicos ||--o{ orcamento_servicos : compõe
orcamentos ||--o{ orcamento_pecas : contém
pecas ||--o{ orcamento_pecas : compõe

ordens ||--o{ ordem_servicos : executa
servicos ||--o{ ordem_servicos : referencia
ordens ||--o{ ordem_pecas : utiliza
pecas ||--o{ ordem_pecas : referencia

ordens ||--o{ pagamentos : recebe
ordens ||--o{ historico : registra
usuarios ||--o{ historico : altera

@enduml

```
