# 🏢 Mini-Tech-Office

Projeto de infraestrutura de rede corporativa simulada desenvolvido no **Cisco Packet Tracer**.

O objetivo deste projeto foi criar uma pequena rede empresarial utilizando conceitos fundamentais de redes de computadores, como:

* VLANs para segmentação da rede.
* Trunk para comunicação entre dispositivos de rede.
* Router-on-a-Stick para comunicação entre VLANs.
* DHCP para distribuição automática de endereços IP.
* Wi-Fi através de Access Point.
* ACL para controle de acesso entre departamentos.

A rede representa uma pequena empresa chamada **Mini-Tech-Office**, contendo diferentes setores com níveis de acesso diferentes.

---

# 🎯 Objetivos do projeto

Implementar uma infraestrutura de rede organizada, segura e escalável contendo:

* Separação dos setores através de VLANs.
* Comunicação entre redes diferentes utilizando roteamento.
* Configuração automática dos computadores.
* Rede sem fio para visitantes.
* Controle de acesso utilizando regras de segurança.

---

# 🖥️ Topologia da rede

A topologia criada contém:

* 1 Router Cisco
* 1 Switch Cisco
* 1 Access Point
* Computadores dos setores:

  * Recepção
  * TI
  * Diretoria
  * Visitantes
  * Servidores

![Topologia](screenshots/01-topology.png)

---

# 🌐 Planejamento de VLANs

As VLANs foram utilizadas para separar logicamente os departamentos.

Cada setor possui sua própria rede, aumentando a organização e segurança.

| VLAN | Nome       | Departamento             | Rede            |
| ---- | ---------- | ------------------------ | --------------- |
| 10   | RECEPCAO   | Recepção                 | 192.168.10.0/24 |
| 20   | TI         | Tecnologia da Informação | 192.168.20.0/24 |
| 30   | DIRETORIA  | Diretoria                | 192.168.30.0/24 |
| 40   | VISITANTES | Wi-Fi Visitantes         | 192.168.40.0/24 |
| 50   | SERVIDORES | Servidores               | 192.168.50.0/24 |

---

# 🔀 Configuração das VLANs

As VLANs foram criadas no Switch para separar os dispositivos.

Comando utilizado:

```bash
enable
configure terminal

vlan 10
name RECEPCAO

vlan 20
name TI

vlan 30
name DIRETORIA

vlan 40
name VISITANTES

vlan 50
name SERVIDORES
```

## Verificação:

```bash
show vlan brief
```

Esse comando mostra:

* VLANs existentes.
* Nome das VLANs.
* Portas associadas.

Resultado:

![VLANs](screenshots/02-vlans.png)

---

# 🔗 Configuração do Trunk

O trunk permite transportar múltiplas VLANs através de uma única conexão física.

Neste projeto, o link entre Switch e Router foi configurado como trunk.

Comando:

```bash
interface gigabitEthernet 0/1

switchport mode trunk
```

Verificação:

```bash
show interfaces trunk
```

O resultado mostra:

* Interface em modo trunk.
* Protocolo 802.1Q.
* VLANs permitidas.

![Trunk](screenshots/03-trunk.png)

---

# 🌐 Router-on-a-Stick

Como o Router precisa se comunicar com várias VLANs, foram criadas subinterfaces.

Cada subinterface funciona como gateway de uma VLAN.

Configuração:

```bash
interface gigabitEthernet 0/0.10

encapsulation dot1Q 10

ip address 192.168.10.1 255.255.255.0
```

Exemplo completo:

| Interface | VLAN | Gateway      |
| --------- | ---- | ------------ |
| G0/0.10   | 10   | 192.168.10.1 |
| G0/0.20   | 20   | 192.168.20.1 |
| G0/0.30   | 30   | 192.168.30.1 |
| G0/0.40   | 40   | 192.168.40.1 |
| G0/0.50   | 50   | 192.168.50.1 |

Verificação:

```bash
show ip interface brief
```

Esse comando mostra o estado das interfaces e os endereços IP configurados.

![Router Interfaces](screenshots/04-router-interfaces.png)

---

# 📡 Configuração DHCP

O DHCP foi utilizado para entregar automaticamente:

* Endereço IP.
* Máscara.
* Gateway.
* Configurações de rede.

Exemplo:

```bash
ip dhcp pool TI

network 192.168.20.0 255.255.255.0

default-router 192.168.20.1
```

Foram criados pools para:

* Recepção.
* TI.
* Diretoria.
* Visitantes.
* Servidores.

Verificação:

```bash
show ip dhcp pool
```

![DHCP Pools](screenshots/05-dhcp-pools.png)

---

## Clientes recebendo IP

Comando:

```bash
show ip dhcp binding
```

Esse comando mostra quais dispositivos receberam endereços através do DHCP.

![DHCP Bindings](screenshots/06-dhcp-bindings.png)

---

# 📶 Rede Wireless

Foi configurado um Access Point para fornecer acesso sem fio aos visitantes.

Configurações:

| Item    | Valor        |
| ------- | ------------ |
| SSID    | Visitantes   |
| VLAN    | 40           |
| Gateway | 192.168.40.1 |
| DHCP    | Ativo        |

O dispositivo conectado ao Wi-Fi recebeu IP automaticamente através do DHCP.

---

# 🔐 Configuração de ACL

A ACL foi criada para controlar o acesso da rede de visitantes.

Objetivo:

* Visitantes podem acessar sua própria rede.
* Visitantes não podem acessar servidores.
* Visitantes não podem acessar redes internas.

Configuração:

```bash
access-list 100 deny ip 192.168.40.0 0.0.0.255 192.168.50.0 0.0.0.255

access-list 100 deny ip 192.168.40.0 0.0.0.255 192.168.20.0 0.0.0.255

access-list 100 deny ip 192.168.40.0 0.0.0.255 192.168.30.0 0.0.0.255

access-list 100 permit ip any any
```

Aplicação na interface:

```bash
interface gigabitEthernet 0/0.40

ip access-group 100 in
```

Verificação:

```bash
show access-lists
```

![ACL Rules](screenshots/09-acl-rules.png)

Verificação da aplicação:

```bash
show ip interface gigabitEthernet 0/0.40
```

![ACL Interface](screenshots/10-acl-interface.png)

---

# 🧪 Testes realizados

## Visitantes → Gateway

Teste:

```bash
ping 192.168.40.1
```

Resultado:

✅ Comunicação funcionando.

---

## Visitantes → Servidor

Teste:

```bash
ping 192.168.50.10
```

Resultado:

❌ Bloqueado pela ACL.

![Teste ACL](screenshots/07-ping-acl-test.png)

---

## Rede interna → Servidor

Teste:

```bash
ping 192.168.50.10
```

Resultado:

✅ Comunicação permitida.

![Teste Interno](screenshots/08-ping-interno-servidor.png)

---

# 📚 Conceitos aprendidos

## VLAN

Permite criar redes lógicas separadas dentro do mesmo switch físico.

Benefícios:

* Organização.
* Segurança.
* Redução de broadcast.

---

## Trunk

Permite transportar várias VLANs através de um único cabo utilizando o protocolo 802.1Q.

---

## Router-on-a-Stick

Técnica utilizada para permitir comunicação entre VLANs usando subinterfaces em um único link físico.

---

## DHCP

Protocolo responsável pela configuração automática dos dispositivos da rede.

---

## ACL

Recurso de segurança utilizado para permitir ou bloquear determinados tipos de tráfego.

---

# 🛠️ Principais comandos utilizados

Ver VLANs:

```bash
show vlan brief
```

Ver trunk:

```bash
show interfaces trunk
```

Ver interfaces IP:

```bash
show ip interface brief
```

Ver DHCP:

```bash
show ip dhcp pool

show ip dhcp binding
```

Ver ACL:

```bash
show access-lists
```

Salvar configuração:

```bash
copy running-config startup-config
```

---

# ✅ Conclusão

O projeto Mini-Tech-Office simulou uma pequena infraestrutura empresarial utilizando conceitos fundamentais de redes Cisco.

Durante o desenvolvimento foram aplicados conhecimentos de:

* Switching.
* VLAN.
* Routing.
* DHCP.
* Wireless.
* Segurança de rede.
* Controle de acesso.

Este projeto serviu como prática para administração de redes e fundamentos de cibersegurança.
