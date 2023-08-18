# Documentação das APIs da Aplicação

Bem-vindo à documentação das APIs da nossa aplicação **LUSH**! Aqui você encontrará informações detalhadas sobre as APIs utilizadas em cada página, incluindo os links das páginas, a descrição das APIs, os parâmetros necessários e exemplos de uso.

## Página: HOME

URL da Página: `https://test.lushmotel.com.br/`

![](https://files.readme.io/3180a5c-image.png)

### Nome da API: Valor da Reserva

Descrição: Esta API é responsável por trazer as reservas com base na data selecionada. Ela fornece informações sobre:
- o ID da categoria
- ID da unidade
- Número de unidades
- Limite de unidades
- Indica se está esgotado ou não

**Endpoint:** `/reservation/amount?selectedDate=2023-08-18`

**Método:** GET

**Parâmetros:**
- `selectedDate` (obrigatório) - Data no formato "AAAA-MM-DD".

**Parâmetros do Retorno da API:**
- `unitCategoryId` (obrigatório) - ID da categoria da unidade.
- `unitId` (obrigatório) - ID da unidade.
- `occupied` (opcional) - Número de unidades ocupadas.
- `limit` (opcional) - Limite de unidades.
- `soldOut` (opcional) - Indica se está esgotado.

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

**Exemplo de Uso:**

```javascript
const axios = require('axios');

const config = {
  method: 'get',
  url: '/reservation/amount?selectedDate=2023-08-18',
  params: {
    selectedDate: '2023-08-18',
  },
  headers: {
    'Authorization': 'Bearer SEU_TOKEN'
  }
};

axios(config)
  .then(function (response) {
    console.log(response.data);
  })
  .catch(function (error) {
    console.log(error);
  });
```

### Nome da API: Lista das Unidades

Descrição: Esta API é responsável por trazer as unidades com base no id da reserva por padrão ele adiciona o id do tipo schedule. Ela fornece informações sobre:

| Parâmetro      | Descrição                                   |
|----------------|---------------------------------------------|
| `id`           | Identificador único da suíte.              |
| `latitude`     | Coordenada de latitude da localização.     |
| `longitude`    | Coordenada de longitude da localização.    |
| `name`         | Nome da suíte.                              |
| `slug`         | Nome amigável para URLs da suíte.           |
| `suites`       | Detalhes das suítes (array).               |

**Endpoint:** `/units/reservation/:reservationId?queryParams`

**Método:** GET

**Parâmetros:**
| Parâmetro  | Tipo    | Descrição                         |
|------------|---------|-----------------------------------|
| `reservationId`  | String  | ID da reserva pode ser do tipo "go" ou "schedule" por padrão é schedule |
| `queryParams`  | String  | Parâmetros de consulta |

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

**Exemplo de Uso:**

```javascript
const axios = require('axios');

const config = {
  method: 'get',
  url: '/units/reservation/{reservationId}/?{queryParams}',
  params: {
    reservationId: 'reservationId'
    queryParams: 'date=2023-08-18',
  },
  headers: {
    'Authorization': 'Bearer SEU_TOKEN'
  }
};

axios(config)
  .then(function (response) {
    console.log(response.data);
  })
  .catch(function (error) {
    console.log(error);
  });
```

### Nome da API: Verificação da disponibilidade das suítes em cada unidade

Descrição: Esta API é responsável por verificar quais suítes estão disponíveis em cada unidade ou seja na Ipiranga e na Lapa. Ela fornece informações sobre:

| Parâmetro      | Descrição                                   |
|----------------|---------------------------------------------|
| `id`           | Identificador único da suíte.              |
| `latitude`     | Coordenada de latitude da localização.     |
| `longitude`    | Coordenada de longitude da localização.    |
| `name`         | Nome da suíte.                              |
| `slug`         | Nome amigável para URLs da suíte.           |
| `suites`       | Detalhes das suítes (array).               |

**Endpoint:** `/units/unitId/next-availability-time`

**Método:** GET

**Parâmetros:**
| Parâmetro  | Tipo    | Descrição                         |
|------------|---------|-----------------------------------|
| `unitId`   | String  | ID da unidade                     |

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

**Exemplo de Uso:**

```javascript
const axios = require('axios');

const config = {
  method: 'get',
  url: '/units/reservation/{reservationId}/?{queryParams}',
  params: {
    reservationId: 'reservationId'
    queryParams: 'date=2023-08-18',
  },
  headers: {
    'Authorization': 'Bearer SEU_TOKEN'
  }
};

axios(config)
  .then(function (response) {
    console.log(response.data);
  })
  .catch(function (error) {
    console.log(error);
  });
```
