# Network Flow — xsilent風

Este documento descreve o **fluxo de tráfego de rede** no projeto xsilent風, com o objetivo de tornar explícito **como os dados trafegam**, **onde há controles** e **onde existem possíveis vazamentos**.

O foco é **clareza técnica**, não promessa de anonimato.

---

## 1. Visão geral do fluxo

De forma simplificada, o fluxo de rede segue o seguinte caminho:

```TXT
Aplicações do usuário
↓
iptables (redirecionamento parcial)
↓
Tor (SOCKS / TransPort)
↓
Rede Tor
↓
Destino externo
```

A resolução DNS pode seguir **caminhos distintos**, dependendo da configuração ativa.

---

## 2. Tráfego TCP

### Fluxo esperado

- Aplicações enviam tráfego TCP
- Regras de `iptables` redirecionam portas 80 e 443
- Tráfego é encaminhado para o **Tor TransPort**
- Tor encapsula o tráfego na rede Tor
- O IP de saída visível ao destino é o **exit node**

### Observações importantes

- Apenas tráfego coberto pelas regras é redirecionado
- Tráfego TCP fora dessas portas pode sair direto
- Aplicações mal configuradas podem ignorar SOCKS

---

## 3. Tráfego UDP

### Situação atual

- Apenas DNS (porta 53) é tratado explicitamente
- Outros fluxos UDP **não são bloqueados por padrão**

### Implicações

- Aplicações que usam UDP fora do DNS podem:
  - vazar IP real
  - contornar Tor
- Não há política de *default deny*

Isso é uma **limitação conhecida e intencional**, mantida para fins educacionais.

---

## 4. Resolução DNS

### Cenários possíveis

#### 4.1 DNS via Tor (TransPort / DNSPort)

- Consultas DNS são encaminhadas ao Tor
- Resolução ocorre dentro da rede Tor
- Reduz exposição ao provedor local

#### 4.2 DNS via DNSCrypt

- Consultas são criptografadas até o resolvedor
- O resolvedor externo ainda vê o IP de origem
- Não há anonimato, apenas confidencialidade

### Ponto crítico

O uso conjunto de **Tor + DNSCrypt** é **experimental**.

Dependendo da ordem e configuração:
- Pode haver redundância
- Pode haver vazamento de metadata
- Pode haver falsa sensação de segurança

Este comportamento exige análise cuidadosa por parte do usuário.

---

## 5. IPv6

### Situação atual

- IPv6 **não é tratado**
- Se ativo no sistema, pode:
  - ignorar regras IPv4
  - vazar tráfego diretamente

Desabilitar ou tratar IPv6 é uma melhoria possível, mas não implementada neste projeto.

---

## 6. Serviços locais

- Serviços desnecessários foram minimizados
- Qualquer serviço ativo:
  - aumenta superfície de ataque
  - pode gerar tráfego não esperado

A responsabilidade de auditoria é do usuário.

---

## 7. Pontos de vazamento conhecidos

Este projeto **não protege contra**:

- Fingerprinting em nível de aplicação
- Vazamentos via WebRTC
- Tráfego UDP não tratado
- Aplicações que ignoram proxy
- Erro humano

Esses riscos são **aceitos e documentados**.

---

## 8. Considerações finais

O fluxo de rede da xsilent風 deve ser visto como:

- Um exercício prático de entendimento de tráfego
- Um ambiente de testes
- Uma base para experimentação

Não é um modelo fechado, completo ou seguro para uso operacional sensível.

A clareza sobre o fluxo é mais importante do que a ilusão de segurança.

---
