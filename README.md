<div align="center">

<img width="1742" height="493" alt="Banner Herik Kato" src="https://github.com/user-attachments/assets/0f18624a-4e40-40db-8d2f-7252d1704139" />

<br/><br/>

<a href="https://www.linkedin.com/in/herik-kato-dev/">
  <img src="https://img.shields.io/badge/LinkedIn-Herik_Kato-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white"/>
</a>
&nbsp;
<a href="mailto:herikkou@gmail.com">
  <img src="https://img.shields.io/badge/Email-herikkou%40gmail.com-EA4335?style=for-the-badge&logo=gmail&logoColor=white"/>
</a>
&nbsp;
<a href="https://github.com/Herikkou">
  <img src="https://img.shields.io/badge/GitHub-Herikkou-181717?style=for-the-badge&logo=github&logoColor=white"/>
</a>
&nbsp;
<img src="https://komarev.com/ghpvc/?username=Herikkou&style=for-the-badge&color=238636&label=VISITAS"/>

</div>

---

## Sobre mim

Desenvolvedor backend especializado em **Java + Spring Boot** com foco em **sistemas financeiros distribuídos**. Construo microsserviços orientados a eventos aplicados a problemas reais do domínio bancário — análise de crédito, disputas de pagamento PIX, gestão de boletos, antifraude e reconciliação financeira.

Formado em **Análise e Desenvolvimento de Sistemas** pela Universidade de Mogi das Cruzes (UMC). Cada projeto nasce de uma dor real: problemas que pessoas enfrentam todos os dias com bancos e fintechs.

<br/>

| | |
|---|---|
| 🎯 **Especialidade** | Sistemas financeiros · Microsserviços · Event-Driven Architecture |
| 🛠️ **Stack principal** | Java 17/21 · Spring Boot · Kafka · PostgreSQL · Docker · Datadog |
| 🤖 **Diferencial** | Integração de LLMs (Claude API) em fluxos financeiros reais |
| 📍 **Localização** | São Paulo, SP — presencial, remoto ou híbrido |
| ✅ **Status** | **Open to Work** |

---

## Projetos em Destaque

### 💳 Sistema de Análise de Crédito com IA
> 🔗 [github.com/HerikKou/Limites-de-credito-com-IA](https://github.com/HerikKou/Limites-de-credito-com-IA)

**Problema:** Pessoas têm limite de crédito disponível, mas não conseguem avaliar com clareza se uma nova compra é compatível com sua situação financeira real. O banco libera o crédito — mas não explica se aquela compra é uma boa decisão.

**Solução:** Sistema distribuído em 5 microsserviços que analisa o cenário financeiro do usuário em tempo real. Avalia histórico de gastos, atrasos em parcelas, consultas no CPF e tempo de histórico. Calcula um score de risco e usa LLM para transformar a análise em linguagem natural. Quando o risco é alto, o usuário recebe um e-mail explicando exatamente o porquê.

| Serviço | Responsabilidade |
|---|---|
| `CreditoService` | Registra dados financeiros e publica eventos no Kafka |
| `HistoricoService` | Consolida o total gasto e calcula exposição financeira |
| `ScoreService` | Calcula score de risco com base em 4 critérios comportamentais |
| `LLMService` | Consome o score e gera explicação humanizada via Claude API |
| `NotificacaoService` | Envia e-mail quando o score indica risco elevado |

`Java 17` `Spring Boot` `Apache Kafka` `PostgreSQL` `Claude API` `Docker` `Datadog` `JUnit` `Mockito`

---

### 💸 Sistema de Devolução de Pagamento PIX com IA
> 🔗 [github.com/HerikKou/Devolucao-de-Pagamento](https://github.com/HerikKou/Devolucao-de-Pagamento)

**Problema:** Pessoas fazem transferências PIX por engano e, ao abrir uma disputa, a instituição financeira aprova ou reprova sem fornecer qualquer explicação. O cliente fica sem entender o que aconteceu com o dinheiro — e sem saber o que fazer.

**Solução:** Sistema de disputas PIX em 7 microsserviços que calcula um score de credibilidade baseado em frequência e histórico de disputas, usando esse score como base para a decisão financeira. **A IA não decide** — a decisão é tomada pelas regras de negócio. O LLM é utilizado exclusivamente para transformar os fatores da análise em uma explicação clara e humanizada para o cliente.

| Serviço | Responsabilidade |
|---|---|
| `TransacaoService` | Registra o PIX e publica o evento inicial |
| `DevolucaoService` | Abre a disputa e calcula métricas comportamentais |
| `ScoreService` | Calcula o score de credibilidade por frequência e volume de disputas |
| `DecisaoService` | Toma a decisão financeira com base no score |
| `PagamentoService` | Registra o pagamento aprovado |
| `ExplicacaoService` | Gera explicação humanizada via Claude API |
| `HistoricoService` | Registra o fluxo completo para auditoria |

**Padrões:** Retry com backoff exponencial · Dead Letter Topic (DLT) · Idempotência por campo de controle

`Java 17` `Spring Boot` `Apache Kafka` `PostgreSQL` `Claude API` `Docker` `Datadog`

---

### 🧾 Plataforma de Gestão de Boletos
> 🔗 [github.com/HerikKou/Plataforma_de_Gestao_de_Boletos](https://github.com/HerikKou/Plataforma_de_Gestao_de_Boletos)

**Problema:** Boletos vencem sem que o usuário perceba — e o custo de um boleto vencido não é só o juro de 5%: é a multa, a negativação e a burocracia para regularizar. O processo inteiro costuma ser manual e sujeito a esquecimento.

**Solução:** Plataforma distribuída em 4 microsserviços que automatiza o ciclo de vida completo do boleto — desde a emissão até o pagamento automático — com notificações proativas por e-mail antes do vencimento. O ciclo de estado de cada boleto é controlado por uma **State Machine**, garantindo transições válidas e auditáveis.

| Serviço | Responsabilidade |
|---|---|
| `BoletoService` | Recebe, valida e registra boletos via REST, publicando eventos no Kafka |
| `VencimentoService` | Monitora vencimentos, alerta 3 dias antes e aplica juros de 5% automaticamente |
| `NotificacaoService` | Envia e-mails com templates Thymeleaf — único serviço com acesso ao e-mail do usuário |
| `PagamentoService` | Processa débito automático com garantia de idempotência |

**State Machine:** `CRIADO → PRÓXIMO_VENCIMENTO → VENCIDO → PAGO`

**Padrões:** Idempotência · Retry · DLT · State Machine · Isolamento de dados sensíveis

`Java 21` `Spring Boot` `Apache Kafka` `PostgreSQL` `Spring Mail` `Thymeleaf` `Docker` `JUnit` `Mockito`

---

## Stack Tecnológica

<div align="center">

### Linguagens
![Java](https://img.shields.io/badge/Java_17/21-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)

### Frameworks & Backend
![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?style=for-the-badge&logo=spring-boot&logoColor=white)
![Spring Security](https://img.shields.io/badge/Spring_Security-6DB33F?style=for-the-badge&logo=springsecurity&logoColor=white)
![Spring MVC](https://img.shields.io/badge/Spring_MVC-6DB33F?style=for-the-badge&logo=spring&logoColor=white)
![Hibernate](https://img.shields.io/badge/Hibernate-59666C?style=for-the-badge&logo=hibernate&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)

### Mensageria & Eventos
![Apache Kafka](https://img.shields.io/badge/Apache_Kafka-231F20?style=for-the-badge&logo=apache-kafka&logoColor=white)

### IA & LLMs
![Claude API](https://img.shields.io/badge/Claude_API-D97757?style=for-the-badge&logo=anthropic&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)

### Bancos de Dados
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)

### Observabilidade & Testes
![Datadog](https://img.shields.io/badge/Datadog-632CA6?style=for-the-badge&logo=datadog&logoColor=white)
![JUnit5](https://img.shields.io/badge/JUnit_5-25A162?style=for-the-badge&logo=junit5&logoColor=white)
![Mockito](https://img.shields.io/badge/Mockito-FF6C37?style=for-the-badge&logo=java&logoColor=white)

### Infra, Cloud & DevOps
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Docker Compose](https://img.shields.io/badge/Docker_Compose-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)

</div>

---

## Formação & Experiência

### 🎓 Formação Acadêmica

| Curso | Instituição |
|---|---|
| Tecnólogo em Análise e Desenvolvimento de Sistemas | Universidade de Mogi das Cruzes (UMC) · 2024–2025 |

### 🚀 Bootcamp Intensivo — Desenvolvimento de Sistemas

Bootcamp em equipe com foco em desenvolvimento e estruturação de sistemas completos, simulando o ciclo real de projetos de software:

- Atuação como **Tech Lead** — definição de arquitetura, decisões técnicas e divisão de responsabilidades
- Modelagem de dados e implementação de regras de negócio complexas
- Desenvolvimento com **controle de acesso, perfis e permissões**
- Criação de **testes automatizados** e documentação técnica dos sistemas
- Colaboração em equipe com resolução de problemas em ambiente real de projeto

### 📚 Aprendizado Contínuo

Datadog · AWS · Arquitetura de Sistemas Distribuídos · LLMs aplicados a fluxos financeiros

---

## Vamos conversar?

<div align="center">

Se você tem um projeto desafiador, uma oportunidade ou quer trocar ideia sobre backend financeiro e arquitetura de sistemas — **me chama.**

<br/>

<a href="https://www.linkedin.com/in/herik-kato-dev/">
  <img src="https://img.shields.io/badge/Conectar_no_LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white"/>
</a>
&nbsp;
<a href="mailto:herikkou@gmail.com">
  <img src="https://img.shields.io/badge/Enviar_Email-EA4335?style=for-the-badge&logo=gmail&logoColor=white"/>
</a>

<br/><br/>

<sub><i>"Construo sistemas que resolvem dores reais — não apenas exercícios técnicos."</i></sub>

<br/><br/>

![Footer](https://capsule-render.vercel.app/api?type=waving&color=0:6DB33F,50:0D1E2E,100:2E75B6&height=80&section=footer)

</div>

