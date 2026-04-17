<img width="1138" height="502" alt="image" src="https://github.com/user-attachments/assets/5315b989-8ef8-4e2a-a856-a9c6c16558f4" />


@startuml FitPass_DomainClassDiagram

title Diagrama de Classes – FitPass Gym Management

skinparam classAttributeIconSize 0
skinparam classFontSize 12
skinparam classHeaderBackgroundColor #DDEEFF
skinparam classBorderColor #5588AA
skinparam arrowColor #335577
skinparam linetype ortho

' ──────────────────────────────────────────
'  ENUMS
' ──────────────────────────────────────────

enum SituacaoAluno {
  ATIVO
  INADIMPLENTE
  BLOQUEADO
}

enum StatusMatricula {
  ATIVA
  CANCELADA
  EXPIRADA
}

enum MetodoPagamento {
  DINHEIRO
  CARTAO
  PIX
  BOLETO
}

enum StatusPagamento {
  PAGO
  PENDENTE
  VENCIDO
}

enum TipoEvento {
  ENTRADA
  NEGADO
}

enum StatusAgendamento {
  CONFIRMADO
  CANCELADO
  FALTA
}

enum TipoNotificacao {
  VENCIMENTO_MENSALIDADE
  CONFIRMACAO_AGENDAMENTO
  LIBERACAO_AVALIACAO
}

enum CanalNotificacao {
  EMAIL
  WHATSAPP
  SMS
  PUSH
}

enum PerfilAcesso {
  RECEPCIONISTA
  INSTRUTOR
  GERENTE
}

enum TipoRelatorio {
  INADIMPLENCIA
  ALUNOS_ATIVOS
  HISTORICO_ACESSOS
  OCUPACAO_AULAS
}

enum TipoPlano {
  MENSAL
  TRIMESTRAL
  ANUAL
  MUSCULACAO
  FUNCIONAL
}

' ──────────────────────────────────────────
'  ALUNO E DADOS PESSOAIS
' ──────────────────────────────────────────

class Aluno {
  - id: UUID
  - nome: String
  - dataNascimento: Date
  - cpf: String
  - rfid: String
  - situacao: SituacaoAluno
}

class Contato {
  - email: String
  - telefone: String
  - whatsapp: String
  - preferencia: CanalNotificacao
}

class Endereco {
  - logradouro: String
  - numero: String
  - complemento: String
  - bairro: String
  - cidade: String
  - uf: String
  - cep: String
}

' ──────────────────────────────────────────
'  PLANO E MATRICULA
' ──────────────────────────────────────────

class Plano {
  - id: UUID
  - nome: String
  - tipo: TipoPlano
  - duracao: Integer
  - valor: Decimal
  - ativo: Boolean
}

class Matricula {
  - id: UUID
  - dataInicio: Date
  - dataFim: Date
  - statusMatricula: StatusMatricula
}

' ──────────────────────────────────────────
'  PAGAMENTO
' ──────────────────────────────────────────

class Pagamento {
  - id: UUID
  - dataPagamento: DateTime
  - valor: Decimal
  - metodoPagamento: MetodoPagamento
  - statusPagamento: StatusPagamento
  - comprovante: String
}

' ──────────────────────────────────────────
'  USUARIO (perfis internos)
' ──────────────────────────────────────────

class Usuario {
  - id: UUID
  - login: String
  - senhaHash: String
  - perfil: PerfilAcesso
  - ativo: Boolean
}

' ──────────────────────────────────────────
'  CONTROLE DE ACESSO
' ──────────────────────────────────────────

class RegistroAcesso {
  - id: UUID
  - dataHora: DateTime
  - tipoEvento: TipoEvento
  - resultado: Boolean
  - motivoNegativa: String
}

' ──────────────────────────────────────────
'  AULAS, AGENDAMENTO E PRESENÇA
' ──────────────────────────────────────────

class Aula {
  - id: UUID
  - nome: String
  - descricao: String
  - dataHoraInicio: DateTime
  - duracao: Integer
  - vagasMax: Integer
  - vagasOcupadas: Integer
}

class Agendamento {
  - id: UUID
  - dataReserva: DateTime
  - statusAgendamento: StatusAgendamento
  - canceladoEm: DateTime
}

class Presenca {
  - id: UUID
  - dataHoraRegistro: DateTime
  - presente: Boolean
  - observacao: String
}

' ──────────────────────────────────────────
'  AVALIAÇÃO FÍSICA
' ──────────────────────────────────────────

class AvaliacaoFisica {
  - id: UUID
  - dataAvaliacao: Date
  - peso: Decimal
  - altura: Decimal
  - imc: Decimal
  - percGordura: Decimal
  - massaMuscular: Decimal
  - observacoes: String
}

class ArquivoAvaliacao {
  - id: UUID
  - nomeArquivo: String
  - urlArmazenamento: String
  - tipoArquivo: String
  - tamanhoBytes: Long
  - dataUpload: DateTime
}

' ──────────────────────────────────────────
'  NOTIFICAÇÃO
' ──────────────────────────────────────────

class Notificacao {
  - id: UUID
  - dataEnvio: DateTime
  - tipo: TipoNotificacao
  - mensagem: String
  - enviado: Boolean
  - canal: CanalNotificacao
}

' ──────────────────────────────────────────
'  RELATÓRIO
' ──────────────────────────────────────────

class Relatorio {
  - id: UUID
  - tipo: TipoRelatorio
  - dataGeracao: DateTime
  - parametros: String
  - urlArquivo: String
}

' ──────────────────────────────────────────
'  UNIDADE
' ──────────────────────────────────────────

class Unidade {
  - id: UUID
  - nome: String
  - cnpj: String
  - ativa: Boolean
}

' ──────────────────────────────────────────
'  RELACIONAMENTOS
' ──────────────────────────────────────────

' Aluno – dados pessoais
Aluno "1" *-- "1" Contato
Aluno "1" *-- "1" Endereco
Aluno "1" -- "1" SituacaoAluno

' Plano – Matricula – Aluno
Plano "1" -- "0..*" Matricula : origina >
Aluno "1" -- "0..*" Matricula : possui >

' Matricula – Pagamento
Matricula "1" -- "0..*" Pagamento : gera >

' Aluno – Acesso
Aluno "1" -- "0..*" RegistroAcesso : gera >

' Aulas
Unidade "1" -- "0..*" Aula : oferece >
Usuario "1" -- "0..*" Aula : ministra >

' Agendamento
Aluno "1" -- "0..*" Agendamento : reserva >
Aula  "1" -- "0..*" Agendamento : possui >

' Presença
Agendamento "1" -- "0..1" Presenca : origina >
Usuario "1" -- "0..*" Presenca : registra >

' Avaliação física
Aluno "1" -- "0..*" AvaliacaoFisica : realiza >
Usuario "1" -- "0..*" AvaliacaoFisica : conduz >
AvaliacaoFisica "1" -- "0..*" ArquivoAvaliacao : anexa >

' Notificação
Aluno "1" -- "0..*" Notificacao : recebe >

' Relatório
Usuario "1" -- "0..*" Relatorio : emite >

' Unidade
Unidade "1" -- "0..*" Aluno : cadastra >
Unidade "1" -- "0..*" Usuario : emprega >

@enduml
