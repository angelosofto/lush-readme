# Documentação da Aplicação **LUSH**

Bem-vindo à documentação da nossa aplicação **LUSH**! Aqui você encontrará informações detalhadas sobre as 
APIs utilizadas,descrição, design, incluindo os links das páginas, os parâmetros necessários e exemplos de uso.

# Link do Figma

https://www.figma.com/file/uLXPSMLHo8IduNZy5qTgh4/Untitled?node-id=1%3A123

# O projeto da Lush é composto pela aplicação administrativa e site. Segue abaixo os links da aplicações.

# Test

- **APP**
- URL do App Test Lush: https://test.lushmotel.com.br/
- URL do App Test Tout: https://test.toutmotel.com.br/


- **ADMIN**
- URL do Admin Test Lush: http://test-admin.lushmotel.com.br/
- URL do Admin Test Tout: https://testadmin.toutmotel.com.br/

- **Credenciais de Test ADMIN**
user: admin@sof.to
pass: Senh@D1ficil

# Produção

- **APP**
URL do App Lush: https://lushmotel.com.br/
URL do App Tout: https://toutmotel.com.br/


- **ADMIN**
URL do Admin Lush: http://admin.lushmotel.com.br/
URL do Admin Tout: http://admin.toutmotel.com.br/

# Conceitos

## Brand

- Lush, Tout e Andar de Cima são marcas do mesmo dono que utilizam o mesmo codigo, mas geram sites separados.

- Todas vem do mesmo repo, mas buildam com configurações diferentes. APIs tambem separadas.

- Cada brand tem um tema, /src/app/presentation/styles/[brand]-theme.ts

- Alem disso pegamos configurações gerais vindas da API, /src/app/application/services/brand/brand-config, vem o tema de cores, fontes, politicas etc.

## Units

- Lush: Ipiranga e Lapa
- Tout: Campinas

- São lugares diferentes da mesma brand, imagine como se Lush fosse a franquia que tem duas unidades, Ipiranga e Lapa.

## Categories

- São modelos de quartos, com suas caracteristicas.

- Ex.: Lush Ipiranga tem varias categorias, que são as opções de suites, como Lush Spa, Lush Cine...

## Suite

- Nomenclatura não muito usada, referente a uma intanscia de uma categoria, ex.: Temos 4 quartos no modelo Lush Spa, são 4 suites daquela categoria.

## Amenities

- São comodidades que temos nas categorias, como banheira, ar condicionado, etc.

Comentarios:

- Uma amenity pode ou não ser principal (isMain), se ela for, quer dizer que ela possui um icone e é mostrada no card da categoria, se não for, ela é mostrada apenas na tela de detalhes da suite de forma textual.

## Extra packages

- São pacotes adicionais que podemos adicionar na nossa reserva, como decoração, espumante, etc.

## Cupom

- Cupom de desconto, podem ser de valor fixo ou percentual, podem ter limite de uso, podem ser para cpfs especificos, e podem ter limitações de data, só poder usar em dia de semana por exemplo.

## Special date

- Pode ser uma data especial para um usuario, mas normalmente é um evento cadastrado pela empresa, como dia dos namorados.

- Possui preços diferentes.

- Temos tambem landing page para esses eventos, configurada no admin.

## Inventario

- No banco cadastramos quantas suites cada categoria tem, e cadastramos quantas dessas o sistema oferece a venda, o resto fica como uma "reserva" para vendas no local.

- Temos a opção de criar inventario diferenciado, fazendo com que o sistema ofereça mais ou menos suites de uma categoria em determinados dias.

- Ex.: Temos 5 suites da Lush Spa, mas o sistema só oferece 3 delas normalmente. Imagine que duas dessas suites estão em reforma, então nesses dias fazemos um inventario diferenciado para que o sistema ofereça apenas 1 suite da Lush Spa.

## Reservas

- Programada | Schedule - Consulta disponibilidade no Banco de dados.

- Reservas a partir do dia seguinte.

- Periodos: Diaria, DayUse e Pernoite.

- Agora | Go - Consulta disponibilidade no Automos (plataforma interna da Lush e Tout de controle de suíte (manutenção, reforma, ocupação, inventário).).

- Reservas no mesmo dia

- Periodos: 3h, 6h, 12h (depende da unit).

- ReservationTypes podem ser: `schedule` ou `go`.

# Serviços

## Unit List

`/units/reservation/{reservationTypeId}?{query}`

- Serviço retorna lista de unidades com suas categorias.

- Serviço utiliza outro serviço (UnitSuitesAvailability) para pegar a lista de disponibilidade de reserva agora das categorias de cada unidade. Ou seja no caso da Lush é feito a utilização do serviço para pegar a disponibilidade das categorias da Lapa, e é utilizado para a disponibilidade das categorias da Ipiranga.

- Os dados das suites são agregados a essa informação da disponbilidade, `available` e `availableIn`.

- Obs.: `availableIn` pode não estar funcionando corretamente, mas a ideia inicial dele era ser uma informação vinda do automos que diria quanto tempo falta para uma suite daquela categoria estar disponivel.

**Usos:**

- Server side da home para pegar a listagem de todas as unidades. 

- Server side da pagina de detalhes da categoria para pegar a listagem das outras categorias da mesma unidade.

- Server side da pagina de reserva agora/programada para listar as unidades. Tambem sendo usada no client side quando feito algum filtro.

**PATH:**
```
/src/app/application/services/unit/unit-list/unit-list.ts
```

**To Do:**

- Mudar nome do serviço que pega a disponibilidade da reserva agora, e remover ele do serviço de listagem, chamar só quando necessario.

**Método:** GET

**Parâmetros:**

| Parâmetro      | Tipo    | Descrição                                               |
|----------------|---------|---------------------------------------------------------|
| `reservationTypeId` | String  | ID da reserva, pode ser do tipo "go" ou "schedule" (por padrão é "schedule") |
| `query`  | String  | Parâmetros de consulta                                   |

**Retorno da API:**
```json
{
  "id": "string",
  "latitude": "number",
  "longitude": "number",
  "name": "string",
  "slug": "string",
  "suites": [
    {
      "id": "string",
      "images": [
        {
          "id": "string",
          "name": "string",
          "fileName": "string",
          "fileExtension": "string",
          "hasMobile": "boolean",
          "url": "string",
          "urlMobile": "string",
          "ordering": "number"
        }
      ],
      "amenities": [
        {
          "id": "string",
          "name": "string",
          "isMain": "boolean",
          "ordering": "number",
          "url": "string"
        }
      ],
      "description": "string",
      "name": "string",
      "slug": "string",
      "suiteTypeId": "string",
      "unitId": "string",
      "unitName": "string",
      "startingAt": "number",
      "available": "boolean",
      "availableIn": "string"
    }
  ]
}
```

## Unit List Available

`/reservation/amount/{categoryId | null}?selectedDate={year-month-day}`

- Serviço retorna disponibilidade da reserva programada em uma data especifica, para todas as categorias da brand.

**Usos:**

- Carrossel de categorias na home.

- Carrossel de categorias na pagina de detalhes da categoria.

- Carrossel de categorias na pagina de reserva programada para listar as unidades.

**PATH:**
```
/src/app/application/services/reservation/suite-status/suite-status.ts
```

**To Do:**

- Mudar o nome para ficar mais facil de identificar que esse serviço é da reserva programada e o outro da agora.

**Método:** GET

**Parâmetros:**

| Parâmetro       | Obrigatório | Descrição                                 |
|-----------------|-------------|-------------------------------------------|
| `selectedDate`  | Sim         | Data no formato "AAAA-MM-DD".             |

**Parâmetros do Retorno da API:**

| Parâmetro          | Obrigatório | Descrição                            |
|--------------------|-------------|--------------------------------------|
| `unitCategoryId`   | Sim         | ID da categoria da unidade.         |
| `unitId`           | Sim         | ID da unidade.                      |
| `occupied`         | Opcional    | Número de unidades ocupadas.       |
| `limit`            | Opcional    | Limite de unidades.                 |
| `soldOut`          | Opcional    | Indica se está esgotado.           |


**Retorno da API:**
```json
{
    "unitCategoryId": "string",
    "unitId": "string",
    "occupied": "number",
    "limit": "number",
    "soldOut": "boolean"
}

```

## Suite Details

`/unit-categories/{unitId}/{categorySlug}/reservation-type/{reservationTypeId}?selectedDate={year-month-day | null}`

- Serviço retorna todos os dados da categoria, como nome, imagens, videos, amenities, e etc.
Além disso ele consulta outro serviço para pegar a disponibilidade da reserva programada ou agora, dependendo do tipo de reserva.

**Usos:**

- Pagina de detalhes da categoria.

**PATH**
```
/src/app/application/services/suite/suite-details/suite-details.ts
```

**To Do:**

- Unir serviço que busca disponibilidade da reserva programada com o serviço Unit List Available.


**Método:** GET

**Parâmetros:**

| Parâmetro           | Obrigatório | Descrição                          |
|---------------------|-------------|------------------------------------|
| `unitId`            | Sim         | ID da unidade.                    |
| `categorySlug`      | Sim         | Slug da categoria.                |
| `reservationTypeId` | Sim         | ID do tipo de reserva.            |
| `selectedDate`      | Não         | Data no formato "AAAA-MM-DD".     |


**Parâmetros do Retorno da API:**

| Campo                  | Tipo    | Descrição                                  |
|------------------------|---------|--------------------------------------------|
| id                     | String  | ID do objeto.                              |
| unit                   | Object  | Informações sobre a unidade.               |
| unit.id                | String  | ID da unidade.                             |
| unit.name              | String  | Nome da unidade.                           |
| unit.slug              | String  | Slug da unidade.                           |
| unit.latitude          | Number  | Latitude da unidade.                      |
| unit.longitude         | Number  | Longitude da unidade.                     |
| category               | Object  | Informações sobre a categoria.             |
| category.id            | String  | ID da categoria.                           |
| category.name          | String  | Nome da categoria.                         |
| category.slug          | String  | Slug da categoria.                         |
| category.description   | String  | Descrição da categoria.                    |
| category.amenities     | Array   | Lista de comodidades da categoria.         |
| videoUrl               | String  | URL do vídeo.                              |
| available              | Number  | Quantidade disponível.                    |
| unitCategoryImages     | Array   | Lista de imagens da unidade na categoria.  |

**Retorno Array unitCategoryImages**

| Campo           | Tipo    | Descrição                                   |
|-----------------|---------|---------------------------------------------|
| id              | String  | ID da imagem.                               |
| name            | String  | Nome da imagem.                             |
| fileName        | String  | Nome do arquivo da imagem.                  |
| fileExtension   | String  | Extensão do arquivo da imagem.              |
| hasMobile       | Boolean | Indica se a imagem possui versão mobile.   |
| url             | String  | URL da imagem.                              |
| urlMobile       | String  | URL da versão mobile da imagem.             |
| ordering        | Number  | Ordem da imagem na categoria.              |


**Retorno da API:**
```json
{
  "id": "string",
  "unit": {
    "id": "string",
    "name": "string",
    "slug": "string",
    "latitude": "number",
    "longitude": "number"
  },
  "category": {
    "id": "string",
    "name": "string",
    "slug": "string",
    "description": "string",
    "amenities": [
      {
        "id": "string",
        "name": "string",
        "isMain": "boolean",
        "ordering": "string",
        "url": "string"
      }
    ]
  },
  "videoUrl": "string",
  "available": "number",
  "unitCategoryImages": [
    {
      "id": "string",
      "name": "string",
      "fileName": "string",
      "fileExtension": "string",
      "hasMobile": "boolean",
      "url": "string",
      "urlMobile": "string",
      "ordering": "string"
    }
  ]
}
```

## Reservation Period Options

`/reservation-periods/?reservationTypeId={reservationTypeId}&unitCategoryId={categoryId}&selectedDate={year-month-day | null}`

- Serviço retorna os periodos disponiveis para uma reserva programada ou agora, com seus valores, se tem pacotes adicionais, e informações sobre. O backend ja tratou os valores levando em consideração o dia da semana e se é uma data especial.

**PATH:**
```
/src/app/application/services/reservation/reservation-period-options/reservation-period-options.ts
```

**Usos:**

- Display de escolha de periodo dentro da pagina de detalhes da categoria.

**Método:** GET

**Parâmetros:**

| Parâmetro           | Obrigatório | Descrição                          |
|---------------------|-------------|------------------------------------|
| `reservationTypeId` | Sim         | ID do tipo de reserva.             |
| `categoryId`        | Sim         | ID da categoria.                   |
| `selectedDate`      | Não         | Data no formato "AAAA-MM-DD".      |


**Parâmetros do Retorno da API:**

| Campo                        | Tipo    | Descrição                                       |
|------------------------------|---------|-------------------------------------------------|
| id                           | String  | ID da diária.                                   |
| name                         | String  | Nome da diária.                                 |
| slug                         | String  | Slug da diária.                                 |
| price                        | Number  | Preço da diária.                                |
| isDefault                    | Boolean | Indica se é a diária padrão.                    |
| checkinAndCheckout           | String  | Horário de check-in e check-out.                |
| isSpecialDate                | Boolean | Indica se é uma data especial.                  |
| isExtraPackagesAvailable     | Boolean | Indica se estão disponíveis pacotes adicionais. |

**Retorno da API:**
```json
{
  "data":[
    {
        "id": "string",
        "name": "string",
        "slug": "string",
        "price": "number",
        "isDefault": "boolean",
        "checkinAndCheckout": "string",
        "isSpecialDate": "boolean",
        "isExtraPackagesAvailable": "boolean"
    }
  ]
}
```

## Suite Schedule List

`/reservation/schedule/{categoryId}?selectedYear=2023&selectedMonth=8`

- Serviço retorna os dias indisponiveis do mes, para a categoria especificado.

**Usos:**

- Componente de selecionar o periodo nos detalhes da categoria, para saber se o dia escolhido está bloqueado e se sim, desativar o botão.

- Calendario nos detalhes da categoria na opção de reserva programada para saber quais dias do mês desativar.

**PATH:**
```
/src/app/application/services/suite/suite-reservation-periods/suite-reservation-periods.ts
```

**To Do:**

Mudar o nome do serviço.

```
[
  {
    day: 15,
    soldOut: true
  }
]
```

**Método:** GET

**Parâmetros:**

| Parâmetro           | Obrigatório | Descrição                          |
|---------------------|-------------|------------------------------------|
| `categoryId`        | Sim         | ID da categoria.                   |


**Parâmetros do Retorno da API:**

| Campo    | Tipo     | Descrição                                 |
|----------|----------|-------------------------------------------|
| day      | Number   | Dia do mês.                               |
| soldOut  | Boolean  | Indica se está esgotado para este dia.   |


**Retorno da API:**
```json
{
  "data": [
    {
        "day": 31,
        "soldOut": true
    },
    {
        "day": 1,
        "soldOut": true
    }
  ]
}
```

## Suite Reservation Periods

`/reservation-periods/unit-category/{categoryId}/reservation-type/{reservationTypeId}/list`

- Serviço retorna os periodos daquela categoria em um determinado tipo de reserva. Retorna os valores desses periodos, podendo variar com o dia da semana. Lush tem preços de sexta e sabado diferentes dos outros dias da semana.

**Usos:**

- Tabela de preços na pagina de detalhes da categoria

**PATH:**
```
/src/app/application/services/suite/suite-reservation-periods/suite-reservation-periods.ts
```

**Método:** GET

**Parâmetros:**

| Parâmetro           | Obrigatório | Descrição                          |
|---------------------|-------------|------------------------------------|
| `categoryId`        | Sim         | ID da categoria.                   |
| `reservationTypeId` | Sim         | ID do tipo de reserva.             |


**Parâmetros do Retorno da API:**

| Campo                  | Tipo    | Descrição                                  |
|------------------------|---------|--------------------------------------------|
| periods                | Array   | Lista de períodos para o evento.           |
| weekDays               | Array   | Lista de preços por dia da semana.         |


**Retorno do parâmetro periods:**

| Campo                        | Tipo    | Descrição                                       |
|------------------------------|---------|-------------------------------------------------|
| id                           | String  | ID da diária.                                   |
| name                         | String  | Nome da diária.                                 |

**Retorno do parâmetro weekDays:**

| Campo                        | Tipo    | Descrição                                       |
|------------------------------|---------|-------------------------------------------------|
| id                           | String  | ID do primeiro dia da semana.                   |
| name                         | String  | Nome do primeiro dia da semana.                 |
| prices                       | Array   | Lista de preços para o primeiro dia da semana.  |

**Retorno do parâmetro prices:**

| Campo                               | Tipo   | Descrição                         |
|-------------------------------------|--------|-----------------------------------|
| id                                  | String | ID do quarto preço.               |
| price                               | Number | Preço do quarto preço.            |
| name                                | String | Nome do quarto preço.             |

**To Do:**

- Mudar o nome do serviço

```
"periods": [
    {
        "id": "9f6c306b-8aa1-478f-bee6-961507b127f3",
        "name": "Diária"
    },
],
"weekDays": [
    {
        "id": "8b21030f-86a9-4bb5-b6ef-58bf28a05e22",
        "name": "Domingo à Quinta",
        "prices": [
            {
                "id": "c8191c4f-2f9c-4902-a0df-b7619a8fc1a3",
                "price": 1,
                "name": "Diária"
            }
        ]
    },
    {
        "id": "5588b891-0874-406b-9591-a95b62f9553a",
        "name": "Sexta, Sábado e Véspera de Feriado",
        "prices": [
            {
                "id": "b62c1c21-0f6a-4f72-bba4-664f6f2791f6",
                "price": 1,
                "name": "Diária"
            }
        ]
    }
]
```

## Suite Reservation Periods

`/brands/specialdate?reservationTypeId={reservationTypeId}&unitCategoryId={categoryId}`

- Serviço retorna os periodos especiais ativos, quando começam, quando terminam e os valores

**Usos:**

- Tabela de periodo especial na pagina de detalhes da categoria

## Unit Details

- Não utilizado

# Notas

- As propriedades `occupied` e `limit` _NÃO_ estão sendo usadas no App, fazemos checagem a partir do `available` e `soldOut`. O `available` costuma ser da reserva agora, e o `soldOut` da programada.

**PATH:**
```
/src/app/application/services/brand/brand-special-date/brand-special-date.ts
```

**To Do:**

- Deixar esse uso mais claro, checar o modelo `SuiteStatus`

**Método:** GET

**Parâmetros:**

| Parâmetro           | Obrigatório | Descrição                          |
|---------------------|-------------|------------------------------------|
| `categoryId`        | Sim         | ID da categoria.                   |
| `reservationTypeId` | Sim         | ID do tipo de reserva.             |


**Parâmetros do Retorno da API:**

| Campo                   | Tipo    | Descrição                                            |
|-------------------------|---------|------------------------------------------------------|
| id                      | String  | ID do evento do Dia dos Solteiros.                   |
| name                    | String  | Nome do evento.                                      |
| slug                    | String  | Slug do evento.                                      |
| ordering                | Number  | Ordem do evento.                                     |
| notes                   | String  | Notas adicionais sobre o evento.                     |
| periods                 | Array   | Lista de períodos para o evento.                     |

**Retorno do parâmetro period:**
| Campo                   | Tipo    | Descrição                                                          |
|-------------------------|---------|--------------------------------------------------------------------|
| id                      | String  | ID do período.                                                     |
| reservationStartDateUtc | String  | Data e hora de início das reservas (UTC) para o período.           |
| reservationEndDateUtc   | String  | Data e hora de término das reservas (UTC) para o período.          |
| ordering                | Number  | Ordem do período.                                                  |
| prices                  | Array   | Lista de preços para o período.                                    |

**Retorno do parâmetro prices:**
| Campo                   | Tipo    | Descrição                                              |
|-------------------------|---------|--------------------------------------------------------|
| id                      | String  | ID do preço.                                           |
| name                    | String  | Nome do preço.                                         |
| slug                    | String  | Slug do preço.                                         |
| price                   | Number  | Preço do preço.                                        |
| isDefault               | Boolean | Indica se é o preço padrão para o período.             |
| checkinAndCheckout      | String  | Horário de check-in e check-out para o preço.          |

**Retorno da API:**
```json
{
  "id": "string",
  "name": "string",
  "slug": "string",
  "ordering": "number",
  "notes": "string",
  "periods": [
      {
          "id": "string",
          "reservationStartDateUtc": "2023-08-22T03:00:00.000Z",
          "reservationEndDateUtc": "2023-08-24T08:59:59.000Z",
          "ordering": "number",
          "prices": [
              {
                  "id": "string",
                  "name": "string",
                  "slug": "string",
                  "price": "number",
                  "isDefault": "boolean",
                  "checkinAndCheckout": null
              }
          ]
      }
  ]
}
```
