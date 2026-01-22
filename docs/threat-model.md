# Threat Model — xsilent風

Este documento descreve o **modelo de ameaça considerado** no projeto xsilent風, bem como **os limites explícitos** do que foi (e não foi) levado em conta durante o design do sistema.

O objetivo aqui não é afirmar segurança absoluta, mas **tornar visíveis as suposições, riscos e trade-offs** adotados ao longo do projeto.

---

## 1. Objetivo do Threat Model

O threat model da xsilent風 existe para:

- Guiar decisões técnicas
- Evitar falsas promessas de anonimato ou segurança
- Deixar explícito **o que está fora de escopo**
- Servir como exercício prático de raciocínio em OPSEC

Este projeto é **educacional e experimental**.

---

## 2. Ativos a serem protegidos

Os principais ativos considerados foram:

- Metadados básicos de navegação (DNS, IP de saída)
- Conteúdo de tráfego TCP padrão (HTTP/HTTPS)
- Persistência de dados locais criptografados
- Isolamento básico do sistema operacional

Não há proteção específica para:
- Identidade comportamental do usuário
- Fingerprinting avançado
- Correlação de tráfego em larga escala

---

## 3. Adversários considerados

### 3.1 Adversários parcialmente considerados

- **ISP ou provedor de rede local**
  - Capaz de observar tráfego não criptografado
  - Capaz de identificar uso de Tor
- **Operadores de Wi-Fi público**
  - Possível inspeção passiva de tráfego
- **Serviços externos**
  - Capazes de registrar IPs de saída do Tor
- **Observadores passivos**
  - Sem controle direto do endpoint

Esses adversários motivaram:
- Uso de Tor
- Criptografia de disco (LUKS)
- Redução de serviços desnecessários

---

### 3.2 Adversários fora de escopo

Os seguintes adversários **não são considerados** neste projeto:

- Atacantes com controle do hardware
- Ataques físicos ao dispositivo
- Malware pré-instalado ou firmware comprometido
- Adversários capazes de:
  - correlação global de tráfego
  - análise de fingerprinting avançado
  - exploração de side-channels
- Ataques direcionados (APT)

Proteger contra esses cenários exigiria:
- Hardware dedicado
- Sistemas amnésicos
- Padronização extrema
- Controles operacionais fora do escopo da xsilent風

---

## 4. Vetores de ataque considerados

### 4.1 Rede

- Vazamento de DNS
- Tráfego TCP direto fora do Tor
- Configurações incorretas de iptables
- Serviços ativos desnecessários

Medidas adotadas:
- Tor em modo SOCKS / TransPort
- DNSCrypt para estudo de resolução DNS
- Redirecionamento básico via iptables

Limitação conhecida:
- Regras não cobrem todos os cenários (IPv6, UDP amplo, leaks avançados)

---

### 4.2 Armazenamento

- Exposição de dados persistentes
- Acesso não autorizado ao dispositivo desligado

Medidas adotadas:
- LUKS + LVM

Limitação conhecida:
- Persistência aumenta superfície de ataque em comparação com sistemas amnésicos

---

### 4.3 Interface e usuário

- Erro humano
- Instalação incorreta
- Uso fora do escopo previsto

Limitação explícita:
> O usuário é considerado o maior vetor de risco.

Não há mecanismos avançados para impedir:
- identificação comportamental
- uso inseguro do navegador
- login em contas pessoais

---

## 5. Suposições de segurança

Este projeto assume que:

- O sistema é instalado corretamente
- O usuário entende as ferramentas que utiliza
- O ambiente não está sob ataque físico
- O hardware não está comprometido

Se qualquer uma dessas suposições falhar, **as garantias do sistema colapsam**.

---

## 6. Considerações sobre anonimato

A xsilent風 **não garante anonimato**.

Motivos principais:

- Alpine Linux não é padronizado para anonimato
- Customizações aumentam fingerprint
- Persistência preserva estado
- Comportamento do usuário não é controlado

O projeto explora **redução de exposição**, não invisibilidade.

---

## 7. Conclusão

A xsilent風 deve ser entendida como:

- Um laboratório técnico
- Um exercício de threat modeling
- Um ambiente para aprendizado prático

Ela **não substitui** soluções projetadas especificamente para anonimato forte, como:
- Tails
- Whonix
- Qubes OS

O valor do projeto está na **clareza das decisões**, não na eliminação absoluta de riscos.

---
