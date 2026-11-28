
  

  

## VetCare - Sistema de Clínica Veterinária

  

  

### Cenário

  

  

A VetCare é uma clínica veterinária de pequeno porte especializada no atendimento de animais domésticos, oferecendo serviços de consultas, vacinação, exames laboratoriais e outros procedimentos veterinários, com o objetivo de promover a saúde e o bem-estar dos animais.

  

  

A clínica realiza o cadastro dos **tutores** e de seus respectivos **animais**, armazenando informações pessoais, dados de contato e características dos animais atendidos. Cada animal possui um prontuário individual, no qual são registradas informações clínicas, alergias e observações relevantes para o acompanhamento de sua saúde.

  

  

As **consultas** são realizadas por **veterinários** cadastrados na clínica, responsáveis por avaliar os animais, registrar diagnósticos e indicar os **procedimentos** necessários. Durante uma consulta, podem ser realizados diversos procedimentos, como aplicação de vacinas, exames laboratoriais e tratamentos específicos.

  

  

Atualmente, a clínica realiza o gerenciamento dessas informações utilizando planilhas eletrônicas e fichas de papel, dificultando a organização dos registros, a consulta ao histórico clínico dos animais e o controle dos atendimentos realizados.

  

  

Com o aumento da demanda por atendimentos, a VetCare identificou a necessidade de implementar um sistema informatizado de banco de dados capaz de centralizar as informações, garantir a integridade dos registros, facilitar o acesso aos dados e proporcionar maior eficiência no gerenciamento de suas atividades.

  

  

#### Os modelos de negócio serão:

  

- Um tutor pode possuir vários animais.

  

- Cada animal pertence a apenas um tutor.

  

- Cada animal possui exatamente um prontuário.

  

- Cada prontuário pertence exclusivamente a um animal.

  

- Um animal pode realizar várias consultas.

  

- Cada consulta está associada a apenas um animal.

  

- Um veterinário pode realizar várias consultas.

  

- Cada consulta é realizada por apenas um veterinário.

  

- Uma consulta pode incluir vários procedimentos.

  

- Um procedimento pode ser realizado em várias consultas.

  

- Um tutor pode possuir vários telefones.

  

- O CPF dos tutores e o CRMV dos veterinários não podem ser duplicados.

  

- A idade do animal deve ser calculada a partir da sua data de nascimento.

  

  

## Modelagem Conceitual

  

O modelo conceitual identificou as seguintes entidades:

  

![Modelo Conceitual](https://i.imgur.com/KCf4kn5.jpeg)

  

  

As entidades identificadas na modelagem conceitual foram:

  

**Tutor - Animal - Prontuário - Consulta - Veterinário - Procedimento**

  

  

## Modelagem Lógica

  

A conversão do que foi estabelecido no modelo conceitual ficará assim:

  

![Modelo Lógico](https://i.imgur.com/HfP1Zlw.jpeg)

  

**TUTOR** (**id_tutor (PK)**, nome_tutor, cpf, email_tutor, rua, numero, bairro, cidade, uf, cep)

  

  

**TELEFONE_TUTOR** (**id_telefone (PK)**, **id_tutor (FK)**, numero_telefone)

  

  

**ANIMAL** (**id_animal (PK)**, **id_tutor (FK)**, nome_animal, especie_animal, raca_animal, sexo_animal, data_nascimento)

  

  

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

Ao todo foram **7.650** dados registrados em um total de 8 tabelas no Banco de Dados, sendo eles:

- Tutor: 500 registros

- Veterinario: 500 registros

- Procedimento: 500 registros

- Telefone_tutor: 750 registros

- Animal: 800 registros

- Prontuario: 800 registros

- Consulta: 1.500 registros

- Consulta_procedimento: 2.300 registros

Os arquivos usados para popular o banco de dados estão juntos na pasta de arquivos [VetcareDataBase](https://github.com/MrBartyy/Vet-Care/tree/main/VetcareDataBase), na parte inicial do repositório

  

## CRUD

  

## 6. CRUD

  

Para demonstrar o funcionamento das operações CRUD, foi simulado um fluxo de atendimento veterinário utilizando as oito tabelas do banco de dados VetCare.

  

Inicialmente, foram inseridos os registros referentes ao tutor, telefone, animal, veterinário, prontuário, procedimento e consulta, incluindo a associação entre consulta e procedimento.

  
  

CREATE

  

![enter image description here](https://i.imgur.com/sawgWsI.png)

  

    INSERT INTO vetcare.tutor (

    id_tutor,

    nome_tutor,

    cpf,

    email_tutor,

    rua,

    numero,

    bairro,

    cidade,

    uf,

    cep

    )

    VALUES (

        501,

        'Lucas Pergolato',

        '583.729.146-00',

        'lucas@example.com',

        'Rua das Flores',

        '150',

        'Centro',

        'Franca',

        'SP',

        '14400-100'

    )

    

    RETURNING *;

  
![enter image description here](https://i.imgur.com/e3Le6ck.png)
    INSERT INTO vetcare.telefone_tutor (

    id_telefone,

    id_tutor,

    numero_telefone

    )

    VALUES (

        10001,

        501,

        '(16) 99123-4567'

    )

    RETURNING *;

  ![enter image description here](https://i.imgur.com/usdkq56.png)

    INSERT INTO vetcare.animal (

    id_animal,

    id_tutor,

    nome_animal,

    especie_animal,

    raca_animal,

    sexo_animal,

    data_nasc

    )

    VALUES (

        10001,

        501,

        'Thor',

        'Canina',

        'Golden Retriever',

        'M',

        '2022-05-15'

    )

    RETURNING *;

  ![enter image description here](https://i.imgur.com/gqhdk0i.png)

    INSERT INTO vetcare.veterinario (

    id_veterinario,

    nome_veterinario,

    crmv,

    especialidade

    )

    VALUES (

        10001,

        'Mariana Almeida',

        'CRMV-SP 98765',

        'Clínica Geral'

    )

    RETURNING *;        

  ![enter image description here](https://i.imgur.com/ZTKLUlN.png)

    INSERT INTO vetcare.prontuario (

    id_prontuario,

    id_animal,

    data_abertura,

    alergias,

    observacoes

    )

    VALUES (

        10001,

        10001,

        CURRENT_DATE,

        'Nenhuma',

        'Animal cadastrado para acompanhamento clínico'

    )

    RETURNING *;

  ![enter image description here](https://i.imgur.com/68qSc7v.png)

    INSERT INTO vetcare.procedimento (

    id_procedimento,

    nome_procedimento,

    descricao,

    valor

    )

    VALUES (

        10001,

        'Vacinação antirrábica',

        'Aplicação de vacina contra a raiva',

        150.00

    )

    RETURNING *;

  ![enter image description here](https://i.imgur.com/Nv6awlp.png)

    INSERT INTO vetcare.consulta (

    id_consulta,

    id_animal,

    id_veterinario,

    data_hora,

    motivo,

    diagnostico,

    status

    )

        VALUES (

        10001,

        10001,

        10001,

        '2026-11-30 14:00:00',

        'Vacinação de rotina',

        NULL,

        'Agendada'

    )

    RETURNING *;

![enter image description here](https://i.imgur.com/TE0DDWE.png)
  

    INSERT INTO vetcare.consulta_procedimento (

    id_consulta,

    id_procedimento,

    quantidade,

    valor_unitario

    )

    VALUES (

        10001,

        10001,

        1,

        150.00

    )

RETURNING *;

Em seguida, foram realizadas consultas utilizando JOIN para verificar os registros cadastrados e os relacionamentos existentes entre as tabelas.

  

READ

  

    SELECT *

    FROM vetcare.tutor

    WHERE id_tutor >= 491 and id_tutor <= 501;

    

    SELECT

    tutor.id_tutor,

    tutor.nome_tutor,

    tutor.cpf,

    tutor.email_tutor,

    telefone_tutor.numero_telefone

    FROM vetcare.tutor

    JOIN vetcare.telefone_tutor

    ON tutor.id_tutor = telefone_tutor.id_tutor

    WHERE tutor.id_tutor = 501;

    

    SELECT

    animal.nome_animal,

    animal.especie_animal,

    animal.raca_animal,

    prontuario.data_abertura,

    prontuario.alergias,

    prontuario.observacoes

    FROM vetcare.animal

    JOIN vetcare.prontuario

    ON animal.id_animal = prontuario.id_animal

    WHERE animal.id_animal = 10001;

    

    SELECT

    consulta.id_consulta,

    tutor.nome_tutor,

    animal.nome_animal,

    veterinario.nome_veterinario,

    consulta.data_hora,

    consulta.motivo,

    consulta.status

    FROM vetcare.consulta

    JOIN vetcare.animal

    ON consulta.id_animal = animal.id_animal

    JOIN vetcare.tutor

    ON animal.id_tutor = tutor.id_tutor

    JOIN vetcare.veterinario

    ON consulta.id_veterinario = veterinario.id_veterinario

    WHERE consulta.id_consulta = 10001;

    

    SELECT

    consulta.id_consulta,

    procedimento.nome_procedimento,

    consulta_procedimento.quantidade,

    consulta_procedimento.valor_unitario,

    consulta_procedimento.quantidade *

    consulta_procedimento.valor_unitario AS valor_total

    FROM vetcare.consulta_procedimento

    JOIN vetcare.consulta

    ON consulta_procedimento.id_consulta =

    consulta.id_consulta

    JOIN vetcare.procedimento

    ON consulta_procedimento.id_procedimento =

    procedimento.id_procedimento

    WHERE consulta.id_consulta = 10001;

  

UPDATE

  

As operações de atualização demonstraram a possibilidade de modificar informações cadastrais, registros clínicos, valores de procedimentos e o status dos atendimentos.

  

    UPDATE vetcare.tutor

    SET email_tutor = 'lucas.pergolato@example.com'

    WHERE id_tutor = 501

    RETURNING *;

    

    UPDATE vetcare.telefone_tutor

    SET numero_telefone = '(16) 99876-5432'

    WHERE id_telefone = 10001

    RETURNING *;

    

    UPDATE vetcare.animal

    SET nome_animal = 'Thor Max'

    WHERE id_animal = 10001

    RETURNING *;

    

    UPDATE vetcare.veterinario

    SET especialidade = 'Medicina Preventiva'

    WHERE id_veterinario = 10001

    RETURNING *;

    

    UPDATE vetcare.prontuario

    SET observacoes = 'Animal vacinado e em boas condições de saúde'

    WHERE id_animal = 10001

    RETURNING *;

    

    UPDATE vetcare.procedimento

    SET valor = 180.00

    WHERE id_procedimento = 10001

    RETURNING *;

    

    UPDATE vetcare.consulta_procedimento

    SET valor_unitario = 180.00

    WHERE id_consulta = 10001

    AND id_procedimento = 10001

    RETURNING *;

    

    UPDATE vetcare.consulta

    SET

    status = 'Realizada',

    diagnostico = 'Animal saudável, vacinação realizada com sucesso'

    WHERE id_consulta = 10001

    RETURNING *;

  

  

## Relatórios.