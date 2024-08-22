# Roteador

## Comandos Básico

```
Router> enable
```

Acessa o modo privilegiado do roteador

```
Router#configure terminal
```

Habilita os comandos de configuração global

Router(config)# hostname <nome>
Dá um nome ao roteador

### Senhas

```
Router(config)#security password min-length <tamanho>
```

Define um tamanho minímo que a senha tem que ter

```
Router(config)#login block-for <tempo_de_espera> attempts <tentativas> within <tempo>
```

Define o tempo de bloqueio que o usuário irá sofrer caso ele erre a senha 'x' vezes em um espaço período de tempo

```
Router(config)#enable password <senha>
```

Habilita uma senha para acessar o modo configuração

```
Router(config)#enable secret <senha>
```

Habilita uma senha para acessar o modo configuração. É mais seguro
do que o password

```
Router(config)#line console 0
```

Entra no modo de configuração do console 0

```
Router(config)#line console 0
Router(config)# password <senha>
```

Configura uma senha para acessar o console do roteador

```
Router(config)#line vty 0 4
Router(config)#login
Router(config)#password <senha>
```

Configura uma senha para acesso do telnet. Nesse caso foi configurado
para liberar acesso até 5 usuário de 0 a 4.

```
Router(config)#service password-encryption
```

Criptografa todas as passwords colocadas no roteador

### Interface

```
Router(config)#interface fastEthernet0/1(f0/1)
Router(config-if)#ip address <ip> <máscara>
Router(config-if)#no shutdown
```

Configura a interface de rede fastEthernet, com o IP especificado e
mascara de rede. O comando no shutdown serve para colocar a interface
de pé

### Serial

```
Router(config)#interface serial 0/1/0
Router(config-if)#ip address <ip> <máscara>
Router(config-if)#clock rate 2000000 (somente se o serial for o DCE)
Router(config-if)#no shutdown
```

Configuração da inteface serial, existe a divisão DCE e DTE. A DCE é
a que define a velocidade do link em que os dados serão enviados, por
isso o comando clock rate, no caso do DTE essa configuração não se
faz necessária

### Show

```
Router#show ?
```

Fornece a lista de comandos show, que mostram especificações sobre a
configuração

```
Router#show arp
```

Mostra a tabela ARP

```
Router#show ip interface brief
```

Mostra de maneira resumida as configurações das interfaces

```
Router#show ip route
```

Mostra a tabela de roteamento

```
Router#show running-config
```

Mostra as configurações ativas na RAM

```
Router#show startup-config
```

Mostra as configurações ativas na NVRAM

```
Router#show flash
```

Mostra os arquivos de SO do Flash

### Salvar

```
Router#copy ruunin-config startup-config
```

Salva as configurações ativas na RAM para a NVRAM

```
Router#wr
```

Faz o mesmo que o comando anterior

### IPV6

```
Router(config)#ipv6 unicast-routing
```

Habilita as configurações do IPv6

```
Router(config)#interface f0/1
Router(config-if)#ipv6 address <ip>(ex: 2001:db8:1:1::1/64)
Router(config-if)#ipv6 address <ip>(ex: fe80::1) link-local
Router(config-if)#no shutdown
```

O link-local é um endereço de rede válido apenas para a comunicação
no segmento da rede ou no domínio de transmissão no qual o host está
conectado

```
Router(config)#int tun 0
Router(config-if)#ipv6 address <ipv6>
Router(config-if)#tunnel source <ipv4>
Router(config-if)#tunnel destination <ipv4>
Route(config-if)#runnel mode ipv6ip
```

Este comando serve para criar uma interface de tunel, por onde o
endereço ipv6 consegue passar. Esse comando é muito utilizado quando
temos a interface local configurada com o ipv6 e a interface externa
(roteamento) configurada com o ipv4.

```
Route(config)#ipv6 route <ipv6> tun 0
```

Para que o tunel funcione corretamente é necessário configurar uma
rota estática, ou um protocolo de roteamento. Vale lembrar que esse
comando deve ser configurada nos dois roteadores.

#### NATv6

```
Router(config)#int <interface>
Router(config-if)#ipv6 nat
```

Habilita o nat ipv6 na interface

```
Router(config)#ipv6 nat v4v6 source <ipv4> <ipv6>
Router(config)#ipv6 nat v6v4 source <ipv6> <ipv4>
Router(config)#ipv6 nat prefix <ipv6/96>
```

Configurado globalmente o primeiro comando especifica que o nat será
feito do endereço ipv4 para o endereço ipv6, e o segundo comando
vice-versa. Já o último comando está especificando o prefixo do
endereço, algo similar ao que seria a máscara de rede do ipv4.

### Banner

```
Router(config)#banner motd d(#, ou $) <mensagem> d
```

Esse banner será mostrado para todos os terminais conectados. Esse é
o chamado message of the day(motd)

```
Router(config)#banner exec d(#, ou $) <mensagem> d
```

Esse banner exibe uma mensagem toda vez que um processo EXEC é criado
Uma linha é ativada ou estabelece uma conexão via vty nos roteadores

```
Router(config)#banner incoming d(#, ou $) <mensagem> d
```

É exibido após o usuário se logar nos roteadores Cisco, mas somente
para sessões via Telnet Reverso

```
Router(config)#banner login d(#, ou $) <mensagem> d
```

É exibido antes do prompt de username e password

### DNS

```
Router(config)#ip domain-lookup
```

Habilita a utilização do servidor DNS no roteador

```
Router(config)#ip name-server <ip-server-dns>
```

Especifica o endereço IP do servidor DNS

## Syslog

```
Router(config)#logging <servidor>
```

Configura o servidor para onde as mensagens log serão enviadas

```
Router(config)#logging trap <level>
```

Diz o nível das mensagens que serão enviadas. Exitem ao todo 7 níveis:
-Emergency: 0
-Alert: 1
-Critical: 2
-Error: 3
-Warning: 4
-Notice: 5
-Informational: 6
-Debug: 7

```
Router(config)#service timestamps {log|debug} datetime msec
```

Coloca um carimbo de data/hora em todas as mensagens log que serão
enviadas para o servidor

```
Router(config)#show logging
```

Mostra informações sobre as configurações de envio syslog

## DHCP

```
Router(config)#ip dhcp pool <nome>
```

Será o pool de IP's

```
Router(dhcp-config)#network <ip-rede> <mascara>
```

Configura a rede IP. LEMBRE-SE o IP da rede é o que define a rede

```
Router(dhcp-config)#default-router <ip-roteador>
```

Define o gateway padrão

```
Router(dhcp-config)#dns-server <ip-dns>
```

Define o servidor DNS

```
Router(dhcp-config)#ip dhcp excluded-address <ip-inicio> <ip-final>
```

Exclui os IP's da pool. Esses serão os estáticos

```
Router(config)#int <interface>
Router(config-if)#ip helper-address <ip-servidor-dhcp>
```

Esse comando é usado quando temos um servidor DHCP no qual queremos
que seja utilizado para fornecer os endereços aos usuários
Esse comando deve ser utilizado na interface do roteador que irá
distribuir os endereços.

## Protocolos de Roteamento

### Estático

```
Router(config)#ip route <ip-rede> <mascara> <ip-roteador>
```

Os dois IPs colocados se referem a outra rede e o outro roteador
respectivamente.

```
Router(config)#ip route <ip-rede> <mascara> <interface>
```

E o mesmo que o comando acima porém utilizando a interface do outro
roteador ao invés do IP

**OBS:** é possível nesse tipo de configuração utilizar o IP 0.0.0.0 e o
mesmo valor na máscara caso você não saiba o IP que está sendo
utilizado do outro lado. Desse modo qualquer IP é válido.
Utilizando isso o comando fica:

```
Router(config)#ip route 0.0.0.0 0.0.0.0 <interface | ip-roteador>
```

Perceba que esse valor só é válido para a rede, o roteador que fara
parte da rota estática precisa ser identificado

### RIP

```
Router(config)#router rip
Router(config-router)# network <ip>
Router(config-router)# netowrk <ip>
```

Configuração do RIP versão 1. Vale dizer que ao colocarmos esses IPs
nós especificamos o ip da rede que será alcançada e a rede em que os
roteadores estão trabalhando

```
Router(config)#router rip
Router(config-router)#version 2
Router(config-router)#no auto-summary
Router(config-router)#network <ip>
Router(config-router)#network <ip>
Router(config)#ip default-network <ip>
```

Configuração do RIP versão 2. Perceba que existe um comando adicional
o no auto-summary.

A sumarização de uma rede nada mais é do que o processo de reduzir
as redes em uma coisa só. No caso de protocolos como o RIP e o
EIGRP isso não é muito recomendado pois pode trazer muitos conflitos,
o ideal é fazer esse processo manualmente

O último comando ip default-network diz aos outros roteadores qual é a
rede de acesso a Internet.

#### IPv6 

```
Router(config)#int <interface>
Router(config-if)#ipv6 address <address>
Router(config-if)#ipv6 rip <name> enable
```

A configuração do RIP para IPv6 segue a estrutura acima. Perceba que sua configuração é feita diretamente na placa de rede, e não mais como um processo separado. Após configurarmos o endereço da placa o comando `ipv6 rip <name> enable` habilita o roteamento naquela placa em especifíco.

``` 
Router(config-if)#ipv6 rip <name> default-information originate
```

O comando acima é usado para especificar que o roteador sendo configurado possui a rota padrão desta rede. IMPORTANTE: Essa configuração precisa ser feita em todas as outras placas de redes que estejam ligada com um vizinho.

#### Resumo de Uso

Imagine o seguinte Roteador A com as redes 172.16.20.0, 172.16.30.0 e
172.16.40.0.
Roteador B com as redes 172.16.50.0, 172.16.60.0 e 172.16.70.0

Esses dois roteadores estão ligados a um roteador C. Caso esse
roteador tenha que enviar esses endereços para outro roteador, essas
6 redes iriam ocupar muito o processo de processamento, logo fica mais
fácil utilizar somente um endereço para propagação das 6.
É aqui que entra a sumarização Manual

Neste exemplo pegamos a rede maior, 172.16.70.x e subtraimos pela
menor 172.16.20.x. Ficamos então com 50, o bloco válido para isso
seria o que suporta 64 IPs.

Subtraimos então 256 - 64 = 192

Como estamos trabalhando no terceiro octeto, a mascara da rede fica
como 255.255.192.0, vamos então usar essa mascara mais a rede
172.16.10.0

Ficamos com a seguinte 172.16.10.0/18, para propagar as 6 redes.

**Troubleshooting**

```
Router#show ip route rip
```

Mostra as rotas que estão sendo tomadas

```
Router#show ip protocols
```

Mostra o protocolo de roteamento sendo utilizado

```
Router#show ip rip database
```

Mostra as redes que estão sendo utilizadas no rip

```
Router#debug ip rip
```

Testa as conexões rip

### EIGRP

```
Router(config)#Interface loopback 0
Router(config)# ip address <ip> <mascara>
```

Essa configuração é opcional, mas ela é muito boa de ser utilizada.
A vantagem da interface de loopback está no fato de que ela é uma
interface lógica que pode ser alcançada independente das outras
interfaces. No caso desse protocolo ela será usada como ID do
roteador exatamente por esse motivo.

```
Router(config)#router eigrp <ASN>
Router(config-route)#eigrp router-id <ip>
Router(config-route)#network <ip> <máscara-curinga>
Router(config-route)#no auto-summary
Router(config-route)#passive-interface <interface>
```

O primeiro comando para o número AS que você definir, esse valor pode
ser de 1 até 65535 e precisa ser o mesmo nos diversos roteadores da
rede EIGRP.
O comando eigrp router-id é a identificação do roteador
O comando network define as interfaces que participarão do processo
de roteamento, é preciso colocar cada uma das rede
E o último comando passive-interface, serve para especificar a rede
faz parte do EIGRP.

Alguns outros comandos utilizados são

```
Router(config-route)# ip bandwidht-percent <AS> <porcentagem>
```

Coloca o máximo de banda-larga que o EIGRP pode consumir

```
Router(config-route)#ip summary-address eigrp <AS> <ip> <mask>
```

Configura a sumarização de rotas manuais

```
Router(config-route)#ip hello-interval eigrp <AS> <segundos>
Router(config-route)#ip hold-time eigrp <AS> <segundos>
```

Define o timer para enviar mensagens hello e o tempo de espera

**Troubleshooting**

```
Router(config)#show ip eigrp interfaces
```

Mostra as interfaces usando eigrp

```
Router(config)#show ip eigrp neighbors
```

Mostra os vizinhos ao roteador que usamo o eigrp

```
Router(config)#show ip eigrp topology
```

Mostra a topologia da rede que está usando esse protocolo

```
Router(config)#show ip eigrp traffic
```

Mostra o tráfico usado por esse protocolo

```
Router(config)#clear ip eigrp neighbors
```

Limpa a lista de vizinhos

```
Router(config)#debug ip eigrp [packet | neighbors]
```

Testa a conexão do eigrp com os vizinhos

### OSPF

```
Router(config)#interface loopback 0
Router(config-if)#ip address <ip> <mask>
```

Essa configuração é opcional, mas ela é muito boa de ser utilizada.
A vantagem da interface de loopback está no fato de que ela é uma
interface lógica que pode ser alcançada independente das outras
interfaces. No caso desse protocolo ela será usada como ID do
roteador exatamente por esse motivo.

```
Router(config)#router ospf <id>
Router(config-route)#router-id <ip-roteador>
Router(config-route)#network <ip-rede> <máscara-curinga> area <área>
Router(config-route)#passive-interace f0/0
```

O primeiro comando ativa o processo de roteamento OSPF no ID que você
definir. O valor do ID pode ir de 1 a 65535, e não precisa ser o mesmo
nos roteadores da rede.
O router-id identifica a interface do roteador que irá utilizar o
OSPF.
E o último comando define a rede que fará parte do roteamento, e a
área que essas redes farão parte. Para que dois roteadores se
comuniquem eles precisam estar na mesma área
O último comando colocado realiza o mesmo procedimento explicado
no EIGRP

```
Router(config)#router ospf <id>
Router(config)#int <interface>
Router(config-if)#ip ospf <id> area <area>
```

Este comando é usado para habilitar o OSPF em interfaces especificas

```
Router(config-if)#ip osfp authentication-key <senha>
```

Está é uma forma de colocar senha entre as comunicações OSPF. A senha
deve ser igual em ambos os lados que estiverem conversando. Lembrado
que esse comando coloca a senha em texto puro, por isso cuidado.

```
Router(config-if)#ip ospf network broadcast
```

Comando usado para definir o tipo de rede que será usada no OSPF. Se
as redes entre os roteadores estiverem definidas de maneiras
diferentes os roteadores não irão conversar. Para mais tipos de redes
utilize o comando '?'. Caso não esteja tendo probelmas não altere
está configuração.

```
Router(config)#router ospf <id>
Router(config-route)#area <id> virtual-link <interface>
```

Este comando serve para criar um tunel OSPF entre roteadores. Esse
processo é feito pois todas as áreas devem estar em contato com a
área 0, desse modo é criado um túnel sobre uma determinada área para
que todas as áreas estejam em contato com o backbone. Este comando
deve ser feito deve ser feito entre os dois ou mais roteadores que
precisam se comunicar, por padrão na <interface> é usado o ip do
loopback.

#### IPv6 (OSPFv3)

```
Router(config)#ipv6 router ospf <id>
Router(config-rtr)#router-id <rid>
```

Antes de usar esses comandos é necessário habilitar o roteamento de IPv6 com `ipv6 unicast-routing`.
Nesta versão do OSPF o route-id *precisa* ser configurado, pois o protocolo não consegue usar um IP para associar com o seu id.
Comandos como `default-information originate` e `passive-interface <interface>` precisam ser feitos durante essa execução.

```
Router(config)#int <interface>
Router(config-if)#ipv6 address <ipv6>
Router(config-if)#ipv6 ospf <id> area <area-id>
Router(config-if)#no shutdown
```

Nesta versão habilitamos o IPv6 por interface.

**Troubleshooting**

```
Router(config)#show ip protocols
```

Mostra os protocolos sendo utilizados

```
Router(config)#show ip ospf <id>
```

Mostra configurações do ospf especificado

```
Router(config)#show ip route
```

Mostra configurações de rota

```
Router(config)#show ip osfp interface [brief]
```

Mostra as interfaces utilizando o OSPF

```
Router(config)#show ip osfp neighbor
```

Mostra os vizinhos que também estão utilizando este protocolo

```
Router(config)#show ip ospf database
```

Mostra as informações obtidas pelo ospf

```
Router(config)#debug ip ospf
```

Testa a conectividade ospf

### BGP

O BGP (Border Gateway Protocol), é o protocolo utilizado para interligar diferentes Sistemas Autônomos (AS) na internet. Dentro desse protocolo temos uma divisão entre eBGP (exterior Gateway Protocol) e o iBGP (interior Gateway Protocol).

O eBGP é utilizado para interligar roteadores de ASs distintos. Existe um atributo utilizado por está parte do protocolo chamado de AS_PATH, ele é uma lista que contém todos os ASs pelos quais uma rota passou da origem até o destino. É por meio dele que essa parte externa do protocolo evita loops.

O iBGP é utilizado para realizar a ligação de roteadores em um mesmo AS. A maneira que o iBGP tem de evitar loops é utilizando da ideia de horizonte dividido (split-horizon), assim o RIP, ou seja o roteador não repassa rotas para um outro roteador que ele tenha apreendido a rota.
Na prática isso gera um problema, pois algumas rotas acabam não sendo anunciadas, para resolver isso temos a seguinte opção:

- **Topologia Full Mesh**: Neste método todos os rotedaores devem ser interconectados. Então em uma rede com 10 roteadores teriamos 45 conexões. O valor é obtido com a seguinte fórmula n x ((n - 1) / 2).

- **Route Reflector**: Neste método um dos roteadores é responsável por refletir as rotas apreendidas para outros roteadores. Este roteador tem o nome de route reflector, pois o mesmo reflete a rota para todos os outros. O grande problema desse método e que se este roteador cair a rede inteira pode ficar indisponível.

- **BGP Confederation**: Nesta técnica os ASs são subdivididos para depois serem reagrupados em uma única confederação.

#### eBGP

```
Router(config)#router bgp <AS>
```

Habilita o BPG no roteador e associa o mesmo a AS(número) especificada.

```
Router(config-router)#neighbor <ip> remote-as <AS>
```

Especifíca o vizinho do roteador atual dentro do BGP. O ip especificado deve ser o mesmo do vizinho, e o AS é o número especificado no bgp do roteador vizinho.

```
Router(config-router)#network <ip> mask <ip-mask>
```

Especifíca os endereços associados a este roteador. Caso o roteador abranja mais de uma rede uma sumarização pode ser feita.

#### iBGP

**OBS:** Em casos onde temos muitos roteadores e nem todos eles estão interligados, se faz necessário o uso de pelo menos mais um protocolo de roteamento, geralmente OSPF, para que os roteadores dentro do iBGP consigam conhecer uns aos outros.

```
Router(config)#int lo0
Router(config-if)#ip address <ip> <mask>
```

Aqui estamos configurando a interface de loopback. Essa configuração não é exatamente obrigatória, a necessidade dela vai variar conforme a topologia que você utilizou para montar a rede.
Essa configuração é usada pois no caso de uma interface cair, os outros roteadores na rede ainda vão saber chegar neste roteador.

```
Router(config)#router bgp <as>
Router(config-router)#neighbor <ip> remote-as <as> 
Router(config-router)#neighbor <ip> next-hop-self
```

Além de estarmos especificando o AS vizinho, o comando `next-hop-self` é usado para obrigar o roteador iBGP a repassar ele mesmo como sendo o roteador de próximo salto. Se este comando não for utilizado, o roteador iBGP repassa, como sendo o próximo salto o roteador pelo qual eele aprendeu a rota.

```
Router(config-router)#neighbor <ip> update-source <int-loop>
```

Caso você tenha configurado a interface de loopback, para fazer parte da rede, como no comando acima, você vai utilizar este comando como uma forma de dizer que a interface de loopback também pode ser utilizada para criar conexões BGP.

```
Router(config-router)#aggregate-address 192.168.0.0 255.255.0.0 summary-only
```

O comando acima é opcional e serve basicamente para realizar um resumo das redes que este roteador anuncia. No exemplo mencionado, estamos resumindo a rede 192.168.0.0. Toda e qualquer solicitação de um IP que esteja dentro deste intervalo será encaminhada para o roteador em questão. Este comando é útil para diminuir o tamanho das tabelas de roteamento.

**Router Reflector**

``` 
Router(config)#router bgp <as> 
Router(config-router)#neighbor <ip> remote-as <as> 
Router(config-router)#neighbor <ip> update-source <loop-int> 
Router(config-router)#neighbor <ip> route-reflector-client 
Router(config-router)#neighbor <ip> next-hop-self
```

A configuração acima exemplifica como o Route Reflector deve ser considerado. Em geral, sua configuração é similar à dos outros roteadores; o único comando diferente é o `neighbor <ip> route-reflector-client`, que especifica o roteador cliente que receberá as rotas do Route Reflector.
Importante: Este roteador precisa anunciar todas as redes que estejam dentro do seu AS utilizando o comando `network`.

#### IPv6

```
Router(config)#router bgp <as> 
Router(config-router)#bgp router-id <id> 
Router(config-router)#no bgp default ipv4-unicast 
Router(config-router)#neighbor <ipv6> remote-as <as>
```

Acima temos o inicio da configuração de BGP com IPv6, neste primeiro momento o único comando que temos diferente da versão IPv4 é o `no bgp default ipv4-unicast`. Este comando serve para dizer ao roteador que o IPv6 será usado ao invés do IPv4.

```
Router(config-router)#address-family ipv6 
Router(config-router-af)#neighbor <ipv6> activate 
Router(config-router-af)#network <ipv6/mask>
```

Acima temos a configuração dos vizinhos (neighbor) e das redes (network) do BPG.
O primeiro comando (`address-family ipv6`) serve para especificar que iremos configurar as redes IPv6.
Começando pelo vizinho usamos o mesmo ip que haviamos especificado no comando `neighbor <ipv6> remote-as <as>`, aqui estamos ativando sua conexão.
Já nas redes o comando é similar ao que tinhamos anteriormente, com a única diferença que a máscara agora é declarada na notação CIDR (ex. fd00::1/64).


**Troubleshooting**

OBS: No caso de estar utilizando o ipv6, substitua os comandos `ip` por `ipv6`

```
Router#show ip bgp
```

Mostra a lista de redes aprendidas pelo bgp e qual o seu ip para o seu salto.

```
Router#show ip bgp <ip>
```

Mostra informações sobre as rotas até uma determinada rede ou host. A rota marcada como `best` é a melhor rota para o destino marcado.

```
Router#show ip bgp summary
```

Mostra a relação deste roteador com seus vizinhos. O estado de Active, significa que o roteador está esperando o estabelecimento da conexão. A conexão só estará ativa e funcional quando aparecer um número positivo no lugar de *Active*.

## NAT

### Estático

```
Router(config)#interface f0/0
Router(config-if)#ip address <ip-externo> <máscara>
Router(config-if)#ip nat outside
Router(config-if)#Exit
Router(config)#interface f0/1
Router(config-if)#ip address <ip-interno> <máscara>
Router(config-if)#ip nat inside
```

Perceba que existem dois comandos a mais ao configurarmos a interface
o ip nat inside/outside. O inside se refere a interface que ficara
na parte de dentro da rede será a área local, enquanto o outside se
rede a interface que ficara na parte de fora da rede será a área
externa.
Quando a tradução for realizada o endereço de dentro irá ser traduzido
para fora(ao enviar uma mensagem), e o contrário também irá ocorrer
(ao receber a mensagem).

```
Router(config)#access-list 1 permit <ip-rede> <máscara-curinga>
```

Esse comando serve para criar uma regra dentro do firewall. Nesse caso
o comando está permitindo, o ip da rede interna com a seguinte máscara
curinga podera ter pacotes circulando

```
Router(config)#ip nat inside source static <ip-interno> <ip-externo>
```

Aqui estamos explicitamente dizendo ao roteador que o nat usado
será estático, onde esse determinado ip interno irá ser traduzido
para aquele ip externo e vice-versa
Observação é possível também especificar a porta na qual será usada
para essa tradução para isso basta dizer a porta do ip interno e a
porta do ip externo

### Dinâmico

```
Router(config)#ip nat pool <nome> <ip-inicial> <ip-final>
netmaks <máscara>
```

Esse comando define uma pool, onde nela iremos encontrar todos os
endereços que poderão ser usados pelo nat. Similar ao que é feito
com o DHCP

```
Router(config)#access-list 1 permit <ip-rede> <máscara-curinga>
```

Esse comando serve para criar uma regra dentro do firewall. Nesse caso
o comando está permitindo, o ip da rede interna com a seguinte máscara
curinga podera ter pacotes circulando

```
Router(config)#ip nat inside source list <número-ACL>
pool <nome-da-pool>
```

Está configuração é usada para o roteador identificar quais
dispositivos recebem quais endereços. Onde iremos ligar a pool a ACL

```
Router(config)# interface f0/0
Router(config-if)#ip nat inside
```

Configure as interfaces que serão consideradas parte da rede interna

```
Router(config)#interface f0/1
Router(config-if)#ip nat outside
```

Configure as interfaces que serão consideradas parte da rede externa

### PAT

```
Router(config)#interface f0/0
Router(config-if)#ip address <ip-externo> <máscara>
Router(config-if)#ip nat outside
Router(config-if)#Exit
Router(config)#interface f0/1
Router(config-if)#ip address <ip-interno> <máscara>
Router(config-if)#ip nat inside
```

```
Router(config)#access-list 1 permit <ip-rede> <máscara-curinga>
```

Esse comando serve para criar uma regra dentro do firewall. Nesse caso
o comando está permitindo, o ip da rede interna com a seguinte máscara
curinga podera ter pacotes circulando

```
Router(config)#ip nat inside source list 1 interface f0/0 overload
```

Esse é o comando que define o PAT em si, é ele que irá permitir a
tradução para um IP com portas diferentes.
A interface colocada neste comando será a que vai ser a tradução.

**Troubleshooting**

```
Router(config)#show ip nat translations
```

Mostra as tabela de tradução NAT

```
Router(config)#clar ip nat translation
```

Apaga as entradas da tabela

```
Router(config)#show ip nat statistics
```

Mostra informações a cerca das traduções NAT

```
Router(config)#debug ip nat
```

Testa as traduções NAT

## SNMP

### SNMPv2c

```
Router(config)#snmp-server comunity <communitystring> RO [ACL|IPv6ACL]
```

Habilita o agente SNMP, colocando as strings como Read Only(RO), e
mais as restrições das ACLs(opcional)

```
Router(config)#snmp-server comunity <communitystring> RW [ACL|IPv6ACL]
```

Habilita o agente SNMP, colocando as strings como Read Write(RW), e
mais as restrições das ACLs(opcional).
É o mesmo da configuração anterior com a diferença nas strings

```
Router(config)#snmp-server location <descrição>
```

É um comando opcional que documenta a localização do dispositivo

```
Router(config)#snmp-server contact <contato>
```

É um comando opcional que documenta uma nome de contato caso algum
erro ocorra

```
Router(config)#snmp-server host {hostname|ip-address} [informs]
version 2c <notificação>
```

Esse comando é usada para cada um dos dizer ao servidor qual host ele
deve enviar mensagens. O padrão é enviar mensagens Trap, mas é
possível escolher a Informs

```
Router(config)#snmp-server enable traps
```

Esse comando permite que o servidor envie mensagens para todos os
hosts da rede

**Troubleshooting**

```
Router(config)#show snmp comunity
```

Mostra as configurações do snmp

```
Router(config)#show snmp location
```

Mostra a localidade configurada no snmp

```
Router(config)#show snmp contact
```

Mostra o contato disponibilizado no snmp

```
Router(config)#show snmp host
```

Mostra os hosts configurados no snmp

```
Router(config)#show snmp
```

Mostra as configurações do snmp

### SNMPv3

```
Router(config)#snmp-server group <grupo> v3 {noauth|auth|priv}
[write v1default] [ACL|ACL-IPv6]
```

Habilita o agente SNMP, criando um grupo SNMPv3 com algumas
configurações de segurança padrão. Opcionalmente o tipo de escrita
e também opcionalmente colocando uma ACL

```
Router(config)#snmp-server user <usuário> <grupo> v3
```

Serve para colocar o usuário dentro de um grupo que possui um nível
de segurança equivalente a noauth

```
Router(config)#snmp-server user <usuário> <grupo> v3 auth [md5|sha]
<senha>
```

Serve para colocar o usuário dentro de um grupo que possui um nível
de segurança equivalente ao auth. Ou seja tem um usuário e senha

```
Router(config)#snmp-server user <usuário> <grupo> v3 [md5|sha] <senha>
priv [des|3des|aes(128|192|256)] <chave-criptografica>
```

Serve para colocar um usuário dentro de um grupo que possui um nível
de segurança equivalente ao priv. Ou seja que tem um usuário e senha,
e também uma criptografia no envio de dados

```
Router(config)#snmp-server host {hostname|ip} [informs|traps] version
3 {noauth|auth|priv} <usuário>
```

Use esse comando para especifica cada um dos usuários que irá receber
mensagens do servidor

```
Router(config)#snmp-server enable traps
```

Permite que o servidor envie mensagens para todos os hosts da rede

**Troubleshooting**

```
Router(config)#show snmp user
```

Mostra os usuários do snmp

```
Router(config)#show snmp group
```

Mostra os grupos do snmp

As outras configurações do SNMPv2c também valem aqui

### CDP

```
Router(config)#cdp run
```

Habilita o CDP nos dispositivos de rede

```
Router#show cdp
```

Mostra informações sobre o CDP

```
Router#show cdp interface <int>
```

Mostra informações do CDP na interface especificada

```
Router#show cdp neighbors
```

Mostra informações sobre os vizinhos no qual o CDP também está
habilitado

```
Router#show cdp neighbors detail
```

Mostra informações sobre os vizinhos em mais detalhes

### LDP

```
Router(config)#lldp run
```

Habilita o LLDP nos dispositivos de rede

```
Router#show lldp
```

Mostra informações sobre o LLDP

```
Router#show lldp interface <int>
```

Mostra informações do LLDP na interface especificada

```
Router#show lldp neighbors
```

Mostra informações sobre os vizinhos no qual o LLDP também está
habilitado

```
Router#show lldp neighbors detail
```

Mostra informações sobre os vizinhos em mais detalhes

### NTP

```
Router(config)#clock timezone <nome> <hora_antes_UTC> <min_antes_UTC>
```

Especifica o fuso horário com base no UTC(Universal Time Clock)

```
Router(config)#summer-time <nome> {date|recurring}
```

Especifica o período em que é horário de verão

```
Router(config)#clock set <hh:mm:ss> <dia> <mês> <ano>
```

Coloca o relógio na data e hora que for especificada

```
Router(config)#ntp master <stratum>
```

Transforma o dispositivo em um servidor NTP. O stratum é um valor que
é especificado para dizer o quão confiável é o relógio desse
dispositivo

```
Router(config)#ntp source <interface>
```

Especifica qual a interface que irá prover os dados do relógio NTP
para outros dispositivos

```
Router(config)#ntp server <ip>
```

Especifica o servidor NTP que irá fornecer a data e a hora para o
dispositivo

**Troubleshooting**

```
Router#sh clock
```

Mostra a data e hora do dispositivo

```
Router#sh ntp associations
```

Mostra os dispositivos associados a esse

```
Router#sh ntp status
```

Mostra informações sobre o NTP

## HSRP

```
Router(config)#standby <group> ip <virtual-ip>
```

Esse comando é o único comando necessário para implementar o HSRP. O
grupo é o IP deve ser igual nos dois(ou mais) roteadores onde esse
comando for implantado

```
Router(config)#standby <group> priority <prioridade>
```

Este comando especifica a prioridade do roteador para ser o root.
Quanto maior o valor maior a sua prioridade

```
Router(config)standby <group> preempt
```

Este comando faz com que o roteador seja sempre o root, independete
dele ter sido ligado depois de outro roteador

**Troubleshooting**

```
Router(config)#show standby
```

Mostra configurações do HSRP

```
Router(config)#show standby brief
```

Mostra configurações do HSRP de maneira resumida

## GLBP

```
Router(config)#glbp <grupo> ip <ip-virtual>
```

O comando é único é precisa ser feito nos dois(ou mais) roteadores.
Assim como no HSRP os grupos devem ser iguais. Porém os IP's não
precisam

```
Router(config)#glbp <group> priority <prioridade>
```

Este comando especifica a prioridade do roteador para ser o root.
Quanto maior o valor maior a sua prioridade

```
Router(config)#glbp <group> preempt
```

Este comando faz com que o roteador seja sempre o root, independete
dele ter sido ligado depois de outro roteador

```
Router(config)#glbp <group> forwarder preempt
```

Este comando faz o mesmo que o de cima porém ele define o roteador
como aquele que irá encaminhar

```
Router(config)#glbp <group> load-balancing <metodo>
```

Escolhe o método de balanceamento que será utilizado.

**Troubleshooting**

```
Router(config)#show glbp
```

Mostra configurações do GLBP

```
Router(config)#show glbp brief
```

Mostra configurações do GLPB de modo resumido

## SSH

```
Router(config)#ip domain-name <nome>
Router(config)#crypto key generate rsa
```

O primeiro passo na configuração do SSH é a de configurar o nome de
domínio pois somente desse modo que a chave rsa irá ser autorizada

```
Router(config)#ip ssh version 2
```

Muda a versão do ssh utilizada. Para poder usar essa versão é
necessário que a senha rsa seja mais forte que 768

```
Router(config)#username <usuário> password <senha>
Router(config)#line vty 0 4
Router(config-line)#login local
Router(config-line)#transport input ssh
Router(config-line)#exit
Router(config)#ip ssh authentication-retries 3
```

O primeiro comando está colocando o usuário e a senha necessária para
acessar o SSH.
O comando login local está implementando a ideia de login local na
linha de console de acesso remoto.
Transport input ssh, é o comando que implementa a utilização do ssh
como uma forma de configurar o transporte.
O último comando authenticaiton-retries 3, dá ao usuário a
possibilidade de tentar até 3 o acesso ao roteador usando o SSH

```
C:\>ssh -l <usuário> <ip>
```

Esse é o comando utilizado para acessar o roteador de um computador
de maneira remota

## AAA

```
Router(config)#aaa new-model
```

Habilita o AAA(Authentication Authorization Accounting), um servidor
de autenticanção.

```
Router(config)#username <nome> password <senha>
```

Mesmo tendo habilitado um servidor de autenticação é muito importante
habilitar um nome e senha como backup para caso o servidor falhe

## TACACS+

```
Router(config)#aaa authentication login default group tacacs+ local
```

Habilita a autenticação por meio do servidor AAA. O login especifica
que a autenticação será pedida ao entrar no dispositivo, equivale ao
login local usado no console. Já o groups tacacs+ especifica que o
servidor utilizado será o TACACS+

```
Router(config)#aaa authentication enable default group tacacs+ local
```

Habilita a autenticação por meio do servidor AAA. O enable especifica
que a autenticação será pedida ao tentar entrar no modo 'root',
equivale ao comando enable password. Assim como no comando anterior
o group tacacs+ especifica que o servidor utilizado será o TACACS+

O TACACS+ é um servidor que utiliza o protocolo AAA. Ele utiliza para
o envio de pacotes o TCP, utiliza uma criptografia em todo o pacote,
e em sua autenticação e autorização são enviadas de maneira separada.

```
Switch(config)#tacacs host <server_ip> key <chave_autenticação>
```

Este comando serve para autenticar o dispositivo no servidor AAA
sem este comando a autenticação não irá funcionar.

## RADIUS

```
Router(config)#aaa authentication login default group radius local
Router(config)#aaa authentication enable default group radius local
```

Equivale aos mesmos comandos mostrados anteriormente com a única
diferença que o servidor utilizado é o Radius

O RADIUS é um servidor que utiliza o protocolo AAA. Ele utiliza o UDP,
criptografa somente as senhas e usuários, e por último combina em um
só pacote a autenticação e autorização

```
Switch(config)#radius host <server_ip> key <chave_autenticação>
```

Este comando serve para autenticar o dispositivo no servidor AAA,
sem este comando a autenticação não irá funcionar.

## QoS

Comandos para implementar qualidade de serviço no envio de seus
pacotes pela rede.

```
Router(config)#class-map <nome>
```

Cria uma classe que será usada para filtrar pacotes. Por padrão essa
classe irá filtrar todos os pacotes, mas é possível alterar isso
especificando antes do nome
Para cada coisa que você deseja filtrar é necessário criar um
class-map

```
Router(config-cmap)#match [opções]
```

Existem diversas opções que podem ser usadas para filtrar um pacote
desde uma ACL até um IP. Por padrão o mais comum é utilizar protocolos
Caso queira aqui é possível colocar uma marcação no pacote, assim que
ele passar pelo roteador ele já terá uma prioridade.

```
Router(config)#policy-map <nome>
```

Cria a politica que vai aplicar os filtros feitos anteriormente.

```
Router(config-pmap)#class <class-map_nome>
```

Aqui você irá especificar o nome dos filtros que você criou
anteriormente.

```
Router(config-pmap-c)#priority <número>
```

Define a prioridade que os pacotes deste filtro possuem ao serem
enviados

```
Router(config-pmap-c)#bandwidth <número>
```

Define a quantidade de banda-larga que os pacotes desta classe irão
possuir na filtragem

```
Router(config-pmap-c)#set ip dscp <valor>
```

Define a prioridade de queda dos pacotes. Segue uma lista de valores
de prioridades que podem ser encontradas:

IPP5 CS5 EF -> Voz

IPP4 CS4 AF41,AF42,AF43 -> Video

IPP3 CS3 AF31,AF32,AF33 -> Critico

IPP2 CS2 AF21,AF22,AF23 -> Transações

IPP1 CS1 AF11,AF12,AF13 -> Dados em Massa

IPP0 CS0 BE -> Scavenger

Esses valores colocados acima dizem respeito a prioridade no sentido
de descarte do pacote.

```
Router(config-pmap-c)#set precedence <valor>
```

Aqui é especificado no roteador se os dados que ele recebem não são
confiáveis e devem ser remarcados com algum outro valor. Os valores
vão de 0 a 7, com 0 tendo menos prioridade e 7 sendo o com maior
prioridade.

```
Router(config)#interface <int>
Router(config-if)#service-policy {in|out} <nome_service-policy>
```

Assim como uma ACL a politica de qualidade de serviços deve ser
especificado em uma interface como sendo entrada ou saída.

## ACL

### Padrão

As ACLs padrões vão da numeração 1 a 99. E da expanção 1300 a 1999

```
Router(config)#access-list <id> {permit|deny} host <ip>
Router(config)#interface f0/0
Router(config-if)# ip access-group <id> {in|out}
```

Na configuração da access-list padrão, o que ocorre é o bloqueio ou a
permissão de um host em especifico. Ou seja todos os pacotes do host
especificado passam por uma filtragem.
O segundo comando tem que ser colocado especificamente em uma
interface, é especificado então o ID da ACL que foi criada
anteriormente, e perceba também que é necessário especificar se a
filtragem de pacotes irá acontecer para os pacotes de dentro(in) ou
fora(out) da interface.

```
Router(config)#ip access-list standard <nome>
Router(config-std-nacl)#{permit|deny} <ip> <máscara>
Router(config-std-nacl)#interface f0/0
Router(config-if)#ip access-group <nome> {in|out}
```

Essa configuração é a mesma que foi feita anteriormente com a
diferença que a ACL nesse caso é nomeada ao invés de enumerada.

Exemplo de uma ACL standard:

```
Router(config)#access-list 1 permit 192.168.12.0 0.0.0.255
```

Essa ACL está permitindo todo o tráfico da rede 192.168.12.0

Ao final de toda ACL existe uma regra oculta que nega tudo. Ela seria
escrita da seguinte forma

```
Router(config)access-list 1 deny any
```

O any é uma palavra chave usada que nesse caso indica qualquer um

### Estendida

A ACL estendinda tem aceita a numeração de 100 a 199. E de maneira
expandida de 2000 a 2699

A grande diferença das ACLs estendidas está no fato de que além de
ser possível filtrar endereços host, também é possível realizar
filtros a partir de portas

```
Router(config)#access-list <id> {permit|deny} {tcp|udp|ip} host <ip>
<ip> eq <porta>
Router(config)#interface f0/0
Router(config)#ip access-group <id> {in|out}
```

O primeiro comando define o id, se irá permitir ou negar. A opção
TCP e UDP serve porque é necessário especificar as portas que serão
utilizadas que costumam utilizar esses serviços, vale dizer que
existe o ICMP(icmp), que é o comando utilizado em ACLs quando queremos
filtrar os envios de pings principalmente

```
Router(config)#ip access-list extended <nome>
Router(config-ext-nacl)#{permit|deny} {tcp|udp|ip} <ip> <máscara> <ip>
<máscara> eq <porta>
Router(config-ext-nacl)interface f0/0
Router(config-if)#ip access-group <nome> {in|out}
```

Esse é o mesmo que o comando anterior porém utilizando nome ao invés
da numeração

```
Router(config)#ip access-list extended {nome|id}
Router(config-ext-nacl)#no <list-id>
Router(config-ext-nacl)#<list-id> {permit|deny} {tcp|udp|ip} <ip>
<máscara> <ip> <máscara> eq <porta>
```

Caso queira editar uma ACL basta acessá-la seja por meio do seu ID ou
de seu nome, depois é necessário saber o número da linha que você quer
editar(list-id), apague a linha e implemente ela novamente

**Exemplo** de uma configuração de ACL extendida:

```
Router(config)#access-list 110 permit tcp 92.128.2.0 0.0.0.255 any eq
80
```

Nessa configuração temos uma ACL que permite qualquer tráfico que
venha da rede 92.128.2.0 para qualquer destino na porta 80

```
Router(config)#access-list 111 permit ip 192.168.1.0 0.0.0.255
192.168.2.0 0.0.0.255
```

Nessa configuração temos uma ACL que permite qualquer tráfico que
venha da rede 192.168.1.0 para a rede 192.168.2.0

Ao final de toda ACL existe uma regra oculta que nega tudo. Ela seria
escrita da seguinte forma

**OBS:**

```
Router(config)#line vty 0 4
Router(config-line)#ip access-class <id> {in|out}
```

Para colocarmos uma ACL em uma VTY utilizamos o access-class ao invés
do access-group

**Troubleshooting**

```
Router(config)#show ip interfaces
```

Mostra as configurações das interfaces

```
Router(config)#show access-lists
```

Mostra as ACLs que existem

```
Router(config)#show ip access-lists
```

Mostra as configurações do ACL

# IOS

Em alguns momentos na utilização dos dispositivos de redes será
necessário atualizar sua imagem(IOS), ou até mesmo salvar as suas
configurações em um servidor a parte. Para realizarmos essa troca de
dados nós utilizamos os seguintes protocolos de redes:

## FTP

```
Router#copy ftp://<usuario>:<senha>@<ip-servidor>/<nome-arquivo>
<armazenamento>
```

Nesse comando do ftp estamos acessando o servido com o usuário e senha
especificada, para copiar o arquivo para um armazenamento(o mais comun
é o flash0).
Caso você primeiro especifique o armazenamento e depois o ftp://...
você estaria copiando o arquivo para o servidor

```
Router(config)#ip ftp username <usuario>
Router(config)#ip ftp password <senha>
Router(config)#end
Router#copy ftp: running-config
```

Nessa configuração nós já especificamos o usuário e a senha do FTP ao
roteador por esse motivo não tivemos que especifica-la novamente ao
copiar o arquivo.
O último comando não traz especificações como IP do servidor, e nome
do arquivo, porém o próprio roteador irá te perguntar isso caso você
não especifique.

## TFTP

```
Router#copy <arquivo> tftp
```

Copia as configurações para um servidor tftp. Como não há
especificação o próprio roteador irá te fazer perguntas

```
Router#copy tftp <arquivo>
```

Copia as configuraçõs do servidor tftp para o roteador. Como não há
especificações o próprio roteador irá te fazer pergunta

OBS: Caso queira utilizar isso para copiar a configuração que está
rodando no roteador basta usar substituir o \<arquivo\> por
runnin-config(no caso da configuração sendo rodado) ou startup-config
(no caso da configuração já salva no roteador)

## SCP

SCP é um protocolo que se utiliza do SSH como base para realizar a
entrega dos seus arquivos, por isso antes de utiliza-lo é necessário
ativar o SSH.

```
Router#ip scp server enable
```

Configura o roteador para ele habilitar o uso do modo servidor SCP

```
Router#scp <username>@<ip>:<armazenamento>/<nom-do-arquivo> .
```

Comando usado para baixar o arquivo do roteador. Vale lembrar o
armazenamento mais utilizado pelo roteador é o flash0:

```
Router#scp <nome-do-arquivo> <usuario>@<ip>:<armazenamento>/<nome-do
-arquivo>
```

Carrega o arquivo do servidor para o roteador

## Archive

No caso de alguns dispositivos de rede ou na atualização de alguns
IOS, os processos citados acima podem ser feitos sem grande problemas
porém em redes onde temos vários dispositivos, esse processo se
torna um pouco mais complicado. Para isso utilizamos o modo archive
do roteador, que efetua backups automáticos.

```
Router(config)#archive
Router(config-archive)#path tftp://<ip>
```

O primeiro comando acessa o modo de configuração archive. O segundo
comando é um exemplo de sua utilização no caso está sendo especificado
um servidor tftp, mas poderia ser ftp ou scp.

```
Router#archive config
```

Esse comando escreve o archive

```
Router(config)#archive
Router(config-archive)#path tftp://<ip>/$h-$t
```

Alguns engenheiros de rede utilizam essa configuração para salvar o
nome do arquivo com o nome do roteador(variável $h) e o horário($t).
Mas as duas variaveis são opcionais, e não precisam ser colocadas
juntas

```
Router(config)#archive
Router(config-archive)#write-memory
```

Esse comando serve para definir que o backup será feito
automáticamente. Esse comando deve ser configurado após o caminho.

```
Router(config)#archive
Router(config-archive)#time-period <tempo>
```

Define o tempo até o próximo backup(em minutos). Essa configuração é
feita após o write-memory.

```
Router#show archive
```

Mostra os backups que foram realizados

No caso de roteador e switch, toda vez que quisermos realizar uma
restauração de arquivos temos que reiniciar o dispositivo. Porém
existe uma alternativa a isso o comando replace, que substitui a
configuração existente pela do backup.

**Exemplo:**

```
Router#show archive

1 ftp://wendell:odom@192.168.1.170/-Oct-24-09-21-38.865-0
2 ftp://wendell:odom@192.168.1.170/-Oct-24-09-22-22.561-1
3 ftp://wendell:odom@192.168.1.170/-Oct-24-09-46-43.165-2

ridiculousname# configure replace ftp://wendell:odom@192.168.1.170/
-Oct-24-09-46-43.165-2

This will apply all necessary additions and deletions
to replace the current running configuration with the
contents of the specified configuration file, which is
assumed to be a complete configuration, not a partial
configuration. Enter Y if you are sure you want to proceed. ? [no]: y

Loading -Oct-24-09-46-43.165-2 !
[OK - 6498/4096 bytes]
Loading -Oct-24-09-46-43.165-2 !
Total number of passes: 1
Rollback Done
```

## ROMMON

O modo ROMMON é o equivalente a um segundo SO do dispositivo, ele
costuma ser acessado quando o IOS não está presente no dispositivo
ou não pode ser acessado por algum motivo. Ele costuma ser utilizado
para a recuperação de senhas.

Caso tenha perdido sua senha e você queira restaurá-la siga os
seguintes passos:

1.Faça o boot no ROMMON, isso pode ser feito quebrando o processo de
boot do console ou removendo a memória flash

2.Coloque as conigurações de registro para ignorar o arquivo
startup-config. Isso é feito com o seguinte comando

> confreg 0x2142

OBS: O padrãod e registro que o roteador costuma atuar é o 0x2102

3.Realize o Boot do roteador com o IOS. O roteador irá fazer o boot
sem configuração alguam. Agora é possível fazer as configurações no
roteador sem precisar das senhas.
E aqui que os backups utilizados serão usados, e as mudanças de
senhas serão feitas.

OBS: Lembre-se de colocar o roteador para autar no padrão de registro
que ele já utiliza(geralmente o 0x2102)

## VLAN

Realizamos a criação de uma interface virtual. No cisco isso é feito por meio do comando `interface <interface>.x`, onde `x` é a subdivisão da interface.

```
Router(config)#interface <interface.x>
Router(config-subif)#encapsulation dot1Q <id>
Router(config-subif)#ip address <ip> <máscara>
```

O comando dot1Q especifíca o protocolode encapsulamento dos pacotes que será usado nesta interface. Após usar este comando, bastar subir a interface padrão(sem ser a virtual) com `no shutdow`, a interface pode estar de pé antes de realizar está configuração.

Exemplo da configuração de VLANs em um roteador

```
Router>enable
Router#configure terminal
Router(config)#interface fastEthernet 0/0
Router(config-if)#ip address 192.168.10.1 255.255.255.248
Router(config-if)#no shutdown
Router(config)#interface fastEthernet 0/0.2
Router(config-subif)#encapsulation dot1Q 2
Router(config-subif)#ip address 192.168.10.9 255.255.255.248
```

Perceba que as interfaces virtuais não necessita do comando no shutdown.

**Troubleshooting**

```
Router#show ip interfaces
Router#show vlans1/0(200.0.0.1)
```

## GRE

```
Router(config)#tunnel <id>
Router(config-if)#ip address <ip> <máscara>
Router(config-if)#tunnel source {ip|interface}
Router(config-if)#tunnel destination {ip}
```

Estas são as configurações que precisam ser feitas para ativar um
túnel GRE entre dois roteadores. Primeiro é ativado a interface
túnel, depois é colocado um endereço IP(as duas interfaces devem estar
na mesma subrede), logo depois nós especificamos qual será a interface
pela qual o túnel irá sair(tunnel source) e para qual interface ele
irá chegar(tunnel destination). Para que essas configurações possam
funcionar é necessário que os dois roteadores estejam se comunicando.

OBS: Em alguns casos para que uma rede ligada ao roteador consiga
conversar com a rede na outra ponta é necessário ativar um protocolo
de roteamento ou uma rota estática.

# Switches

# Comandos Básicos

A grande maioria dos seus comandos comuns são identicos aos comandos
utilizados no roteador.

```
Switch(config)#ip name-server <ip>
```

Configura o servidor DNS que o Switch irá utilizar

```
Switch(config)#no ip domain-lookup
```

Desabilita o DNS lookup

```
Switch(config)#ip default-gateway <ip>
```

Estabelece um endereço de roteador padrão que o switch irá utilizar

```
Switch(config)#interface <interface>
Switch(config-if)# speed <velocidade-Mbps>
```

Define a velocidade da interface

```
Switch(config)#interface <interface>
Switch(config-if)#speed auto
```

Define a velocidade da interface de maneira automática

```
Switch(config)#interface <interface>
Switch(config-if)#duplex full
```

Define a interface como full duplex. Nesse modo a comunicação segue
as duas direções simutaneamente, ou seja o canal de comunicações e
divido em duas partes uma para enviar e outra para receber dados e
isso é feito de maneira simultanea

```
Switch(config)#interface <interface>
Switch(config-if)#duplex half
```

Define a interface como half duplex. Nesse modo a configuração somente
uma direção de cada vez, ou seja o canal de comunicações ou está
enviando ou está recebendo dados

```
Switch(config)#interface <interface>
Switch(config-if)#duplex auto
```

Define automáticamente o tipo de duplex que será utilizado

**OBS**

Caso esteja utilizando um switch de camada 3, é possível desligar o modo switch de uma de suas partes. Isso permite que o que o mesmo seja usado como um roteador.

```
Switch(config)#interface <interface>
Switch(config-if)#no switchport
```

## VLAN

```
Switch(config)#vlan <id>
Switch(config-vlan)#name <nome>
```

Cria a VLAN e dá um nome a ela

```
Switch(config)#interface <interface>
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan <id>
```

Nessa primeira configuração de porta, devemos configurar o modo no
qual a porta estara atuando. Nesse caso a interface irá atuar no modo
access, nesse modo somente uma VLAN é utilizado e costuma ser
implementado em dispositivos finais, que também estarão na mesma VLAN

```
Switch(config)#interface <interface>
Switch(config-if)#switchport mode trunk
Switch(config-if)#switchport trunk encapsulation dot1q
Switch(config-if)#switchport trunk allowed vlan <number[, number...]>
```

Nessa configuração estabelecemos a porta sendo utilizada como modo
trunk, nesse modo de configuração a porta pode carregar o tráfico de
várias VLANs(por padrão todas as configuradas no switch), e geralmente
é utilizado para fazer a conexão entre Switches e de Switch para
roteador.
O próximo comando estabelece o tipo de encapsulação que será usada,
nesse caso utilizamos a mesma que usamos no roteador Dot1Q, é isso
que irá permitir que o roteador faça o roteamento de VLANs.
Por último especificamos quais são as VLANs que poderam ser passadas por está trunk

Exemplo de configuração:

```
Switch(config)#vlan 2
Switch(config-vlan)#name ADM
Switch(config-vlan)#exit
Switch(config)#interface range f0/1-0/10
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 2
Switch(config-if)#exit
```

Nesse caso o primeiro comando serve para você pegar todo um raio de
interfaces, aqui ele está pegando da interface f0/1 até a interface
f0/10

```
Switch(config)#interface f0/24
Switch(config-if)#switchport mode trunk
Switch(config-if)#switchport trunk encapsulation dot1q
```

## DTP(Dynamic Trunking Protocol)

Protocolo proprietário da Cisco usado para que duas portas negociem
o modo trunk entre Switches. Também pode ser usado para negociar
o tipo de encapsulação

Dynamic auto, quando uma interface está configurada com este modo ela
não irá tentar converter a outra interface do switch para ser trunk.
Ou seja caso configurada desse modo, ela só irá se tornar trunk se o
outro switch estiver configurado no modo trunk ou dynamic desirable.

Dynamic desirable, quando uma interface está configurado com este modo
ela irá tentar ativamente configurar o outro switch a coloca sua
interface no modo trunk. Ele irá formar um link trunk em qualquer
interface que esteja como dynamic desirable, dynamic auto ou trunk.

```
Switch(config)#interface <interface>
Switch(config-if)#switchport mode dynamic desirable
```

Configura o dynamic desirable

```
Switch(config)#interface <interface>
Switch(config-if)#switchport mode dynamic auto
```

Configura o dynamic auto

```
Switch(config)#interface <interface>
Switch(config-if)#swichport mode trunk
Switch(config-if)#switchport nonegotiate
```

Esse comando serve para estabelecer que a porta do switch não irá
sofrer auterações de negociações com as dinâmicas

**Troubleshooting**

```
Switch(config)#show vlan
```

Mostra as VLANs

```
Switch(config)#show vlan brief
```

Mostra um resumo das VLANs

```
Switch(config)#show interface trunk
```

Mostra informações do trunk

```
Switch(config)#show interface switchport
```

Mostra informações a cerca das portas do switch

## VLAN Trunk Protocol

VTP é um protocolo da Cisco que é usado para distribuir e sincronizar
informações de VLANs configuradas na rede. Ele permite uma manutenção
das VLANs por meio de pontos especificos da rede.

Para utilizar o VTP é necessário configurar um Switch como servidor e
outro como cliente.

### Switch Server

```
Switch(config)#vtp domain <nome>
```

O primeiro passo é criar um nome de domínio do VTP

```
Switch(config)#vtp mode server
```

Coloca o switch em modo servidor. Nesse modo o switch pode criar e
deletar VLANs, essas mudanças serão propagadas pela rede

```
Switch(config)#vtp password <senha>
```

Define uma senha na utilização do VTP. A senha definida deve ser
colocada em todos os dispositivos VTP, e deve ser a mesma em todos
eles

### Switch Client

```
Switch(config)#vtp mode client
```

O switch que utilizar esse modo não podera alterar as configurações
das VLANs.
Antes de configurar esse modo configure a interface que está
conversando com o servidor

### Switch transparent

```
Switch(config)#vtp mode transparent
```

Nessa configuração o switch não irá compartilhar seu banco de dados
porém ele irá encaminhar as mensagens VTP que chegam até ele. É
possível criar e deletar VLANs nesse switch, porém não será enviado
para outros.

## Roteamento VLAN

O processo de roteamento entre VLANs só pode ser feito por meio de
switchs multicamada(switch camada 3)

```
Switch(config)#int <interface>
Switch(config-if)#switchport trunk encapsulation dot1q
Switch(config-if)#switchport mode trunk
```

Esse comando serve coloar uma porta como um trunk e estabelecer qual o
tipo de encapsulamento que será usado. Nesse caso e o 802.1 o mais
utilizado

```
Switch(config)#int <interface>
Switch(config-if)#switchport trunk allowed vlan 10, 20-30
```

Esse comando serve para permitir somente algumas VLANs de serem
utilizadas na porta trunk

```
Switch(config)#int <interface>
Switch(config)#switchport trunk native vlan <id>
```

Esse comando é utilizado para alterar a vlan nativa. Por padrão a vlan
nativa usada para Protocolos como CDP, VTP e outros é a VLAN 1

```
Switch(config)#interface vlan <id>
Switch(config-if)#ip address <ip> <máscara>
Switch(config-if)#no shut
```

Esse comano serve para estabelecer um endereço IP para a VLAN que
tiver sido criada no roteador. O comando no shut não é essencial,
porém usá-lo é de boa prática

```
Switch(config)#ip routing
```

Esse comando é o responsável por permitir que o roteamento entre VLANs
se esse comando não for ativado o roteamento simplesmente não irá
funcionar

## Cisco Discovery Protocol

Os equipamentos Cisco, por padrão, utilizam o protocolo CDP para
verificar os equipamentos diretamente conectados. É um protocolo
criado pela Cisco

```
Switch(config)#cdp run
```

Ativa o CDP globalmente

```
Switch(config-if)#cdp enable
```

Ativa o CDP em uma interface

```
Switch(config)#clear cdp counters
```

Zera os contadores de tráfego

```
Switch(config)#show cdp
```

Exibe informações globais do CDP

```
Switch(config)#show cdp neighbor
```

Mostra os dispositivos conectados

```
Switch(config)#show cdp neighbor detail
```

Mostra os dipositios conectados ao switch em mais detalhes

```
Switch(config)#debug dcp events(ou ip)
```

Usado para solucionar problemas ou monitorar informações de eventos/IP
do CDP

## Link Layer Discovery Protocol

O protocolo LLDP foi criado com a mesma intenção do CDP. Assim como
o CDP ele também consegue arrecadar informações sobre dispositivos
vizinhos.

```
Switch(config-if)#lldp transmit
Switch(config-if)#lldp receive
```

O primeiro comando especifica que pacotes LLDP são enviados dessa
interface. E o segundo especifica que eles são recebidos

Aqui vai alguns comandos opcionais

```
Switch(config-if)#lldp holdtime <segundos>
```

Especifica a quantidade de tempo que um dispositivo deve segurar
informações antes de descartar. Ele aceita de 0 a 65355 segundos

```
Switch(config-if)#lldp reinit <2-5>
```

Especifica o tempo de demora para o LLDP iniciar em qualquer interface
Ele aceita especificações de 2 a 5 segundos

```
Switch(config-if)#lldp timer <segundos>
```

Especifica a frequência de transmissões de atualizações LLDP em
segundos. Ele aceita de 5 a 65534

## Spanning Tree Protocol

O protocolo STP é utilizado em redes, para prover caminhos
alternativos no caso de falhas de ligações. E também é utilizado para
evitar a formação de loops entre switchs, ativando e desativando
caminhos alternativos.

```
Switch(config)#spanning-tree mode pvst
```

Modo padrão utilizado pelos Switchs

```
Switch(config)#spanning-tree mode rapid-pvst
```

Rapid PVST+ utiliza uma conexão de ponta a ponta para realizar uma
convergência mais rapída. Nessa configuração a reconfiguração ocorre
em 1 segundo diferente do STP padrão que ocorre em até 50 segundos

```
Switch(config)#spanning-tree mode msp
```

Esse modo é utilizado em conexões onde as redundâncias possam vir a
causar problema. Muito usado em redes onde há uma topologia em anel

Por padrão é elegido o switch root automáticamente a partir do menor
MAC address da rede, mas é possível alterar isso por manualmente

```
Switch(config)#spanning-tree vlan <id> priority <id>
```

Aqui estamos colocando uma prioridade em cada uma das VLAN. O número
de prioridade padrão é o 32768, quanto menor o número da prioridade
mais prioridade o switch possui

```
Switch(config)#int <interface>
Switch(config-if)#spanning-tree portfast
```

Esse comando serve para dizer ao dispositivo quais as portas que serão
as de acesso de dispositivos de trabalho(PC). Esse comando só deve
ser ativado em locais onde não haverá outro switch, para evitar loops

```
Switch(config)#spanning-tree portfast bpduguard
```

Ativa o BPDU guard em todas as portfast do switch. O BPDU guard serve
para dar uma camada de segurança maior para evitar loops, caso um
BPDU seja recebido em uma das portfast a porta será colocada em um
estado de desativada

```
Switch(config)#int <interface>
Switch(config-if)#spanning-tree bpduguard
```

Ativa o BPDU guard somente na interface especificada

+Troubleshooting

```
Switch#show spanning-tree bridge id
```

Mostra o id é o MAC address do switch

```
Switch#show spanning-tree root
```

Mostra configurações sobre o root

```
Switch#show spanning-tree
```

Mostra informações sobre o STP no switch, é com esse comando que
iremos identificar o switch root

## EtherChannel

O EtherChannel é um protocolo que pode ser utilizado entre dois
switchs para tornar um grupo de portas em um único canal lógico. Sua
utilização costuma se dar quando estamos utilizado o STP, onde algumas
portas são bloqueadas, para que isso não ocorra podemos utilizar este
protocolo.
Os protocolos que dividem o EtherChannel são PAgP(Cisco), LACP(não
proprietário)

```
Switch(config)#interface <int>
Switch(config-if)#switchport trunk encapsulation dot1q
Switch(config-if)#switchport mode trunk
```

Como primeiro passo é importante que as configurações entre as portas
do switch sigam um padrão, todas as portas que vão ser utilizadas no
canal devem ser configuradas de maneira igual. Nesse caso as portas
foram colocadas como trunk, pois geralmente é utilizado VLANs junto
a esses protocolos.

```
Switch(config-if)#channel-group <id> mode ?
```

Ainda dentro da interface podemos estabelecer o modo do canal. Existem
alguns modos que podem ser utilizados são eles:
active Enable LACP unconditionally
passive Enable LACP only if a LACP device is detected
auto Enable PAgP only if a PAgP device is detected
desirable Enable PAgP unconditionally
on Enable Etherchannel only
Perceba que existe o LACP e o PAgP. A configuração escolhida deve ser
a mesma nos dois switches

```
Switch(config)#int port-channel <id>
Switch(config-if)#switchport trunk encapsulation dot1q
Switch(config-if)#switchport mode trunk
```

Depois de ter ativado o canal é necessário configurá-lo.

Novamente as configurações devem ser iguais nos dois dispositivos,
para que não tenha um incompatibilidade entre eles.

```
Switch#sh etherchannel port-channel
```

Mostra informações sobre os canais

```
Switch#sh etherchannel summary
```

Mostra informações sobre os canais de maneira resumida

## Security Ports

É possível trazer mais segurança a rede, configurando seu switch para
receber informações vindas somente de portas conhecidas pelo sistema.
Isso é feito por uma filtragem do endereço MAC

```
Switch(config)#interface <interface>
Switch(config-if)#switchport port-security maximum <número>
```

Define a quantidade de endereços MAC que podem acessar esta porta.
O padrão a ser utilizado é 1

```
Switch(config)#interface <interface>
Switch(config-if)#switchport port-security mac-address <endereço-MAC>
```

Nessa configuração estamos delimitando a utilização dessa porta a
somente o endereço MAC especificado

```
Switch(config)#interface <interface>
Switch(config-if)#switchport port-security mac-address sticky
```

Delimita a utilização dessa porta a partir do endereço MAC que está
sendo utilizado nessa interface nesse instante.

**OBS:** A ordem na qual os computadroes estavam conectados também é
consideadro, o computador conectado na porta um só se conecta a porta
um.

```
Switch(config)#interface <interface>
Switch(config-if)#switchport port-security violation {protect|restrict
shutdown}
```

Essa configuração serve para ditar ao switch o que ele deve fazer em
caso de violação da porta.
Protect, apenas restrigem o uso da porta
Restric, restrigem o uso da porta e gera um log
Shutdown, gera um log e derruba a porta. Para ativá-la é necessário
acessá-la manualmente

```
Switch(config)#terminal monitor
```

registra o que acontece nas portas

```
Switch(config)#errdisable recovery cause psecure-violation
Switch(config)#errdisable recovery interval <segundos>
```

O primeiro comando serve para habilitar a recuperação da porta caso
uma violação de segurança aconteça. Já o segundo comando estabelece os
segundos de intervalo de espera que vão acontecer antes da porta ser
reabilitada novamente

**Recovery**

```
Switch(config)#errdisable recovery cause ?
```

Mostra algumas opções de recuperação caso um switch entre em um
estado de errdisable(porta desligada).

```
Switch(config)#errdisabel recovery cause psecurity-violation
Switch(config)#errdisable recovery in
Switch(config)#errdisable recovery interval <timer>
```

Este é o comando mais comumente usado, configurado junto com a
segurança das portas. Neste comando estamos dizendo que no caso
de haver uma violação de segurança após um determinado período de
tempo será verificado e se o problema tiver sido resolvido a
porta volta a ser religada.

**Troubleshooting**

```
Switch#show port-security
```

Mostra informações de segurança das portas

```
Switch#show port-security interface <interface>
```

Observar as configurações de segurança em uma determinada interface

## DHCP Snooping

DHCP Snooping é uma configuração que pode ser colocado no switch
para garantir a segurança da rede, impedindo que um servidor DHCP
pirata consiga enviar informações para as máquinas na rede.

```
Switch(config)#ip dhcp snooping
Switch(config)#ip dhcp snooping vlan <id>
```

O primeiro comando habilita o dhcp snooping no switch. Já o
segundo serve para habilitar está função em cada vlan

```
Switch(config)#int <interface>
Switch(config-if)#ip dhcp snooping trust
```

Este comando serve para garantir confiança para determinada porta
Basicamente ele está dizendo que confia em determinada porta para
receber pacotes DHCP(é a interface onde vai estar o servidor DHCP)

```
Switch(config)#no ip dhcp snooping information
```

Este comando serve para desabilitar as opções do DHCP, tal qual
opção 150(TFTP). Este comando serve para evitar o conflito que a
opção 82 pode vir a causar no envio das mensagens.

## DAI

Dinamyc ARP Inspection, assim como o DHCP Snooping serve para
garantir que ninguém malicioso passe um endereço MAC falsificado
para pessoas na rede

```
Switch(config)#ip arp inspection vlan <id>
```

Este comando habilita está verificação de endereços MAC de
maneira global

```
Switch(config)#int <interface>
Switch(config-if)#ip arp inspection trust
```

Garante confiança para determinada interface. Ou seja não realiza
a verificação ARP na interface especificada

## Syslog

```
Switch#set logging server <ip_server>
```

Especifica o servidro logging no qual as mensagens serão enviadas

```
Switch#set logging timestamp {enable | disable}
```

Habilita ou desabilita o carimbo de data/hora, pra ser usado nos logs
que são enviados

```
Switch#set logging server severity <level>
```

Especifica o nível de severidade das mensagens que serão enviadas

```
Switch#set logging server enable
```

Permite que o switch envie mensagens log para os servidores syslog

```
Switch#show logging
```

Mostra informações sobre o logging
