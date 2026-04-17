UC01 - Cadastrar Aluno

Ator: Recepcionista

Descrição: cadastrar um novo aluno no sistema

Fluxo:

Recepcionista insere dados pessoais
Sistema valida CPF e dados
Sistema salva aluno
UC02 - Realizar Matrícula

Ator: Recepcionista

Descrição: vincular aluno a um plano

Fluxo:

Selecionar aluno
Escolher plano
Definir datas
Sistema cria matrícula
UC03 - Registrar Pagamento

Ator: Recepcionista

Descrição: registrar pagamento de mensalidade

Fluxo:

Selecionar matrícula
Informar valor e método
Sistema registra pagamento
Sistema atualiza status
UC04 - Controlar Acesso

Ator: Sistema

Descrição: liberar ou negar entrada do aluno

Fluxo:

Ler RFID do aluno
Verificar situação
Registrar acesso (entrada ou negado)
UC05 - Agendar Aula

Ator: Aluno

Descrição: reservar vaga em aula

Fluxo:

Escolher aula
Verificar vagas
Confirmar agendamento
Sistema registra
UC06 - Registrar Presença

Ator: Instrutor

Descrição: registrar presença do aluno

Fluxo:

Selecionar aula
Marcar presença
Sistema salva registro
UC07 - Realizar Avaliação Física

Ator: Instrutor

Descrição: registrar dados físicos do aluno

Fluxo:

Inserir dados (peso, altura, etc.)
Calcular IMC
Salvar avaliação
UC08 - Enviar Notificação

Ator: Sistema

Descrição: enviar mensagens ao aluno

Fluxo:

Identificar evento (ex: vencimento)
Selecionar canal
Enviar notificação
UC09 - Gerar Relatório

Ator: Gerente

Descrição: gerar relatórios do sistema

Fluxo:

Selecionar tipo de relatório
Definir parâmetros
Sistema gera arquivo
UC10 - Gerenciar Usuários

Ator: Gerente

Descrição: cadastrar e controlar usuários internos

Fluxo:

Inserir dados do usuário
Definir perfil
Ativar ou desativar
UC11 - Gerenciar Aulas

Ator: Instrutor / Gerente

Descrição: criar ou editar aulas

Fluxo:

Inserir dados da aula
Definir vagas e horário
Sistema salva
UC12 - Atualizar Situação do Aluno

Ator: Sistema

Descrição: atualizar status do aluno automaticamente

Fluxo:

Verificar pagamentos
Atualizar situação (ativo, inadimplente, bloqueado)
Diagrama de Caso de Uso (PlantUML)
@startuml
left to right direction

actor Aluno
actor Recepcionista
actor Instrutor
actor Gerente
actor Sistema

rectangle "FitPass System" {

  Aluno -- (Agendar Aula)
  
  Recepcionista -- (Cadastrar Aluno)
  Recepcionista -- (Realizar Matrícula)
  Recepcionista -- (Registrar Pagamento)

  Instrutor -- (Registrar Presença)
  Instrutor -- (Realizar Avaliação Física)
  Instrutor -- (Gerenciar Aulas)

  Gerente -- (Gerar Relatório)
  Gerente -- (Gerenciar Usuários)

  Sistema -- (Controlar Acesso)
  Sistema -- (Enviar Notificação)
  Sistema -- (Atualizar Situação do Aluno)
}

@enduml
