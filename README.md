# Xsilent風
![Status](https://img.shields.io/badge/status-experimental-orange)
![Security](https://img.shields.io/badge/security-OS-_yellow)
![Privacy](https://img.shields.io/badge/privacy-threat--modeled-blueviolet)
![OS](https://img.shields.io/badge/base-Alpine_Linux-0D597F?logo=alpinelinux)
![Init](https://img.shields.io/badge/init-OpenRC-lightgrey)
![Audience](https://img.shields.io/badge/audience-advanced_users-black)
![UX](https://img.shields.io/badge/user%20experience-not_plug--and--play-critical)

A xsilent風 é um projeto experimental e educacional baseado em Alpine Linux, voltado para o estudo de hardening manual, controle explícito de tráfego, OPSEC básica e construção de sistemas minimalistas com persistência.

O objetivo do projeto não é garantir anonimato, mas explorar decisões técnicas, seus custos e limitações, ao montar um ambiente enxuto com foco em privacidade operacional e isolamento de serviços.

  "Este projeto deve ser entendido como um laboratório prático, não como uma solução pronta ou recomendada para cenários de alto risco."
## Objetivos do projeto

- Explorar o Alpine Linux como base mínima para sistemas customizados
- Entender o fluxo real de tráfego de rede (DNS, TCP, UD0P)
- Implementar Tor em modo standalone e analisar seus impactos
- Estudar persistência, criptografia em disco (LUKS + LVM) e seus trade-offs
- Trabalhar com configuração manual, evitando abstrações excessivas
- Servir como base para aprendizado em:
  - [✔] OPSEC
  - [✔] hardening
  - [✔] redes
  - [✔] containers (Docker)
## O que este projeto não é

- [❌] Não garante anonimato
- [❌] Não substitui Tails, Whonix ou Qubes
- [❌] Não é recomendado para uso operacional real
- [❌] Não protege contra fingerprinting avançado
- [❌] Não elimina riscos de erro humano

Persistência, customização profunda e escolhas fora do padrão aumentam a superfície de fingerprint. Isso é uma decisão consciente, não uma falha ignorada.
## Principais componentes

- Base: Alpine Linux (x86_64)
- Gerenciador gráfico: i3wm (configuração adaptada do projeto NullByte-7w7)
- Rede e privacidade:
  - Tor (SOCKS5 / TransPort)
  - DNSCrypt Proxy
  - iptables (redirecionamento básico)
  - MAC spoofing (experimental)
- Criptografia:
  - LUKS + LVM
- Persistência:
  - Live USB com partição persistente
## Por que Alpine Linux?

### Vantagens
- Superfície mínima
- Inicialização rápida
- Forte controle manual
- Ideal para montar sistemas do zero
- Excelente base para containers

### Custos
- Maior fingerprint
- Mais configuração manual
- Menos proteções “opinionadas” por padrão
- Maior chance de erro humano
Esses custos são parte intencional do aprendizado.
## Limitações e decisões conscientes
- Tor + DNSCrypt
  O uso conjunto é explorado para estudo de fluxo e resolução DNS, ciente de que pode haver redundância ou vazamentos se mal configurado.
- iptables
  As regras implementadas são básicas e não cobrem todos os vetores (IPv6, UDP amplo, leaks avançados). Servem como base educacional.
- MAC spoofing
  Implementado de forma automática apenas para fins experimentais. Não é uma prática universalmente recomendada e pode gerar novos identificadores comportamentais.
- Persistência
  Facilita uso contínuo, mas reduz segurança em comparação com sistemas amnésicos.
## Público-alvo
- Estudantes de segurança
- Pessoas interessadas em:
  - OPSEC
  - hardening manual
  - Linux minimalista
  - redes
- Quem quer entender o “como” e o “porquê”, não apenas executar receitas prontas
## Requisitos
1. Pendrive (8–16 GB)
2. Sistema Linux
3. ISO Alpine Linux x86_64
4. Conhecimento intermediário em Linux
5. Disposição para ler, errar e ajustar
## Instalação
A instalação não é simples por design.
Ela foi dividida em aproximadamente 10 etapas, cobrindo:
- Criação de Live USB
- Partição persistente
- Instalação manual
- Criptografia com LUKS + LVM
- Configuração de rede
- Hardening básico
- Ambiente gráfico

>> Veja a seção de instalação completa e leia antes de executar.
## Créditos

- Configuração visual (WM) baseada no trabalho de:
  - [NullByte-7w7](https://github.com/NullByte-7w7)
- Configurações gerais:
  - [Hexsilent水](https://github.com/rv157244)
## Considerações finais

A xsilent風 representa um momento específico da minha evolução técnica.
Hoje, várias decisões seriam feitas de forma diferente — e isso faz parte do valor do projeto.

Mais importante do que o resultado final é o processo de raciocínio, os erros cometidos e a clareza sobre suas limitações.
