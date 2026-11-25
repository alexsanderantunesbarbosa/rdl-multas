# Repositório das Letras — Multas

Microsserviço de **Multas** do sistema [Repositório das Letras](https://github.com/kauanysartor/Repositorio-das-Letras). O documento de requisitos e o descritivo técnico completo estão no repositório principal.

Registra multas e pagamentos. Ao lançar ou pagar uma multa, altera a dívida do leitor.


## Endpoints

| Método | Rota                              | Descrição                                                        |
|--------|-----------------------------------|------------------------------------------------------------------|
| POST   | `/api/multas`                     | Lança multa (aciona integração 3)                                |
| PUT    | `/api/multas/{id}/pagamento`      | Registra pagamento (aciona integração 3)                         |
| GET    | `/api/multas?leitorId=&situacao=` | Lista multas por leitor e por situação (`Pendente` ou `Paga`)    |

Documentação interativa (Scalar) em `/scalar`.

## Integrações

| # | Tipo               | Serviço chamado                                                   | Rota                              |
|---|--------------------|-------------------------------------------------------------------|-----------------------------------|
| 3 | Alteração de dados | [Leitores](https://github.com/alexsanderantunesbarbosa/rdl-leitores) | `PATCH /api/leitores/{id}/divida` |

Ao lançar uma multa, soma o valor à dívida do leitor e muda a situação para bloqueado. No pagamento, reduz a dívida e libera o leitor se ela zerar. A chamada fica na classe `LeitorClient`, com a URL lida da configuração.

## Configuração

| Variável                               | Descrição                                   | Padrão local              |
|----------------------------------------|---------------------------------------------|---------------------------|
| `LeitoresUrl`                          | URL base do serviço de Leitores             | `http://localhost:5002`   |
| `ConnectionStrings__DefaultConnection` | Conexão com o banco SQLite                  | `Data Source=Multas.db`   |
| `PORT`                                 | Porta informada ao Render (usar `8080`)     | —                         |

No Render, `LeitoresUrl` deve apontar para a URL pública (`https://...onrender.com`) do serviço de Leitores.

## Como executar localmente

Subir Leitores antes. Depois:

```
cd AppTemplate
dotnet run
```

Acessar `http://localhost:5004/scalar`.

## Docker

```
docker build -t rdl-multas .
docker run -p 8080:8080 -e LeitoresUrl=<url-leitores> rdl-multas
```

## Outros microsserviços

- [Livros](https://github.com/alexsanderantunesbarbosa/rdl-livros)
- [Leitores](https://github.com/alexsanderantunesbarbosa/rdl-leitores)
- [Empréstimos](https://github.com/alexsanderantunesbarbosa/rdl-emprestimos)
