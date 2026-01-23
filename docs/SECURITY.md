# Security Policy — xsilent風

Este documento descreve o escopo de segurança do projeto xsilent風, suas premissas, limites e como lidar com falhas ou riscos identificados.

---

##  Premissas de Segurança

A xsilent風 é uma Ghost Machine focada em **redução de rastros**, **minimização de superfície de ataque** e **controle consciente do usuário**.

Ela **não promete anonimato absoluto**.  
Ela fornece **ferramentas, configurações e decisões arquiteturais** para usuários que entendem os riscos e agem de forma disciplinada.

Segurança aqui é tratada como:
- Processo contínuo
- Responsabilidade compartilhada
- Trade-off consciente entre usabilidade e exposição

---

##  Modelo de Ameaça (Resumo)

A xsilent風 foi pensada para mitigar:
- Fingerprinting de navegador
- Vazamento de DNS
- Conexões diretas não intencionais
- Persistência forense simples em ambiente live

Ela **não protege contra**:
- Ataques direcionados (APT, correlação global)
- Comprometimento físico do dispositivo
- Malware em firmware ou BIOS
- Erros operacionais do próprio usuário (OPSEC falha)

---

##  Fora do Escopo

Este projeto **não cobre**:
- Hardening de firmware
- Anonimato contra adversários estatais
- Segurança pós-comprometimento
- Uso malicioso ou ilegal das ferramentas

Qualquer tentativa de uso fora desses limites **é responsabilidade exclusiva do usuário**.

---

##  Navegadores e Superfície de Ataque

Cada navegador incluído no projeto possui **perfil de risco distinto**:

- **Tor Browser**
  - Melhor proteção contra fingerprinting
  - Maior custo de usabilidade e performance
  - Dependente de disciplina operacional

- **qutebrowser**
  - Interface minimalista, menor exposição por padrão
  - Alto risco se mal configurado
  - Usuário precisa entender cada ajuste aplicado

- **Falkon**
  - Alternativa leve baseada em QtWebEngine
  - Menor foco em anonimato, uso pontual recomendado

- **Lynx**
  - Navegação em modo texto
  - Superfície de ataque mínima
  - Ideal para consultas rápidas e ambientes críticos

Nenhum navegador é “seguro por si só”.  
Configuração + comportamento definem o risco real.

---

##  Responsabilidade do Usuário

Ao utilizar a xsilent風, o usuário concorda que:
- Entende os limites técnicos do projeto
- Assume responsabilidade por sua OPSEC
- Não confia cegamente em ferramentas
- Evita misturar identidades, sessões e contextos

---

##  Reporte de Vulnerabilidades

Se você identificar:
- Vazamento de DNS
- Configuração insegura por padrão
- Falhas críticas de isolamento

Abra uma **Issue** descrevendo:
- Ambiente utilizado
- Passos para reprodução
- Impacto potencial

Relatórios responsáveis são bem-vindos.  
Exploração irresponsável não.

---

##  Filosofia Final

> Anonimato não é um estado.  
> É um comportamento sustentado por decisões difíceis.

Se você busca conforto, este projeto não é para você.  
Se busca controle consciente, bem-vindo.
