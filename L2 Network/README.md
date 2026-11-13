Laboratório MPLS e VPLS com MikroTik, Proxmox e Linux

1. Sobre o projeto

Este projeto tem como objetivo implementar e validar uma comunicação Layer 2 (Camada 2) entre dois roteadores MikroTik, utilizando MPLS e VPLS.

A topologia foi construída utilizando:

-Um MikroTik físico com RouterOS 6.49.20.

-Um MikroTik virtual com RouterOS 7.16, hospedado no Proxmox VE.

-OSPF como protocolo de roteamento dinâmico.

-LDP para distribuição de labels MPLS.

-MPLS para encaminhamento de pacotes identificados por labels.

-VPLS para estabelecer um pseudowire Ethernet entre os roteadores.

-Bridges Linux no Proxmox para interligação das máquinas virtuais.

-VLAN 100 para conectar o computador físico ao domínio Layer 2 do VPLS.

-Uma máquina virtual Linux para realizar os testes de conectividade.

O objetivo final é permitir que um computador físico e uma máquina virtual Linux, conectados a equipamentos diferentes, consigam se comunicar como se estivessem conectados ao mesmo segmento Ethernet.

2. Arquitetura da solução

2.1. Topologia lógica

                 COMPUTADOR FÍSICO
                    Linux Debian
                         |
                   Interface física
                         |
                     VLAN 100
                  10.10.100.2/24
                         |
                         |
                  ether4 - VLAN 100
                  MIKROTIK FÍSICO
                  RouterOS 6.49.20
                  Router-ID: 10.255.0.2
                         |
                  bridge-vpls-teste
                         |
                     vpls-teste
                         |
                ===== MPLS / LDP =====
                         |
                  REDE DE TRANSPORTE
                    192.168.88.0/24
                         |
                  Proxmox vmbr0
                         |
                  ether1 - MikroTik
                  MIKROTIK VIRTUAL
                  RouterOS 7.16
                  Router-ID: 10.255.0.1
                         |
                     vpls-teste
                         |
                    bridge-vpls
                         |
                 Interface dedicada
                         |
                       vmbr2
                         |
                   VM Linux 100
                  10.10.100.1/24



2.2. Inventário de endereçamento

Equipamento: MikroTik físico

Interface: bridge1

Endereço: 192.168.88.1/24

Finalidade: Transporte

------------------------------------

Equipamento: MikroTik virtual

Interface: ether1

Endereço: 192.168.88.250/24

Finalidade: Transporte

------------------------------------

Equipamento: MikroTik físico

Interface: Loopback

Endereço: 10.255.0.2/32

Finalidade: Router-ID / LDP

------------------------------------

Equipamento: MikroTik virtual

Interface: Loopback

Endereço: 10.255.0.1/32

Finalidade: Router-ID / LDP

------------------------------------

Equipamento: MikroTik físico

Interface: ether4

Endereço: 192.168.5.1/24

Finalidade: Acesso/gerenciamento

------------------------------------

Equipamento: PC Linux

Interface: VLAN 100

Endereço: 10.10.100.2/24

Finalidade: Teste VPLS

------------------------------------

Equipamento: VM Linux

Interface: Nova interface

Endereço: 10.10.100.1/24

Finalidade: Teste VPLS
