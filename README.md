**Typebot CEAF – Consulta de Status (Farmácia de Alto Custo)**

![Fluxo do Typebot](app.typebot.io_typebots_cmkr0fvxa000ml104iutxoof2_edit.png)


Este repositório contém o **fluxo completo em JSON de um Typebot** desenvolvido para automatizar o atendimento da **Farmácia de Alto Custo (CEAF)**, permitindo que pacientes consultem o **status do seu processo** utilizando o **CNS** ou **CPF**, com total conformidade com a **LGPD**.

O projeto foi criado com foco em:
- Automação de atendimento público
- Tratar e dar segurança aos dados públicos que serão dispostos aos solicitantes
- Permitir que atenda e se adapte para a realidade de outras unidades ou órgãos
- Atender uma demanda de informação crescente nas farmácias de alto custo do DF.
- Dar transparência ao trâmite executado dentro das farmácias de alto custo.
- Entregar ao paciente exatidão no andamento do seu processo.

---

## Objetivo do Projeto

Permitir que o cidadão ou representante:
- Localizar o status do seu processo no CEAF via **CNS**
- Tratar os dados e fornecer apenas o status administrativo
- Certificar que seus dados tratados estejam conforme a **Lei Geral de Proteção de Dados (LGPD)**

---


## 🧩 Visão Geral do Fluxo (Passo a Passo)

O fluxo do bot foi projetado de forma **linear, modular e segura**, garantindo clareza para o usuário final e facilidade de manutenção e adaptação por outros desenvolvedores.

---

### 1️⃣ Reset de Segurança (Inicialização da Sessão)

- Ao iniciar o bot, é executado automaticamente um **reset completo das variáveis de sessão**.
- Variáveis limpas neste passo:
  - `nome_paciente`
  - `cns_paciente`
  - `resposta_bot`
- Esse procedimento evita:
  - Reaproveitamento indevido de dados
  - Vazamento de informações entre sessões
  - Inconsistências em novas consultas
- Este bloco é sempre executado **antes de qualquer interação com o usuário**.

---

### 2️⃣ Saudação Inicial e Contextualização

- O bot apresenta uma mensagem de boas-vindas ao usuário.
- Informa que o atendimento é referente à **Farmácia de Alto Custo (CEAF)**.
- Este passo tem como objetivo:
  - Contextualizar o serviço oferecido
  - Gerar confiança no atendimento automatizado
  - Orientar o usuário sobre o tipo de informação disponível

---

### 3️⃣ Menu de Escolha (Decisão do Usuário)

- O usuário recebe um menu com duas opções principais:
  - **Verificar o status do processo**
  - **Solicitar outra informação**
- Cada opção direciona para um fluxo distinto:
  - Consulta automatizada via CNS
  - Fluxo alternativo para atendimento humano
- Este ponto funciona como um **divisor lógico do fluxo**, evitando consultas desnecessárias.

---

### 4️⃣ Coleta do CNS do Paciente

- Caso o usuário opte por consultar o status:
  - O bot solicita o número do **CNS do paciente**
  - É informado que o CNS deve ser digitado:
    - Apenas com números
    - Sem pontos ou espaços
- O valor informado é armazenado na variável:
  - `cns_paciente`

---

### 5️⃣ Validação Estrutural do CNS

- O CNS informado passa por uma validação automática:
  - Verificação via **expressão regular (regex)**
  - Exigência de **exatamente 15 dígitos numéricos**
- Se o CNS não atender ao padrão esperado:
  - O usuário é informado imediatamente do erro
  - O fluxo é redirecionado para correção
- Esta etapa evita:
  - Consultas inválidas
  - Erros de integração com a base de dados

---

### 6️⃣ Confirmação e Aviso de LGPD

- Após a validação do CNS:
  - O bot confirma o recebimento do dado
  - Exibe um aviso sobre o tratamento das informações
- O aviso informa que:
  - Os dados são usados exclusivamente para atendimento
  - Não há compartilhamento com terceiros
  - As informações são tratadas como **não públicas**
- Este passo reforça a conformidade com a **LGPD**.

---

### 7️⃣ Consulta Automatizada no Google Sheets

- O bot realiza uma integração direta com o **Google Sheets**, utilizado como base de dados.
- A planilha usada funciona como a base de dados, contendo diversos dados sensíveis relativos ao paciente.
- A lógica do bot irá cruzar os dados do paciente que entra com um dado sensível pessoal, esses dados são tratados e por fim, o retorno é fornecido do status administrativo processo do paciente, sem qualquer exposição dos dados sensíveis.

---

### 8️⃣ Tratamento do Resultado da Consulta

Após a consulta, o fluxo segue conforme o resultado obtido:

#### ✅ Registro Encontrado
- O bot informa que o cadastro foi localizado com sucesso.
- Exibe o **status do processo**, conforme retornado da planilha.
- O usuário pode:
  - Realizar uma nova consulta
  - Encerrar o atendimento

#### ❌ Registro Não Localizado
- O usuário é informado que o processo não foi encontrado na base de dados.
- É orientado a:
  - Conferir o número informado
  - Aguardar atendimento humano, se necessário

---

### 9️⃣ Fluxos Alternativos e Tratamento de Exceções

O bot possui fluxos específicos para situações fora do caminho principal:

- **CNS inválido**
  - Erro de formatação ou quantidade incorreta de dígitos
- **Processo não localizado**
  - CNS válido, porém sem registro na planilha
- **Outras informações**
  - Usuário opta por atendimento humano
  - Coleta do CPF (sem pontos)
  - Encaminhamento para um servidor responsável

Esses fluxos garantem que **nenhum usuário fique sem resposta**.

---

### 🔟 Encerramento do Atendimento

- O atendimento é finalizado com uma mensagem de agradecimento.
- A sessão é encerrada de forma segura.
- Um novo atendimento pode ser iniciado a qualquer momento, com variáveis limpas automaticamente.

---


## 🗂 Estrutura do JSON

O arquivo JSON contém:

- **Eventos**
  - `start`: ponto inicial do bot

- **Groups (Grupos de Blocos)**
  - Reset de variáveis
  - Saudação
  - Menu de escolhas
  - Validação de CNS
  - Termos LGPD
  - Consulta ao Google Sheets
  - Exibição de resultados
  - Fluxos de erro e alternativos
  - Encerramento

- **Variáveis**
  | Variável | Descrição |
  |--------|----------|
  | `cns_paciente` | CNS digitado pelo usuário |
  | `cpf_paciente` | CPF para atendimento humano |
  | `resposta_bot` | Status retornado da planilha |
  | `nome_paciente` | Nome do paciente (opcional) |

---

## 🔐 LGPD e Segurança

O fluxo foi projetado para estar em conformidade com a **LGPD**, incluindo:

- Uso exclusivo dos dados para atendimento
- Dados tratados como **informação não pública** e entregue o status administrativo do processo sem envolver outros dados sensíveis
- Reset automático de variáveis por sessão
- Comunicação clara com o usuário sobre o uso das informações

---

## 🔧 Como Usar

- Faça o download do arquivo JSON (typebot-export-autoriza-o-utxoof2.json) disponível neste repositório
- Vá em https://typebot.io/ crie uma conta, caso já tenha acesse o "dashboard"
- Crie um novo bot em "Create a typebot"
- Ao criar um novo bot aparecerá a opção "Import file"
- Nessa opção, ao abrir a janela você irá procurar o arquivo JSON que foi baixado e irá seleciona-lo.
- O fluxo do bot irá aparecer conforme descrito neste projeto e por fim basta integrar o arquivo com o google sheets que será por você utilizado.
  - Esse google sheets está no bloco "Consulta status" selecione "Get data from sheet"
  - Em "select row" mantenha em "all" e adicione um filtro em "+add filter rule"
  - Em "+add filter rule" digite na primeira linha "CNS" ou "CPF" , se for usar o número do CPF, selecione "Equals to" para procurar o valor exato do CNS e atribua a variável "cns_paciente".
- Para usar esse chatbot no whatsapp é preciso usar uma ferramenta de integração, no próprio site do typebot existe uma opção paga, vale lembrar que os serviços de integração são em sua maioria pagos.
  - DISCLAIMER!: Existem APIs de integração gratuitas, mas que correm o risco do número ser banido do Whatsapp, é indicado usar a integração fornecida pelo site Typebot. 

