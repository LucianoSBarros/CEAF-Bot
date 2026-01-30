Typebot CEAF – Consulta de Status (Farmácia de Alto Custo)

Este repositório contém o **fluxo completo em JSON de um Typebot** desenvolvido para automatizar o atendimento da **Farmácia de Alto Custo (CEAF)**, permitindo que pacientes consultem o **status do seu processo** utilizando o **CNS** ou **CPF**, com total conformidade com a **LGPD**.

O projeto foi criado com foco em:
- Automação de atendimento público
- Tratar e dar segurança aos dados públicos que serão dispostos aos solicitantes
- Permitir que atenda e se adapte para a realidade de outras unidades ou órgãos
- Compartilhamento e reutilização por outros desenvolvedores

---

## Objetivo do Projeto

Permitir que o cidadão ou representante:
- Localizar o status do seu processo no CEAF via **CNS**
- Tratar os dados e fornecer apenas o status administrativo
- Certificar que seus dados tratados estejam conforme a **Lei Geral de Proteção de Dados (LGPD)**

---
![Fluxo do Typebot](app.typebot.io_typebots_cmkr0fvxa000ml104iutxoof2_edit.png)


## 🧩 Visão Geral do Fluxo

O fluxo do bot segue os seguintes passos:

1. **Reset de Segurança**
   - Limpa variáveis de sessões anteriores para evitar vazamento de dados.

2. **Saudação Inicial**
   - Apresenta o assistente e pergunta sobre a escolha do atendimento.

3. **Menu de Escolha**
   - Usuário escolhe entre:
     - Verificar status do processo
     - Solicitar outra informação (fluxo humano)

4. **Coleta e Validação do CNS**
   - Solicita o CNS (aplica uma regra de 15 dígitos, sem pontos)
   - A busca pelo processo do paciente ou representante é feita na base de dados por meio do CNS

5. **Aviso LGPD**
   - Informa como os dados serão tratados e utilizados pelo órgão.

6. **Consulta em Google Sheets**
   - Busca o registro do paciente usando o CNS como chave para localizar todo o processo.
   - Integração direta com planilha do Google Sheets

7. **Exibição do Resultado**
   - Retorna o status se for encontrado
   - Oferece nova consulta ou encerramento

9. **Encerramento**
   - Mensagem final de agradecimento


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

- Faça o download do arquivo JSON disponível neste repositório
- Vá em https://typebot.io/ crie uma conta, caso já tenha acesse o "dashboard"
- Crie um novo bot em "Create a typebot"
- Ao criar um novo bot aparecerá a opção "Import file"
- Nessa opção, ao abrir a janela você irá procurar o arquivo JSON que foi baixado e irá seleciona-lo.
- O fluxo do bot irá aparecer conforme descrito neste projeto e por fim basta integrar o arquivo com o google sheets que será por você utilizado.
  - Esse google sheets está no bloco "Consulta status" selecione "Get data from sheet"
  - Em "select row" mantenha em "all" e adicione um filtro em "+add filter rule"
  - Em "+add filter rule" digite na primeira linha "CNS" ou "CPF" , se for usar o número do CPF, selecione "Equals to" para procurar o valor exato do CNS e atribua a variável "cns_paciente".
