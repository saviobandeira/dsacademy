# DESAFIO: Modelo de domínio e ORM

## ESPECIFICAÇÃO - Sistema EVENTO

Deseja-se construir um sistema para gerenciar as informações dos participantes das atividades de um evento acadêmico. As atividades deste evento podem ser, por exemplo, palestras, cursos, oficinas práticas, etc. Cada atividade que ocorre possui nome, descrição, preço, e pode ser dividida em vários blocos de horários (por exemplo: um curso de HTML pode ocorrer em dois blocos, sendo necessário armazenar o dia e os horários de início de fim do bloco daquele dia). Para cada participante, deseja-se cadastrar seu nome e email.

### Modelo conceitual

```mermaid
classDiagram
    class Participante {
        -<<oid>> id : Integer
        -nome : String
        -email : String
    }

    class Atividade {
        -<<oid>> id : Integer
        -nome : String
        -descricao : String
        -preco : Double
    }

    class Categoria {
        -<<oid>> id : Integer
        -descricao : String
    }

    class Bloco {
        -<<oid>> id : Integer
        -inicio : Instant
        -fim : Instant
    }

    Participante "*" -- "1..*" Atividade
    Atividade "*" -- "1" Categoria
    Atividade "1" -- "1..*" Bloco
```

**Papéis e multiplicidades das associações:**

| Classe A     | Papel de A        | Multiplicidade de A | Classe B  | Papel de B     | Multiplicidade de B |
|--------------|-------------------|---------------------|-----------|----------------|---------------------|
| Participante | `- participantes` | `*`                 | Atividade | `- atividades` | `1..*`              |
| Atividade    | `- atividades`    | `*`                 | Categoria | `- categoria`  | `1`                 |
| Atividade    | `- atividade`     | `1`                 | Bloco     | `- blocos`     | `1..*`              |

---

## Instância dos dados para *seeding*

### Diagrama de objetos

```mermaid
graph LR
    p1["<b>p1 : Participante</b><br>id = 1<br>nome = José Silva<br>email = jose@gmail.com"]
    p2["<b>p2 : Participante</b><br>id = 2<br>nome = Tiago Faria<br>email = tiago@gmail.com"]
    p3["<b>p3 : Participante</b><br>id = 3<br>nome = Maria do Rosário<br>email = maria@gmail.com"]
    p4["<b>p4 : Participante</b><br>id = 4<br>nome = Teresa Silva<br>email = teresa@gmail.com"]

    a1["<b>a1 : Atividade</b><br>id = 1<br>nome = Curso de HTML<br>descricao = Aprenda HTML de forma prática<br>preco = 80.00"]
    a2["<b>a2 : Atividade</b><br>id = 2<br>nome = Oficina de Github<br>descricao = Controle versões de seus projetos<br>preco = 50.00"]

    c1["<b>c1 : Categoria</b><br>id = 1<br>descricao = Curso"]
    c2["<b>c2 : Categoria</b><br>id = 2<br>descricao = Oficina"]

    b1["<b>b1 : Bloco</b><br>id = 1<br>inicio = 25/09/2017 08:00:00<br>fim = 25/09/2017 11:00:00"]
    b2["<b>b2 : Bloco</b><br>id = 2<br>inicio = 25/09/2017 14:00:00<br>fim = 25/09/2017 18:00:00"]
    b3["<b>b3 : Bloco</b><br>id = 3<br>inicio = 26/09/2017 08:00:00<br>fim = 26/09/2017 11:00:00"]

    p1 --- a1
    p1 --- a2
    p2 --- a1
    p3 --- a1
    p3 --- a2
    p4 --- a2

    a1 --- c1
    a2 --- c2

    a1 --- b1
    a2 --- b2
    a2 --- b3
```