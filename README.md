## VetCare - Sistema de Clínica Veterinária

### Cenário

A VetCare é uma clínica veterinária de pequeno porte especializada no atendimento de animais domésticos, oferecendo serviços de consultas, vacinação, exames laboratoriais e outros procedimentos veterinários, com o objetivo de promover a saúde e o bem-estar dos animais.

A clínica realiza o cadastro dos **tutores** e de seus respectivos **animais**, armazenando informações pessoais, dados de contato e características dos animais atendidos. Cada animal possui um prontuário individual, no qual são registradas informações clínicas, alergias e observações relevantes para o acompanhamento de sua saúde.

As **consultas** são realizadas por **veterinários** cadastrados na clínica, responsáveis por avaliar os animais, registrar diagnósticos e indicar os **procedimentos** necessários. Durante uma consulta, podem ser realizados diversos procedimentos, como aplicação de vacinas, exames laboratoriais e tratamentos específicos.

Atualmente, a clínica realiza o gerenciamento dessas informações utilizando planilhas eletrônicas e fichas de papel, dificultando a organização dos registros, a consulta ao histórico clínico dos animais e o controle dos atendimentos realizados.

Com o aumento da demanda por atendimentos, a VetCare identificou a necessidade de implementar um sistema informatizado de banco de dados capaz de centralizar as informações, garantir a integridade dos registros, facilitar o acesso aos dados e proporcionar maior eficiência no gerenciamento de suas atividades.

#### Os modelos de negócio serão:
-   Um tutor pode possuir vários animais.
    
-   Cada animal pertence a apenas um tutor.
    
-   Cada animal possui exatamente um prontuário.
    
-   Cada prontuário pertence exclusivamente a um animal.
    
-   Um animal pode realizar várias consultas.
    
-   Cada consulta está associada a apenas um animal.
    
-   Um veterinário pode realizar várias consultas.
    
-   Cada consulta é realizada por apenas um veterinário.
    
-   Uma consulta pode incluir vários procedimentos.
    
-   Um procedimento pode ser realizado em várias consultas.
    
-   Um tutor pode possuir vários telefones.
    
-   O CPF dos tutores e o CRMV dos veterinários não podem ser duplicados.
    
-   A idade do animal deve ser calculada a partir da sua data de nascimento.

## Modelagem Conceitual
O modelo conceitual identificou as seguintes entidades:
![Modelo Conceitual](https://i.imgur.com/KCf4kn5.jpeg)

As entidades identificadas na modelagem conceitual foram:
**Tutor - Animal - Prontuário - Consulta - Veterinário - Procedimento**

## Modelagem Lógica
A conversão do que foi estabelecido no modelo conceitual ficará assim:
![Modelo Lógico](https://i.imgur.com/HfP1Zlw.jpeg)
**TUTOR** (**id_tutor (PK)**, nome_tutor, cpf, email_tutor, rua, numero, bairro, cidade, uf, cep)

**TELEFONE_TUTOR** (**id_telefone (PK)**, **id_tutor  (FK)**, numero_telefone)

**ANIMAL** (**id_animal (PK)**, **id_tutor  (FK)**, nome_animal, especie_animal, raca_animal, sexo_animal, data_nascimento)

**PRONTUARIO** (**id_prontuario (PK)**, **id_animal (FK)**, data_abertura, alergias, observacoes)

**VETERINARIO** (**id_veterinario (PK)**, nome, crmv, especialidade)

**CONSULTA** (**id_consulta (PK)**, **id_animal (FK)**, **id_veterinario (FK)**, data_hora, motivo, diagnostico, status)

**PROCEDIMENTO** (**id_procedimento (PK)**, nome, descricao, valor)

**CONSULTA_PROCEDIMENTO** (**id_consulta (FK), id_procedimento (FK)**, quantidade, valor_unitario)

## Modelagem Física
O Banco de dados físico ficará assim: 
![Modelagem física](https://i.imgur.com/RLjjKCr.png)

Criação das tables:
![enter image description here](https://i.imgur.com/5s8N1t5.png)
![enter image description here](https://i.imgur.com/2eZWmMW.png)

## Códig SQL do Supabase para a criação total do Banco de Dados:

    CREATE SCHEMA vetcare;

    CREATE TABLE vetcare.tutor(
    id_tutor INT PRIMARY KEY,
    nome_tutor VARCHAR(100) NOT NULL,
    cpf VARCHAR(14) NOT NULL,
    email_tutor VARCHAR(120) NOT NULL,
    rua VARCHAR(100) NOT NULL,
    numero VARCHAR(10) NOT NULL,
    bairro VARCHAR(80) NOT NULL,
    cidade VARCHAR(80) NOT NULL,
    uf CHAR(2) NOT NULL,
    cep VARCHAR(9) NOT NULL
    );

    CREATE TABLE vetcare.telefone_tutor (
    id_telefone INT PRIMARY KEY,
    id_tutor INT NOT NULL REFERENCES vetcare.tutor(id_tutor),
    numero_telefone VARCHAR(15) NOT NULL
    );

    CREATE TABLE vetcare.animal (
    id_animal INT PRIMARY KEY,
    id_tutor INT NOT NULL REFERENCES vetcare.tutor(id_tutor),
    nome_animal VARCHAR(60) NOT NULL,
    especie_animal VARCHAR(30) NOT NULL,
    raca_animal VARCHAR(60) NOT NULL,
    sexo_animal CHAR(1) NOT NULL CHECK (sexo_animal IN ('M','F')),
    data_nasc DATE NOT NULL
    );

    CREATE TABLE vetcare.veterinario(
    id_veterinario INT PRIMARY KEY NOT NULL,
    nome_veterinario VARCHAR(150) NOT NULL,
    crmv VARCHAR(20) NOT NULL UNIQUE,
    especialidade VARCHAR (80) NOT NULL
    );

    CREATE TABLE vetcare.prontuario(
    id_prontuario INT PRIMARY KEY NOT NULL,
    id_animal INT NOT NULL REFERENCES vetcare.animal(id_animal),
    data_abertura DATE NOT NULL,
    alergias TEXT,
    observacoes TEXT
    );

    CREATE TABLE vetcare.consulta(
    id_consulta INT PRIMARY KEY NOT NULL,
    id_animal INT NOT NULL REFERENCES vetcare.animal(id_animal),
    id_veterinario INT NOT NULL REFERENCES vetcare.veterinario(id_veterinario),
    data_hora TIMESTAMPTZ NOT NULL,
    motivo VARCHAR(150) NOT NULL,
    diagnostico TEXT,
    status VARCHAR(20) NOT NULL CHECK (status IN ('Agendada', 'Realizada', 'Cancelada'))
    );

    CREATE TABLE vetcare.procedimento(
    id_procedimento INT PRIMARY KEY NOT NULL,
    nome_procedimento VARCHAR(100) NOT NULL,
    descricao TEXT,
    valor DECIMAL(10,2) NOT NULL CHECK (valor >= 0)
    );

    CREATE TABLE vetcare.consulta_procedimento (
    id_consulta INT NOT NULL REFERENCES vetcare.consulta(id_consulta),
    id_procedimento INT NOT NULL REFERENCES vetcare.procedimento(id_procedimento),
    quantidade INT NOT NULL CHECK (quantidade > 0),
    valor_unitario DECIMAL(10,2) NOT NULL CHECK (valor_unitario >= 0)
    );


### Estrutura de dados que entrará no banco.
**Table - Tutor**
![enter image description here](https://i.imgur.com/hHBWx5d.png)

**Table - Telefone - Tutor**
![enter image description here](https://i.imgur.com/dW14Txj.png)

**Table - Animal**
![enter image description here](https://i.imgur.com/qo8FJXn.png)

**Table - Prontuário**
![enter image description here](https://i.imgur.com/Q0mcBtH.png)

**Table - Consulta**
![enter image description here](https://i.imgur.com/czgRduT.png)

**Table - Veterinário**
![enter image description here](https://i.imgur.com/Vm6y4KR.png)

**Table - Procedimento**
![enter image description here](https://i.imgur.com/asPBicN.png)

**Table - Consulta - Procedimento**
![enter image description here](https://i.imgur.com/mSq3HR4.png)

## Dados



## CRUD



## Relatórios.


