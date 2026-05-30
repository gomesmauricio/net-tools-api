# API Net Tools

A API Net Tools é um aplicativo Mule implantável que você pode implementar no CloudHub ou em qualquer nuvem de trabalho. O aplicativo expõe uma interface de usuário muito simples que permite executar comandos básicos de rede. A ideia é que a maioria dos problemas de rede relacionados à sua VPC e VPN do CloudHub esteja ligada à conectividade com seus sistemas locais, e a maioria desses problemas acaba sendo resolvida no lado do cliente. Com essa ferramenta disponível, você pode trabalhar com sua equipe de rede para testar a conectividade com vários sistemas locais e verificar se o firewall e as regras de roteamento estão funcionando. Ela também pode ser usada para gerar tráfego que auxilie no diagnóstico de problemas de rede.

![Interface Net-Tools](/interface_net-tools.jpg)

Ela suporta conexões HTTP e HTTPS com uma porta configurável para cada uma.

## Funcionalidades

- Consultas de DNS
- Ping
- TraceRoute
- Abertura de um socket TCP
- Requisição curl simples
- Obtenção de certificados SSL
- Verificação de cifras suportadas para um determinado endpoint SSL/TLS

## Versão mais recente

A versão mais recente pode ser encontrada aqui: https://github.com/mulesoft-labs/net-tools-api/releases

# Uso

A interface do usuário pode ser acessada usando a URL base do aplicativo. As opções estão listadas abaixo.

- Balanceador de Carga Compartilhado do CloudHub: `http://{nome-do-aplicativo}.{região}.cloudhub.io`, onde o nome-do-aplicativo e a região são específicos para o aplicativo implantado.

- Balanceador de Carga Dedicado: `URL personalizada`. Consulte a seção *Configuração* para atualizar as configurações.

A interface do usuário é protegida por Autenticação Básica e as credenciais padrão estão listadas na seção *Configuração*.

# Configuração
As propriedades abaixo podem ser definidas no aplicativo para substituir as configurações padrão. As portas corretas devem ser configuradas para acomodar as configurações do balanceador de carga e das regras de firewall da VPC. As configurações padrão são para o endpoint HTTP do balanceador de carga compartilhado do CloudHub.

- `user`: Nome de usuário para login. O padrão é `vpc-tools`
- `pass`: Senha para login. O padrão é `SomePass`
- `httpPort`: Define a porta de escuta para HTTP. O padrão é `8081`
- `httpsPort`: Define a porta de escuta para HTTPS. O padrão é `8082`
- `httpListener`: O estado de execução dos fluxos do endpoint HTTP. O padrão é `started`. Opções: `started` ou `stopped`. Desative esta opção para desabilitar o endpoint HTTP no CloudHub 1.0 ou em infraestruturas não RTF. Isso não afeta o RTF ou o CloudHub 2.0, pois apenas uma única porta HTTP é usada.

- `ignoreFiles`: Lista de arquivos de recursos solicitados pelo navegador, separados por vírgula, que este aplicativo deve ignorar. O padrão é `favicon.ico`.

## Considerações de Rede

- `httpsPort` e `httpPort` **devem sempre** ser números diferentes, mesmo se `httpListener=stopped`. Isso ocorre porque as configurações de ouvinte HTTP e HTTPS são sempre criadas, mesmo que o endpoint HTTP não esteja habilitado.

- O CloudHub 2.0 e o RTF usam apenas uma única porta para o ouvinte HTTP. Isso significa que você só pode executar HTTP ou HTTPS, mas não ambos ao mesmo tempo. Certifique-se de que a propriedade que você deseja usar esteja definida com a porta correta e a outra esteja definida com uma porta não utilizada.

- Ao usar o CloudHub 2.0 e o RTF, você deve habilitar a *Segurança de Última Milha* na guia Ingress do aplicativo se quiser usar HTTPS.

- Isso não usa as propriedades `http.port` e `https.port`, pois elas são sobrescritas no CloudHub 2.0 e no RTF para a mesma porta e impedirão a inicialização do aplicativo devido a um conflito de portas.

# Referências
- [Considerações sobre a infraestrutura do CloudHub 2.0](https://docs.mulesoft.com/cloudhub-2/ch2-comparison#infrastructure-considerations)
- [Arquitetura do balanceador de carga do CloudHub 1.0](https://docs.mulesoft.com/cloudhub-1/lb-architecture)
- [Habilitar segurança de última milha no RTF](https://help.mulesoft.com/s/article/How-to-Enable-both-Last-Mile-Security-and-Mutual-TLS-in-Runtime-Fabric)

# Manutenção
Este código utiliza as bibliotecas JS abaixo.

- jQuery 1.11.3 [min](https://code.jquery.com/jquery-1.11.3.min.js) e [map](https://code.jquery.com/jquery-1.11.3.min.map).

- [Toastr](https://github.com/CodeSeven/toastr) 2.1.4 [min, map e css](https://cdnjs.com/libraries/toastr.js).
