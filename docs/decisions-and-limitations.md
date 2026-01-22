# Decisions and Limitations — xsilent風

Este documento registra as **principais decisões técnicas** tomadas no projeto xsilent風, bem como suas **limitações conhecidas** e os **trade-offs conscientemente aceitos**.

O objetivo não é justificar cada escolha como “correta”, mas **explicitar o raciocínio** por trás delas e reconhecer onde o projeto falha ou poderia ser melhorado.

---

## 1. Uso do Alpine Linux

### Decisão
O Alpine Linux foi escolhido como base do sistema por sua:
- Superfície mínima
- Simplicidade estrutural
- Forte controle manual
- Adequação para ambientes customizados e containers

### Limitações
- A customização profunda aumenta a **fingerprint do sistema**
- Ausência de mecanismos de anonimato por padrão
- Menor proteção “opinionada” comparada a sistemas como Tails ou Whonix

### Trade-off consciente
Foi priorizado **controle e aprendizado** em detrimento de padronização e anonimato forte.

---

## 2. Persistência em Live USB

### Decisão
Foi adotado um modelo de Live USB com persistência para:
- Facilitar uso contínuo
- Permitir evolução incremental do ambiente
- Evitar reinstalação constante

### Limitações
- Persistência preserva estado e histórico
- Aumenta impacto de comprometimentos
- Reduz segurança comparado a sistemas amnésicos

### Trade-off consciente
Usabilidade e experimentação foram priorizadas em relação à segurança máxima.

---

## 3. Criptografia com LUKS + LVM

### Decisão
A criptografia de disco foi implementada para:
- Proteger dados em repouso
- Mitigar acesso não autorizado ao dispositivo desligado

### Limitações
- Não protege contra:
  - comprometimento do sistema em execução
  - ataques físicos avançados
- Depende de senha forte e disciplina operacional

---

## 4. Uso do Tor (standalone)

### Decisão
Tor foi utilizado em modo standalone para:
- Estudo de roteamento de tráfego
- Redução de exposição de IP de origem
- Compreensão prática de SOCKS e TransPort

### Limitações
- Não impede fingerprinting
- Uso de Tor é detectável por observadores de rede
- Não protege contra correlação global de tráfego

---

## 5. Uso conjunto de Tor e DNSCrypt

### Decisão
O uso de DNSCrypt foi explorado como exercício para:
- Entender resolução DNS
- Avaliar fluxo de consultas fora e dentro do Tor

### Limitações
- Pode haver redundância ou vazamentos se mal configurado
- Não adiciona anonimato quando todo tráfego já passa pelo Tor
- Exige entendimento claro do caminho do tráfego

Esta escolha é **experimental**, não recomendada como padrão operacional.

---

## 6. Regras de iptables

### Decisão
iptables foi utilizado para:
- Redirecionamento básico de tráfego
- Exercício prático de controle de fluxo

### Limitações
- Regras não são exaustivas
- IPv6 não é tratado
- Nem todo tráfego UDP é coberto
- Não há política de bloqueio total (default deny)

Essas regras devem ser vistas como **base educacional**.

---

## 7. MAC Address Spoofing

### Decisão
O spoofing de MAC foi implementado automaticamente para:
- Experimentar técnicas de alteração de identificadores de rede

### Limitações
- Não garante anonimato
- Pode introduzir novos padrões comportamentais
- Pode quebrar conectividade em redes conhecidas
- Pode chamar mais atenção do que evitar rastreamento

Esta prática é **contextual** e não universalmente recomendada.

---

## 8. Interface gráfica e customização

### Decisão
Foi adotado um ambiente gráfico minimalista (i3wm) para:
- Reduzir consumo de recursos
- Manter controle explícito do ambiente
- Evitar serviços desnecessários

### Limitações
- Ambiente não padronizado
- Customizações aumentam fingerprint
- Requer familiaridade do usuário

---

## 9. Comportamento do usuário

### Decisão
O projeto **não tenta controlar o comportamento do usuário**.

### Limitações
- Login em contas pessoais
- Uso de navegadores comuns
- Hábitos previsíveis

Esses fatores anulam qualquer ganho técnico de privacidade.

O usuário é considerado o **principal vetor de risco**.

---

## 10. O que seria feito de forma diferente hoje

Com base em maior maturidade técnica, hoje seriam consideradas:

- Separação mais clara entre ambientes amnésicos e persistentes
- Uso de sistemas projetados para anonimato quando esse for o objetivo
- Threat model mais restritivo antes da implementação
- Redução de customizações em cenários sensíveis

---

## Conclusão

A xsilent風 reflete um estágio específico de aprendizado técnico.

Seu valor está em:
- Tornar explícitos os trade-offs
- Documentar decisões
- Reconhecer limitações

Mais importante do que o resultado final é a **capacidade de avaliar criticamente as próprias escolhas**.

---
