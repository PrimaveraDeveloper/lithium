# Tax Authority Portugal (TAXPT)

Esta biblioteca - `Primavera.Lithium.TaxAuthority.Portugal.dll` - disponibiliza as operações de integração com os serviços da Autoridade Tributária de Portugal (AT).

Esta versão disponibiliza a integração com dois serviços:

- Comunicação de faturas.
- Comunicação de documentos de transporte (guias).

## Inicialização da biblioteca

Para utilizar qualquer um dos serviços da AT, primeiro é necessário inicializar uma instância da classe `TaxAuthorityClient` (o ponto de entrada da biblioteca).

Eis um exemplo:

```csharp
using (TaxAuthorityClient client = this.GetServiceClient())
{
    // (...)
}

private TaxAuthorityClient GetServiceClient()
{
    TaxAuthorityClient result = new TaxAuthorityClient(
        async (args) =>
        {
            return await ClientCredentials
                .ForScope(
                    new Uri(args.Authority),
                    "{client-id}",
                    "{client-secret}",
                    "{scope}")
                .RetrieveAccessTokenAsync()
                .ConfigureAwait(false);
        });

    result.Configuration.Authentication.UserName = "{user-name}";
    result.Configuration.Authentication.Password = "{password}";
    result.Configuration.CertificatesService.ServiceBaseUri = "{certificates-address}";
    result.Configuration.CertificatesService.LocalMemoryCacheEnabled = true;
    result.Configuration.CertificatesService.LocalStorageCacheEnabled = true;

    result.Configuration.Endpoints.UseTestingEndpoints = false;
}
```

### Certificates Service

Para comunicar com os serviços da AT são necessários certificados que a bibliteca obtém de um micro serviço da PRIMAVERA chamado "Certificates Service" (ver capítulo sobre certificados mais adiante).

Isso implica definir na configuração do cliente o endereço desse serviço (variável `{certificates-address}`). Tipicamente será o endereço do serviço produtivo: [https://lithium-certificates.primaverabss.com](https://lithium-certificates.primaverabss.com).

### Autorização

Para comunicar com a API do Certificates Service a biblioteca necessita de credenciais específicas, definidas no construtor do cliente. São as seguintes variáveis no exemplo anterior:

- `{client-id}`: o identificador da aplicação cliente.
- `{client-secret}`: o segredo associado a essa aplicação cliente.
- `{scope}`: o scope necessário (`lithium-certificates`).

Os valores corretos destas variáveis dependem da aplicação que estiver a utilizar a biblioteca. Pode ser o ERPv10, o Jasmin, o ROSE, etc. Essas aplicações têm estas credenciais e podem ter formas de as disponibilizar (ou não) a aplicações de terceiros.

### Autenticação

Os serviços da AT requerem a autenticação do utilizador dos seus serviços, que identifica a organização em nome da qual são realizas as comunicações. São as seguintes variáveis no exemplo:

- `{user-name}`: o identificador do utilizador (atribuído pela AT).
- `{password}`: a password associada a esse utilizador.

### Endpoints de teste

Os serviços da AT disponibilizam endpoints produtivos e endpoints para teste. A biblioteca pode ser utilizada para comunicar com uns ou com outros, dependendo da configuração `Configuration.Endpoints.UseTestingEndpoints`.

## Comunicação de faturas

Existem duas formas distintas para comunicar uma fatura à AT:

- Numa única operação, em que se constroi o objeto que representa a fatura a comunicar e se invoca a operação que a comunica imediatamente.
- Em dois passos, um primeiro em que se constroi o objeto da fatura e se invoca a operação que retorna o XML que descreve a comunicação, para depois invocar uma outra operação que aceita esse XML e faz a comunicação.

> Deve preferir-se o primeiro modo (até porque também permite ter acesso ao XML depois da comunicação ser concluída). O modo em dois passos existe exclusivamente por razões de retro-compatibilidade e para suportar cenários de diagnóstico ao próprio XML da comunicação.

Eis um exemplo de uma comunicação direta:

```csharp
using (TaxAuthorityClient client = this.GetServiceClient())
{
    try
    {
        InvoiceData invoice = (...);

        ServiceOperationResult<InvoiceRegistrationResult> registration =
            await client.Invoices.RegisterInvoiceAsync(
                invoice)
                .ConfigureAwait(false);

        InvoiceRegistrationResult result = registration.Body;

        if (result.Succeeded)
        {
            // (...)
        }
        else
        {
            // (...)
        }
    }
    catch (ServiceException ex)
    {
        // (...)
    }
}
```

> Note-se que qualquer erro durante a comunicação lançará uma exceção do tipo `ServiceException`. Um exemplo disso, será o caso do serviço da AT não responder ao pedido.

> A comunicação junto da AT pode ser completada e ainda assim o documento não ser aceite (por exemplo, por algum erro de validação). Nesse caso, não é lançada uma exceção e os dados em `InvoiceRegistrationResult` incluirão informação sobre esse erro.

## Comunicação de documentos de transporte

Como acontece para as faturas, também é possível comunicar documentos de transporte usando os mesmos dois modos distintos.

Eis um exemplo de uma comunicação num único passo:

```csharp
using (TaxAuthorityClient client = this.GetServiceClient())
{
    try
    {
        DispatchData dispatch = (...);

        ServiceOperationResult<DispatchRegistrationResult> registration =
            await client.Dispatches.RegisterDispatchAsync
                (dispatch)
                .ConfigureAwait(false);

        DispatchRegistrationResult result = registration.Body;

        if (result.Succeeded)
        {
            // (...)
        }
        else
        {
            // (...)
        }
    }
    catch (ServiceException ex)
    {
        // (...)
    }
}
```

## Certificados

Como foi referido antes, os serviços da AT requerem certificados para proteger a comunicação e para identificar o "publisher" (o software certificado).

A biblioteca usa os seguintes certificados:

- `TaxServicePTPublicKey`: a chave pública da AT.
- `TaxServicePTPRIMAVERAPrivateKey`: a chave privada da PRIMAVERA (utilizada para comunicação com os endpoints produtivos).
- `TaxServicePTTestPrivateKey`: a chave privada para testes (utilizada para comunicação com os endpoints de testes).

> Não são utilizados quaisquer outros certificados em quais outras circunstâncias. Por exemplo, os certificados necessários para assegurar a chain dos certificados referidos, não são utilizados na biblioteca porque a sua utilização é da responsabilidade do servidor (os servidores da AT), não do cliente.

### Certificates Service

A biblioteca comunicará com este micro serviço periodicamente para garantir a atualização de todos os certificados.

Essa comunicação é otimizada por um mecanismo de caching no cliente com dois níveis, que ainda assim garante que, sempre que existir uma nova versão de qualquer dos certificados no micro serviço, ela ficará rapidamente disponível no cliente.

Os níveis de caching são os seguintes, por ordem:

1. Cache em memória (RAM) com um tempo de refresh de 1 hora.
2. Cache em disco (isolated storage) com um tempo de refresh de 24 horas.

Ambas as caches podem ser ligadas e desligadas por configuração na biblioteca, usando as seguintes opções:

- `Configuration.CertificatesService.LocalMemoryCacheEnabled`
- `Configuration.CertificatesService.LocalStorageCacheEnabled`

### Cache em disco

Se a configuração `LocalStorageCacheEnabled` estiver ativa, o certificado será armazenado em disco numa área de [isolated storage](https://docs.microsoft.com/en-us/dotnet/standard/io/isolated-storage) do sistema operativo.

É importante referir os seguintes aspetos:

- O certificado é armazenado na sua representação binária (não como um X509). Mais, essa representação é protegida por um mecanismo adicional de cifragem.
- Dependendo da máquina onde a biblioteca for executada, a isolated storage pode não estar disponível. Se isso acontecer - o que é identificado por uma exceção específica - a respetiva opção de configuração deve ser desativada.

## Serviços da AT

Os dois serviços da AT estão disponíveis nos seguintes endereços de produção:

- [https://servicos.portaldasfinancas.gov.pt:400/fews/faturas](https://servicos.portaldasfinancas.gov.pt:400/fews/faturas)
- [https://servicos.portaldasfinancas.gov.pt:401/sgdtws/documentosTransporte](https://servicos.portaldasfinancas.gov.pt:401/sgdtws/documentosTransporte)

E nos seguintes endereços de testes:

- [https://servicos.portaldasfinancas.gov.pt:700/fews/faturas](https://servicos.portaldasfinancas.gov.pt:700/fews/faturas)
- [https://servicos.portaldasfinancas.gov.pt:701/sgdtws/documentosTransporte](https://servicos.portaldasfinancas.gov.pt:701/sgdtws/documentosTransporte)

A documentação dos serviços está disponível em:

- [http://info.portaldasfinancas.gov.pt/pt/apoio_contribuinte/Documents/ComunicacaodosdadosdasfaturasaAT.pdf](http://info.portaldasfinancas.gov.pt/pt/apoio_contribuinte/Documents/ComunicacaodosdadosdasfaturasaAT.pdf)
- [http://info.portaldasfinancas.gov.pt/pt/apoio_contribuinte/Documents/Comunicacao_Dados_Documentos_Transporte.pdf](http://info.portaldasfinancas.gov.pt/pt/apoio_contribuinte/Documents/Comunicacao_Dados_Documentos_Transporte.pdf)

## Diagnóstico

Para monitorizar a cache de certificados e a validade dos próprios certificados, a biblioteca disponibiliza ainda a seguinte operação de diagnóstico:

```csharp
using (TaxAuthorityClient client = this.GetServiceClient())
{
    try
    {
        ServiceOperationResult<CertificatesDiagnosticResult> result =
            await client.Diagnostics.CheckCertificatesAsync()
                .ConfigureAwait(false);

        CertificatesDiagnosticResult diagnostics = result.Body;

        // (...)
    }
    catch (ServiceException ex)
    {
        // (...)
    }
}
```

Eis um exemplo do output deste diagnóstico (tipo `CertificatesDiagnosticResult`):

```console
Result:
  Certificate
    CertificateName: TaxServicePTPublicKey
    RetrievedFrom..: LocalMemory
    NotBefore......: 24/07/2017 14:51:08
    NotAfter.......: 23/07/2020 14:51:08
  Certificate
    CertificateName: TaxServicePTPRIMAVERAPrivateKey
    RetrievedFrom..: LocalMemory
    NotBefore......: 22/05/2019 09:01:25
    NotAfter.......: 21/05/2021 09:01:25
  Certificate
    CertificateName: TaxServicePTTestPrivateKey
    RetrievedFrom..: LocalMemory
    NotBefore......: 28/11/2019 10:39:35
    NotAfter.......: 26/05/2020 11:39:35
  LocalMemoryCacheEnabled.: True
  LocalStorageCacheEnabled: True
```

> IMPORTANTE: este diagnóstico está disponível apenas a partir da versão 2.0.1.25 da biblioteca.