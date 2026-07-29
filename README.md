# 🖥️ Mini Tech Office

## 📌 Sobre o projeto

O **Mini Tech Office** é um laboratório de redes desenvolvido utilizando o **Cisco Packet Tracer**, simulando a infraestrutura de uma pequena empresa de tecnologia.

O objetivo do projeto é aplicar conceitos fundamentais de redes de computadores e segurança, implementando segmentação da rede, configuração automática de dispositivos e controle de acesso entre departamentos.

O ambiente foi projetado para representar um cenário próximo ao encontrado em pequenas e médias empresas, permitindo a prática de configuração e troubleshooting em equipamentos Cisco.

---

## 🏢 Cenário da empresa

A Mini Tech Office possui os seguintes setores:

* Recepção
* Tecnologia da Informação (TI)
* Diretoria
* Servidores
* Rede Wi-Fi para visitantes

Para melhorar a organização e a segurança da infraestrutura, a empresa solicitou uma nova arquitetura de rede contendo:

* Segmentação da rede utilizando VLANs.
* Comunicação entre equipamentos através de links Trunk.
* Distribuição automática de endereços IP utilizando DHCP.
* Rede Wi-Fi dedicada para visitantes.
* Controle de acesso utilizando ACL (Access Control List).
* Estrutura preparada para futuras expansões.

---

## 🎯 Objetivos do projeto

Neste laboratório serão implementados:

* VLANs.
* Trunk (802.1Q).
* Router-on-a-Stick.
* DHCP.
* Access Point (Wi-Fi).
* ACL.
* Testes de conectividade.
* Troubleshooting.

---

## 🛠️ Ferramenta utilizada

* Cisco Packet Tracer

---

## 🌐 Tecnologias e conceitos aplicados

### VLAN (Virtual Local Area Network)

As VLANs permitem dividir uma rede física em múltiplas redes lógicas, melhorando a organização, desempenho e segurança.

### Trunk (802.1Q)

Os links Trunk permitem o transporte de múltiplas VLANs através de uma única conexão entre dispositivos de rede.

### Router-on-a-Stick

Técnica utilizada para permitir a comunicação entre VLANs utilizando um único roteador e múltiplas subinterfaces.

### DHCP (Dynamic Host Configuration Protocol)

Responsável por fornecer automaticamente:

* Endereço IP
* Máscara de sub-rede
* Gateway padrão

### ACL (Access Control List)

As ACLs serão utilizadas para controlar quais dispositivos poderão acessar determinados recursos da rede.

### Wi-Fi

Uma rede sem fio será disponibilizada exclusivamente para visitantes, mantendo a rede corporativa isolada.

---

## 🖥️ Estrutura da rede

| VLAN    | Departamento | Dispositivos |
| ------- | ------------ | ------------ |
| VLAN 10 | Recepção     | 2 PCs        |
| VLAN 20 | TI           | 3 PCs        |
| VLAN 30 | Diretoria    | 1 PC         |
| VLAN 40 | Visitantes   | 1 Notebook   |
| VLAN 50 | Servidores   | 1 Servidor   |

---

## 🌐 Planejamento IP

| VLAN    | Rede            | Gateway      |
| ------- | --------------- | ------------ |
| VLAN 10 | 192.168.10.0/24 | 192.168.10.1 |
| VLAN 20 | 192.168.20.0/24 | 192.168.20.1 |
| VLAN 30 | 192.168.30.0/24 | 192.168.30.1 |
| VLAN 40 | 192.168.40.0/24 | 192.168.40.1 |
| VLAN 50 | 192.168.50.0/24 | 192.168.50.1 |

---

## 🔒 Política de segurança

A rede de visitantes possuirá as seguintes restrições:

* Não poderá acessar a VLAN da TI.
* Não poderá acessar a VLAN da Diretoria.
* Não poderá acessar a VLAN dos Servidores.
* Terá acesso apenas aos recursos permitidos pela ACL.

---

## 📡 Topologia da rede

> (Adicionar imagem da topologia aqui)

---

## 🚧 Status do projeto

Em desenvolvimento.
