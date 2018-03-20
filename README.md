# Exemplo de utilizacao Client SOAP python Zeep

## Instalacao

```bash
$ pip install zeep
```

## Utilizacao

Primeiramente voce pode verificar as interfaces do webservice a partir do arquivo wsdl. Basta fazer o seguinte:

```bash
$ python -mzeep http://url.do.wsdl
```

### Criando um cliente

para criar um objeto cliente para interagir com o webservice e bem simples:

```python
from zeep import Client

client = Client(wsdl='http://url.do.wsdl')
```

Uma vez o cliente estacionado voce pode mais uma vez verificar as interfaces do webservice usando:

```python
client.wsdl.dump()
```

A partir das interface voces pode executar consultas navegando atraves dos `PortTypes` do webservice.
Supondo de na execucao de `client.wsdl.dump()` voce verificou que o webservice tem 2 `PortTypes`: `Pedido` e `Carrinho`.
Por default, o cliente cria um `bind` com um destes `PortTypes`, por exemplo `Pedido`. Para executar consultas neste `PortType`
basta executar:

```python
client.service.ValorTotal()
```

Supondo de `ValorTotal` e uma operacao do `PortType Pedido`. 

OBS: As operacoes tambem podem ser vistas executando o metodo `dump()` do atributo `wsdl` da instancia do cliente. As operacoes
sao agrupadas por `PortTypes` e nesta descricao pode-se ver tambem os parametros recebidos pelas operacoes e qual o tipo de retorno.

Para executar operacoes/consultas do outro `PortType` voce deve criar um novo `bind` a partir do objeto `client`:

```python
# criacao de bind com o PortType Carrinho
service2 = client.bind('<nome_do_webservice>', 'Carrinho')
```

A partir dai voce podera interagir com o `PortType Carrinho` a partir do objecto `service2`.

```python
service2.RemoverItem()
```

Onde `RemoverItem()` e uma operacao de `Carrinho`.

### Autenticacao

Para efetuar autenticacao no webservice voce pode fazer isso atraves do objeto `Transport` da lib `zeep`. No exemplo abaixo segue autenticacao do tipo Basic.

```python
from zeep import Client
from zeep.transports import Transport
from requests import Session
from requests.auth import HTTPBasicAuth

auth = HTTPBasicAuth(username, password)
session = Session()
session.auth = auth
tranport = Transport(session=session)
client = Client(<wsdl_link>, transport=transport)
```

`zeep` e uma lib de uso simples e extensivel. Para maiores detalhes me ponho a disposicao e tambem fique a vontade para consultar a [documetacao oficial](http://docs.python-zeep.org/en/master/).