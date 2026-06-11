<!-- README adaptado a partir do template acadêmico fornecido para o projeto OficinaPro -->

<a href="https://classroom.github.com/online_ide?assignment_repo_id=99999999&assignment_repo_type=AssignmentRepo"><img src="https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg" width="200"/></a> <a href="https://classroom.github.com/open-in-codespaces?assignment_repo_id=99999999"><img src="https://classroom.github.com/assets/launch-codespace-2972f46106e565e64193e422d61a12cf1da4916b45550586e14ef0a7c637dd04.svg" width="250"/></a>

---

# 🏷️ OficinaPro — Sistema de Gestão de Oficina Mecânica 👨‍💻

> [!NOTE]
> Projeto acadêmico de documentação, modelagem, arquitetura e diagramação de um sistema para gestão operacional de oficina mecânica.  
> O foco do projeto é representar corretamente requisitos, regras de negócio, casos de uso, arquitetura, modelos UML e modelo de dados, sem necessidade de implementação de código.

<table>
  <tr>
    <td width="800px">
      <div align="justify">
        O <b>OficinaPro</b> é um projeto acadêmico desenvolvido para a disciplina de <b>Projeto de Software</b>, com o objetivo de documentar e modelar um sistema de gestão para oficinas mecânicas de pequeno e médio porte. A proposta central é organizar os principais processos de atendimento de uma oficina, incluindo cadastro de clientes e veículos, abertura de ordens de serviço, registro de diagnóstico, elaboração e aprovação de orçamentos, execução de serviços, controle de peças, pagamento e consulta ao histórico de manutenção. 
        <br><br>
        Por se tratar de uma atividade voltada à <b>documentação de projeto</b>, o sistema não precisa estar implementado. Ainda assim, a documentação foi construída como se pudesse orientar uma futura implementação real, mantendo coerência entre <i>regras de negócio</i>, <i>casos de uso</i>, <i>contratos de operação</i>, <i>arquitetura</i>, <i>diagramas UML</i> e <i>modelo de dados relacional</i>. Os diagramas do projeto foram especificados em <b>PlantUML</b>, garantindo padronização, rastreabilidade e facilidade de manutenção.
      </div>
    </td>
    <td>
      <div>
        <img src="https://cdn-icons-png.flaticon.com/512/1995/1995470.png" alt="Logo OficinaPro" width="120px"/>
      </div>
    </td>
  </tr> 
</table>

---

## 🚧 Status do Projeto

![Status](https://img.shields.io/badge/Status-Documentação%20Acadêmica-007ec6?style=for-the-badge)
![Versão](https://img.shields.io/badge/Versão-1.0.0-blue?style=for-the-badge)
![Projeto](https://img.shields.io/badge/Tipo-Projeto%20de%20Software-007ec6?style=for-the-badge)
![PlantUML](https://img.shields.io/badge/Diagramas-PlantUML-007ec6?style=for-the-badge)
![UML](https://img.shields.io/badge/Modelagem-UML-007ec6?style=for-the-badge)
![Banco](https://img.shields.io/badge/Modelo%20de%20Dados-Relacional-007ec6?style=for-the-badge)

> [!IMPORTANT]
> Este repositório representa um projeto acadêmico de documentação e arquitetura. Não há implementação funcional obrigatória, execução de código, deploy em produção ou testes automatizados reais.

---

## 📚 Índice
- [Links Úteis](#-links-úteis)
- [Sobre o Projeto](#-sobre-o-projeto)
- [Funcionalidades Principais](#-funcionalidades-principais)
- [Tecnologias Utilizadas](#-tecnologias-utilizadas)
- [Arquitetura](#-arquitetura)
  - [Diagramas do Projeto](#diagramas-do-projeto)
- [Instalação e Execução](#-instalação-e-execução)
  - [Pré-requisitos](#pré-requisitos)
  - [Variáveis de Ambiente](#-variáveis-de-ambiente)
  - [Instalação de Dependências](#-instalação-de-dependências)
  - [Inicialização do Banco de Dados](#-inicialização-do-banco-de-dados)
  - [Como Executar a Aplicação](#-como-executar-a-aplicação)
- [Deploy](#-deploy)
- [Estrutura de Pastas](#-estrutura-de-pastas)
- [Demonstração](#-demonstração)
  - [Documentação do Projeto](#-documentação-do-projeto)
  - [Diagramas PlantUML](#-diagramas-plantuml)
- [Testes](#-testes)
- [Documentações utilizadas](#-documentações-utilizadas)
- [Autores](#-autores)
- [Contribuição](#-contribuição)
- [Agradecimentos](#-agradecimentos)
- [Licença](#-licença)

---

## 🔗 Links Úteis

* 📄 **Documentação do Projeto:** [Documento OficinaPro](./docs/Trabalho_2_Projeto_OficinaPro.docx)  
  > 📚 **Descrição:** Documento principal contendo introdução, atores, regras de negócio, casos de uso, contratos de operação, arquitetura, modelos de projeto e modelo de dados.

* 🧩 **Diagramas PlantUML:** [Pasta de Diagramas](./docs/diagramas/)  
  > 🧠 **Descrição:** Arquivos `.puml` utilizados para gerar os diagramas UML do projeto.

* 🌐 **PlantUML Online Server:** [Acessar PlantUML](https://www.plantuml.com/plantuml/)  
  > 💻 **Descrição:** Ferramenta online para renderizar os códigos PlantUML dos diagramas.

* 📖 **Documentação Oficial PlantUML:** [Leia a documentação](https://plantuml.com/)  
  > 📚 **Descrição:** Referência oficial da sintaxe utilizada nos diagramas de casos de uso, sequência, classes, componentes, implantação, estados e modelo de dados.

---

## 📝 Sobre o Projeto

O **OficinaPro — Sistema de Gestão de Oficina Mecânica** é um sistema hipotético criado para fins acadêmicos, cujo objetivo é representar a solução de software para uma oficina mecânica que deseja organizar seus atendimentos de forma mais clara, rastreável e controlada.

O problema central tratado pelo projeto é a dificuldade de controlar manualmente informações como clientes, veículos, ordens de serviço, diagnósticos, orçamentos, peças utilizadas, pagamentos e histórico de manutenção. Em uma oficina real, a ausência de um sistema centralizado pode gerar perda de informações, retrabalho, dificuldade para consultar atendimentos anteriores e baixa previsibilidade sobre peças, serviços e faturamento.

A proposta do OficinaPro é modelar um sistema capaz de apoiar o fluxo operacional básico da oficina, desde a chegada do cliente até a entrega do veículo. O projeto considera diferentes perfis de usuários, como cliente, atendente, mecânico, gerente e administrador, além de integrações simuladas com sistema de pagamento e sistema de notificação.

O contexto do projeto é **acadêmico**, com foco em **engenharia de software**, **projeto de sistemas**, **modelagem UML**, **arquitetura em camadas** e **modelo de dados relacional**. Portanto, não há exigência de desenvolvimento do código-fonte da aplicação. A entrega principal é a documentação do sistema e seus modelos.

---

## ✨ Funcionalidades Principais

- 👤 **Cadastro de Clientes:** Registro de dados do cliente, documento, telefone, e-mail e endereço.
- 🚗 **Cadastro de Veículos:** Associação de um ou mais veículos a cada cliente.
- 🧾 **Abertura de Ordem de Serviço:** Criação de atendimento vinculado a um veículo, com descrição do problema informado.
- 🔎 **Registro de Diagnóstico:** Lançamento da análise técnica feita pelo mecânico.
- 💰 **Elaboração de Orçamento:** Definição de serviços, peças, quantidades, valores unitários e valor total estimado.
- ✅ **Aprovação ou Rejeição de Orçamento:** Controle da decisão do cliente antes da execução dos serviços.
- 🛠️ **Execução de Serviços:** Registro dos serviços executados após aprovação do orçamento.
- 📦 **Controle de Peças:** Associação de peças à ordem de serviço e baixa de estoque.
- 🔄 **Atualização de Status:** Controle do ciclo de vida da ordem de serviço com histórico de alterações.
- 💳 **Registro de Pagamento:** Registro operacional do pagamento e atualização da ordem para paga.
- 📜 **Histórico do Veículo:** Consulta de ordens anteriores, serviços realizados e peças utilizadas.
- 📊 **Relatórios Gerenciais:** Consulta de indicadores de faturamento, ordens concluídas, peças utilizadas e produtividade.

---

## 🛠 Tecnologias Utilizadas

Como o projeto é voltado para documentação e arquitetura, as tecnologias abaixo representam as ferramentas e padrões utilizados ou previstos para uma possível implementação futura.

### 💻 Front-end

* **Interface prevista:** Aplicação Web.
* **Framework sugerido:** React.
* **Linguagem sugerida:** TypeScript.
* **Build Tool sugerida:** Vite.
* **Estilização sugerida:** CSS Modules, Tailwind CSS ou Material UI.
* **Responsabilidade:** Disponibilizar telas para clientes e usuários internos interagirem com cadastros, ordens de serviço, orçamento, pagamento, estoque e relatórios.

### 🖥️ Back-end

* **Arquitetura sugerida:** API REST em camadas.
* **Linguagem sugerida:** Java 17 ou superior.
* **Framework sugerido:** Spring Boot.
* **Persistência sugerida:** Spring Data JPA / Hibernate.
* **Banco de Dados sugerido:** PostgreSQL.
* **Autenticação sugerida:** Spring Security com controle por perfis.
* **Responsabilidade:** Controlar os casos de uso, aplicar regras de negócio, persistir dados e integrar serviços externos simulados.

### 🧩 Modelagem e Documentação

* **Modelagem UML:** PlantUML.
* **Diagramas previstos:** Casos de uso, sequência, componentes, implantação, classes, comunicação, estados e DER.
* **Documento principal:** DOCX com a documentação de projeto.
* **Linguagem de documentação:** Markdown e documentação acadêmica estruturada.

### ⚙️ Infraestrutura & DevOps

* **Ambiente previsto:** Navegador, servidor de aplicação, banco de dados relacional e serviços externos simulados.
* **Containerização sugerida:** Docker e Docker Compose.
* **CI/CD:** Não obrigatório para a entrega acadêmica.
* **Deploy:** Não aplicável nesta etapa, pois o projeto não exige implementação funcional.

---

## 🏗 Arquitetura

A arquitetura proposta para o OficinaPro segue uma organização em **camadas**, separando responsabilidades de apresentação, aplicação, domínio, persistência e integrações externas. Essa escolha foi adotada por ser adequada para projetos acadêmicos de engenharia de software, além de facilitar a compreensão dos papéis de cada parte do sistema.

A separação em camadas ajuda a reduzir o acoplamento, melhora a rastreabilidade entre requisitos e projeto, facilita a manutenção conceitual do sistema e permite que as regras de negócio sejam representadas de forma clara.

### Camadas principais

| Camada | Responsabilidade | Exemplos no OficinaPro |
| :--- | :--- | :--- |
| **Apresentação** | Disponibilizar telas para interação dos usuários. | Interface Web, telas de ordem de serviço, orçamento, estoque e relatórios. |
| **Aplicação** | Coordenar os casos de uso e controlar o fluxo das operações. | OrdemServicoService, OrcamentoService, PagamentoService. |
| **Domínio** | Representar entidades e regras de negócio principais. | Cliente, Veiculo, OrdemServico, Orcamento, Peca, Pagamento. |
| **Persistência** | Armazenar e recuperar dados estruturados. | Repositórios e banco relacional OficinaProDB. |
| **Integrações** | Simular comunicação com serviços externos. | Sistema de Pagamento e Sistema de Notificação. |

### Padrões de projeto e organização previstos

- **Service Layer:** concentração dos fluxos de aplicação em serviços.
- **Repository Pattern:** abstração do acesso a dados.
- **DTOs:** transferência de dados entre interface e API.
- **Controle por perfis:** separação de permissões entre atendente, mecânico, gerente e administrador.
- **Histórico de status:** rastreabilidade das mudanças no ciclo de vida da ordem de serviço.

### Diagramas do Projeto

Para melhor visualização e entendimento da estrutura do sistema, os diagramas principais estão organizados abaixo. As imagens podem ser geradas a partir dos arquivos `.puml` localizados em `./docs/diagramas/`.

| Diagrama de Arquitetura | Diagrama de Componentes |
| :---: | :---: |
| **Visão Geral em Camadas** | **Componentes Internos** |
| <img src="./docs/diagramas/05_arquitetura.png" alt="Diagrama de Arquitetura" width="220px"> | <img src="./docs/diagramas/06_componentes.png" alt="Diagrama de Componentes" width="220px"> |
| **Diagrama de Implantação** | **Diagrama de Classes** |
| <img src="./docs/diagramas/07_implantacao.png" alt="Diagrama de Implantação" width="220px"> | <img src="./docs/diagramas/08_classes.png" alt="Diagrama de Classes" width="220px"> |
| **Modelo de Dados / DER** | **Diagrama de Estados** |
| <img src="./docs/diagramas/17_modelo_dados_der.png" alt="Modelo de Dados DER" width="220px"> | <img src="./docs/diagramas/15_estados_ordem_servico.png" alt="Diagrama de Estados da Ordem de Serviço" width="220px"> |

---

## 🔧 Instalação e Execução

> [!WARNING]
> Este projeto não possui implementação de código-fonte obrigatória. As instruções abaixo existem apenas para manter o padrão do template e indicar como o projeto poderia ser organizado caso fosse implementado futuramente.

### Pré-requisitos

Para visualizar, editar ou gerar os artefatos do projeto, recomenda-se:

* **Editor de texto ou IDE:** Visual Studio Code, IntelliJ IDEA ou similar.
* **PlantUML:** Para renderização dos diagramas.
* **Java:** Necessário caso o PlantUML seja executado localmente.
* **Graphviz:** Recomendado para alguns tipos de diagrama PlantUML.
* **Git:** Para versionamento do projeto.
* **Microsoft Word ou LibreOffice:** Para abrir a documentação `.docx`.

---

### 🔑 Variáveis de Ambiente

Não há variáveis de ambiente obrigatórias, pois o projeto não possui aplicação executável.

Caso a solução fosse implementada futuramente, poderiam ser utilizadas variáveis como:

| Variável | Descrição | Exemplo |
| :--- | :--- | :--- |
| `SERVER_PORT` | Porta da API back-end. | `8080` |
| `SPRING_DATASOURCE_URL` | URL de conexão com o banco PostgreSQL. | `jdbc:postgresql://localhost:5432/oficinapro` |
| `SPRING_DATASOURCE_USERNAME` | Usuário do banco de dados. | `postgres` |
| `SPRING_DATASOURCE_PASSWORD` | Senha do banco de dados. | `senha-segura` |
| `JWT_SECRET` | Chave para assinatura de tokens. | `chave_super_segura` |
| `VITE_API_URL` | URL base da API consumida pelo front-end. | `http://localhost:8080/api` |

---

### 📦 Instalação de Dependências

Como não há aplicação implementada, não existem dependências obrigatórias de front-end ou back-end.

Para trabalhar apenas com os diagramas, uma alternativa é instalar PlantUML localmente ou utilizar o servidor online.

#### Opção 1 — Usar PlantUML Online

Acesse:

```text
https://www.plantuml.com/plantuml/
```

Cole o conteúdo de qualquer arquivo `.puml` e gere o diagrama.

#### Opção 2 — Usar PlantUML localmente

```bash
# Exemplo genérico de renderização local
java -jar plantuml.jar docs/diagramas/05_arquitetura.puml
```

---

### 💾 Inicialização do Banco de Dados

Não há banco de dados real para inicializar.

O projeto possui apenas um **modelo relacional proposto**, documentado no arquivo principal e no diagrama DER em PlantUML. Caso o sistema fosse implementado, o banco sugerido seria PostgreSQL.

---

### ⚡ Como Executar a Aplicação

Não há aplicação executável nesta entrega.

Para validar os artefatos do projeto, recomenda-se:

```bash
# 1. Abrir o documento principal
docs/Trabalho_2_Projeto_OficinaPro.docx

# 2. Abrir os diagramas PlantUML
docs/diagramas/*.puml

# 3. Renderizar os diagramas no PlantUML
java -jar plantuml.jar docs/diagramas/*.puml
```

---

## 🚀 Deploy

Não há deploy previsto, pois o projeto não possui implementação funcional.

Em uma implementação futura, a estratégia de deploy poderia seguir o seguinte modelo:

1. **Front-end:** hospedagem em Vercel, Netlify ou servidor estático.
2. **Back-end:** execução de API Spring Boot em Render, Railway, Heroku, VPS ou ambiente acadêmico.
3. **Banco de Dados:** PostgreSQL em serviço gerenciado ou container Docker.
4. **Diagramas e documentação:** versionados em `/docs`.

> [!NOTE]
> Para a entrega atual, o objetivo é a documentação de projeto e a correta modelagem dos artefatos, não a publicação de uma aplicação em produção.

---

## 📂 Estrutura de Pastas

Estrutura sugerida para organizar o projeto no repositório:

```text
.
├── README.md                              # 📘 Documentação principal do repositório.
├── LICENSE                                # ⚖️ Licença do projeto, se aplicável.
├── .gitignore                             # 🧹 Arquivos ignorados pelo Git.
│
├── /docs                                  # 📚 Documentação acadêmica do projeto.
│   ├── Trabalho_2_Projeto_OficinaPro.docx # 📄 Documento principal da atividade.
│   ├── /diagramas                         # 🧩 Diagramas PlantUML.
│   │   ├── 01_casos_de_uso.puml
│   │   ├── 02_sistema_sequence_uc02_abrir_ordem.puml
│   │   ├── 03_sistema_sequence_uc05_aprovar_orcamento.puml
│   │   ├── 04_sistema_sequence_uc10_registrar_pagamento.puml
│   │   ├── 05_arquitetura.puml
│   │   ├── 06_componentes.puml
│   │   ├── 07_implantacao.puml
│   │   ├── 08_classes.puml
│   │   ├── 09_projeto_sequence_uc02_abrir_ordem.puml
│   │   ├── 10_projeto_sequence_uc04_uc05_orcamento_aprovacao.puml
│   │   ├── 11_projeto_sequence_uc10_registrar_pagamento.puml
│   │   ├── 12_comunicacao_uc02_abrir_ordem.puml
│   │   ├── 13_comunicacao_uc05_aprovar_orcamento.puml
│   │   ├── 14_comunicacao_uc10_registrar_pagamento.puml
│   │   ├── 15_estados_ordem_servico.puml
│   │   ├── 16_estados_orcamento_pagamento.puml
│   │   └── 17_modelo_dados_der.puml
│   │
│   └── /imagens                           # 🖼️ Imagens exportadas dos diagramas, se geradas.
│       ├── 05_arquitetura.png
│       ├── 06_componentes.png
│       ├── 07_implantacao.png
│       ├── 08_classes.png
│       └── 17_modelo_dados_der.png
│
└── /referencias                           # 🔗 Materiais de apoio e referências, se necessário.
```

---

## 🎥 Demonstração

> [!WARNING]
> Como o projeto não possui implementação funcional, esta seção demonstra os artefatos acadêmicos produzidos: documentação, diagramas e modelos.

### 📄 Documentação do Projeto

| Artefato | Descrição |
| :---: | :--- |
| **Documento Principal** | Contém a documentação de projeto do OficinaPro, incluindo introdução, atores, regras de negócio, casos de uso, contratos, modelos de projeto e modelo de dados. |
| **Casos de Uso** | Representam as interações entre Cliente, Atendente, Mecânico, Gerente, Administrador e sistemas externos simulados. |
| **Contratos de Operação** | Especificam operações como abrir ordem de serviço, registrar diagnóstico, gerar orçamento, registrar decisão de orçamento, finalizar ordem e registrar pagamento. |

### 🧩 Diagramas PlantUML

| Diagrama | Arquivo |
| :--- | :--- |
| Casos de Uso | `01_casos_de_uso.puml` |
| Sequência do Sistema — UC-02 | `02_sistema_sequence_uc02_abrir_ordem.puml` |
| Sequência do Sistema — UC-05 | `03_sistema_sequence_uc05_aprovar_orcamento.puml` |
| Sequência do Sistema — UC-10 | `04_sistema_sequence_uc10_registrar_pagamento.puml` |
| Arquitetura | `05_arquitetura.puml` |
| Componentes | `06_componentes.puml` |
| Implantação | `07_implantacao.puml` |
| Classes | `08_classes.puml` |
| Sequências de Projeto | `09`, `10` e `11` |
| Comunicação | `12`, `13` e `14` |
| Estados | `15` e `16` |
| Modelo de Dados / DER | `17_modelo_dados_der.puml` |

### 💻 Exemplo de Renderização dos Diagramas

```bash
# Renderiza todos os diagramas PlantUML da pasta docs/diagramas
java -jar plantuml.jar docs/diagramas/*.puml
```

**Saída Esperada:**
```text
[INFO] Renderizando diagramas PlantUML...
[SUCCESS] 17 diagramas processados.
[SUCCESS] Arquivos PNG gerados na pasta docs/diagramas.
```

---

## 🧪 Testes

Não existem testes automatizados, pois não há código implementado.

Ainda assim, os artefatos podem ser avaliados por meio de revisão acadêmica:

- Verificar se os casos de uso estão coerentes com os atores.
- Verificar se os contratos de operação citam regras de negócio compatíveis.
- Verificar se os diagramas de sequência representam os casos de uso selecionados.
- Verificar se o diagrama de classes é consistente com o modelo de dados.
- Verificar se os estados da ordem de serviço respeitam as regras de negócio.
- Verificar se os nomes usados nos diagramas batem com os nomes usados no documento.

### Checklist de validação

```text
[OK] Documento possui introdução e escopo.
[OK] Atores foram descritos.
[OK] Casos de uso possuem identificadores.
[OK] Existem diagramas PlantUML previstos para os modelos solicitados.
[OK] Contratos de operação possuem pré-condições e pós-condições.
[OK] Modelo de dados possui tabelas, chaves primárias e chaves estrangeiras.
[OK] Projeto não depende de código executável.
```

---

## 🔗 Documentações utilizadas

* 📖 **PlantUML — Documentação Oficial:** [https://plantuml.com/](https://plantuml.com/)
* 📖 **PlantUML — Diagrama de Casos de Uso:** [https://plantuml.com/use-case-diagram](https://plantuml.com/use-case-diagram)
* 📖 **PlantUML — Diagrama de Sequência:** [https://plantuml.com/sequence-diagram](https://plantuml.com/sequence-diagram)
* 📖 **PlantUML — Diagrama de Classes:** [https://plantuml.com/class-diagram](https://plantuml.com/class-diagram)
* 📖 **PlantUML — Diagrama de Componentes:** [https://plantuml.com/component-diagram](https://plantuml.com/component-diagram)
* 📖 **PlantUML — Diagrama de Estados:** [https://plantuml.com/state-diagram](https://plantuml.com/state-diagram)
* 📖 **PlantUML — Information Engineering / DER:** [https://plantuml.com/ie-diagram](https://plantuml.com/ie-diagram)
* 📖 **UML — Linguagem de Modelagem Unificada:** Referência conceitual utilizada para organização dos modelos.
* 📄 **Documento de Projeto OficinaPro:** Documentação acadêmica produzida para a atividade.

---

## 👥 Autores

| 👤 Nome | 🖼️ Foto | :octocat: GitHub | 💼 LinkedIn | 📤 Gmail |
|---------|----------|-----------------|-------------|-----------|
| Lorran Xavier | <div align="center"><img src="https://joaopauloaramuni.github.io/image/aramunilogo.png" width="70px" height="70px"></div> | <div align="center"><a href="https://github.com/lorranpedro"><img src="https://joaopauloaramuni.github.io/image/github6.png" width="50px" height="50px"></a></div> | <div align="center"><a href="#"><img src="https://joaopauloaramuni.github.io/image/linkedin2.png" width="50px" height="50px"></a></div> | <div align="center"><a href="mailto:lorranpedro.lp6@gmail.com"><img src="https://joaopauloaramuni.github.io/image/gmail3.png" width="50px" height="50px"></a></div> |

> [!TIP]
> Caso o professor exija identificação institucional, substituir ou complementar os dados acima com matrícula, turma e unidade.

---

## 🤝 Contribuição

Este é um projeto acadêmico individual. Portanto, contribuições externas não são o foco principal.

Caso o projeto fosse evoluído em equipe, o fluxo sugerido seria:

1. Faça um `fork` do projeto.
2. Crie uma branch para sua alteração (`git checkout -b feature/nova-modelagem`).
3. Faça commits seguindo o padrão Conventional Commits (`docs: atualiza diagrama de classes`).
4. Envie a branch para o repositório remoto.
5. Abra um Pull Request explicando a alteração proposta.

> [!IMPORTANT]
> Alterações nos diagramas devem manter consistência com o documento principal, especialmente nomes de atores, casos de uso, classes, estados e tabelas.

---

## 🙏 Agradecimentos

Agradecimentos à **PUC Minas** e à disciplina de **Projeto de Software** pela proposta da atividade e pelo incentivo ao uso de boas práticas de documentação, modelagem e arquitetura de sistemas.

Também são reconhecidas como referências importantes:

* **Engenharia de Software PUC Minas** — Pela formação acadêmica em análise, projeto e documentação de sistemas.
* **PlantUML** — Pela ferramenta utilizada para especificação textual dos diagramas.
* **Comunidade de Engenharia de Software** — Pelas práticas consolidadas de modelagem, documentação e organização de projetos.

---

## 📄 Licença

Este projeto possui finalidade exclusivamente acadêmica.

Caso seja necessário definir uma licença formal para publicação em repositório, recomenda-se utilizar a **Licença MIT** ou manter o repositório como material privado da disciplina.

---
