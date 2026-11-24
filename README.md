
## VetCare - Sistema de Clínica Veterinária

### Cenário

A VetCare é uma clínica veterinária de pequeno porte especializada no atendimento de animais domésticos, oferecendo serviços de consultas, vacinação, exames laboratoriais e outros procedimentos veterinários, com o objetivo de promover a saúde e o bem-estar dos animais.

A clínica realiza o cadastro dos **tutores** e de seus respectivos **animais**, armazenando informações pessoais, dados de contato e características dos animais atendidos. Cada animal possui um prontuário individual, no qual são registradas informações clínicas, alergias e observações relevantes para o acompanhamento de sua saúde.

As **consultas** são realizadas por **veterinários** cadastrados na clínica, responsáveis por avaliar os animais, registrar diagnósticos e indicar os **procedimentos** necessários. Durante uma consulta, podem ser realizados diversos procedimentos, como aplicação de vacinas, exames laboratoriais e tratamentos específicos.

Atualmente, a clínica realiza o gerenciamento dessas informações utilizando planilhas eletrônicas e fichas de papel, dificultando a organização dos registros, a consulta ao histórico clínico dos animais e o controle dos atendimentos realizados.

Com o aumento da demanda por atendimentos, a VetCare identificou a necessidade de implementar um sistema informatizado de banco de dados capaz de centralizar as informações, garantir a integridade dos registros, facilitar o acesso aos dados e proporcionar maior eficiência no gerenciamento de suas atividades.

###  Modelagem Conceitual
O modelo conceitual identificou as seguintes entidades:
![Modelo Conceitual](https://i.imgur.com/KCf4kn5.jpeg)


### Modelagem Lógica

![Modelo Lógico](https://i.imgur.com/HfP1Zlw.jpeg)
**TUTOR** (**id_tutor (PK)**, nome_tutor, cpf, email_tutor, rua, numero, bairro, cidade, uf, cep)

**TELEFONE_TUTOR** (**id_telefone (PK)**, **id_tutor  (FK)**, numero_telefone)

**ANIMAL** (**id_animal (PK)**, **id_tutor  (FK)**, nome_animal, especie_animal, raca_animal, sexo_animal, data_nascimento)

**PRONTUARIO** (**id_prontuario (PK)**, **id_animal (FK)**, data_abertura, alergias, observacoes)

**VETERINARIO** (**id_veterinario (PK)**, nome, crmv, especialidade)

**CONSULTA** (**id_consulta (PK)**, **id_animal (FK)**, **id_veterinario (FK)**, data_hora, motivo, diagnostico, status)

**PROCEDIMENTO** (**id_procedimento (PK)**, nome, descricao, valor)

**CONSULTA_PROCEDIMENTO** (**id_consulta (FK), id_procedimento (FK)**, quantidade, valor_unitario)

### Modelagem Física


### Dados



### CRUD



### Relatórios.

