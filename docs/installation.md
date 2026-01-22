# Installation Guide — xsilent風

Este documento descreve o **processo de instalação** da xsilent風.

⚠️ **Aviso importante**  
Este processo é **manual**, **deliberadamente detalhado** e voltado para **aprendizado técnico**.  
Não execute comandos que você não entende.  
Este projeto **não é recomendado para uso operacional sensível**.

---

## 1. Pré-requisitos

- Pendrive: 8–16 GB
- Sistema Linux funcional
- Conhecimento intermediário em:
  - linha de comando
  - particionamento de disco
  - redes
- ISO do Alpine Linux (x86_64)

Download oficial:  
https://www.alpinelinux.org/downloads/

---

## 2. Criação do Live USB

Grave a ISO no pendrive:

```bash
sudo dd if=alpine-standard-[version]-x86_64.iso of=/dev/sdX bs=15M status=progress && sync
```
>> Substitua /dev/sdX pelo dispositivo correto.
>> Um erro aqui pode destruir dados.

--- 
## 3. Criação de partição persistente (opcional)
### 3.1 Criar partição
```BASH
sudo fdisk /dev/sdX
```
Passos:
- n → nova partição
- p → primária
  - escolher número disponível
- w → salvar

### 3.2 Formatar partição
```BASH
sudo mkfs.ext4 -L persistence /dev/sdX2
```

### 3.3 Configurar persistência
```BASH
sudo mkdir -p /mnt/persistence
sudo mount /dev/sdX2 /mnt/persistence
echo "/ union" | sudo tee /mnt/persistence/persistence.conf
sudo umount /mnt/persistence
```

>> Persistência aumenta superfície de ataque.

---
## 4. Boot e instalação base do Alpine
Inicie o sistema pelo pendrive e execute:
```BASH
setup-alpine
```
- Sugestões:
  - Teclado: br-abnt2
  - Hostname: xsilent
  - Interface: wlan0 ou eth0
  - DHCP: yes
  - Timezone: America/Sao_Paulo
  - SSH: opcional
  - Usuário: root (senha forte)
## 5. Particionamento e criptografia (LUKS + LVM)
### 5.1 Layout sugerido
```BASH
/dev/sdX1 → /boot (ext4, ~512MB)
/dev/sdX2 → LUKS + LVM (restante)
```

### 5.2 Criptografia
```BASSH
cryptsetup luksFormat /dev/sdX2
cryptsetup open /dev/sdX2 cryptlvm
```

### 5.3 LVM
```BASH
pvcreate /dev/mapper/cryptlvm
vgcreate xsilentvg /dev/mapper/cryptlvm
lvcreate -L 2G xsilentvg -n swap
lvcreate -l 100%FREE xsilentvg -n root
```

### 5.4 Formatar e montar
```BASH
mkfs.ext4 /dev/xsilentvg/root
mkswap /dev/xsilentvg/swap
mkfs.ext4 /dev/sdX1
mount /dev/xsilentvg/root /mnt
mkdir /mnt/boot
mount /dev/sdX1 /mnt/boot
swapon /dev/xsilentvg/swap
```

### 5.5 Instalar no novo root
```BASH
setup-disk -m sys /mnt
```
---

## 6. Repositórios e pacotes básicos
Após reiniciar no sistema instalado:
```BASH
cat <<EOF >> /etc/apk/repositories
https://dl-cdn.alpinelinux.org/alpine/v3.19/main
https://dl-cdn.alpinelinux.org/alpine/v3.19/community
https://dl-cdn.alpinelinux.org/alpine/edge/testing
EOF

apk update
apk add tor openvpn iptables macchanger dnscrypt-proxy git
```
---
## 7. Configuração de MAC spoofing (experimental)
```BASH
echo "macchanger wlan0" >> /etc/local.d/spoof.start
chmod +x /etc/local.d/spoof.start
rc-update add local default
```
>> Alerta: Pode causar problemas de conectividade.

---
## Configurando o TOR
```BASH
echo "SocksPort 9050" >> /etc/tor/torrc
echo "TransPort 9040" >> /etc/tor/torrc
echo "DNSPort 5353" >> /etc/tor/torrc

rc-update add tor default
service tor start
```
## 9. Regras básicas de iptables (educacional)
```BASH
iptables -F
iptables -t nat -A OUTPUT -p tcp --dport 80 -j REDIRECT --to-ports 9050
iptables -t nat -A OUTPUT -p tcp --dport 443 -j REDIRECT --to-ports 9050
iptables -t nat -A OUTPUT -p tcp --syn -j REDIRECT --to-ports 9040
iptables -t nat -A OUTPUT -p udp --dport 53 -j REDIRECT --to-ports 5353
```
Regras não são completas, [Leia network-flow.md](network-flow.md).

---

## 10. Interface gráfica (opcional)
Configuração baseada no projeto:
  - [https://github.com/NullByte-7w7/RICE-ARCH-KALI](https://github.com/NullByte-7w7/RICE-ARCH-KALI)
```BASH
git clone https://github.com/NullByte-7w7/RICE-ARCH-KALI.git
cd RICE-ARCH-KALI
chmod +x install.sh
./install.sh
```
---

## 11. Navegadores e Camada de Aplicação

Esta seção descreve os navegadores utilizados no projeto xsilent風 e **o papel específico de cada um** dentro do modelo de ameaça.

Nenhum navegador, isoladamente, garante anonimato.  
A segurança depende da **combinação entre fluxo de rede, configuração e comportamento do usuário**.

---

### 11.1 Tor Browser

**Objetivo**  
Uso pontual quando o foco é **redução de fingerprint** e navegação dentro das premissas da rede Tor.

**Instalação**

```bash
apk add torbrowser-launcher
```
Observações importantes
- O Tor Browser ignora configurações externas de proxy/DNS
- Ele já implementa:
  - isolamento de sessão
  - hardening de navegador
  - mitigação de fingerprinting
- Customizações não são recomendadas

Limitação
- Menor flexibilidade
- Não integrado ao fluxo manual do sistema
### 11.2 qutebrowser

#### Objetivo
Navegador principal para uso controlado, estudo de OPSEC e navegação via SOCKS5, assumindo maior responsabilidade do usuário.

#### Instalação
```BASH
apk add qutebrowser python3 py3-pip
```

#### Configuração básica de proxy (Tor SOCKS5)

Abra o qutebrowser e execute:
```BASH
:set content.proxy socks5://127.0.0.1:9050/
```
Para persistir a configuração:
```BASH
echo "config.set('content.proxy', 'socks5://127.0.0.1:9050')" >> ~/.config/qutebrowser/config.py
```
#### Observações
- Não protege contra fingerprinting avançado
- WebRTC deve ser desativado manualmente
- Totalmente dependente do comportamento do usuário
### 11.3 Falkon

#### Objetivo
Navegador gráfico leve, usado como comparativo e para navegação menos sensível, nunca como ferramenta de anonimato.

#### Instalação
```BASH
apk add falkon
```

#### Configuração de proxy (manual)

- Acesse:
  - `Settings → Network → Proxy`
- Configure:
  - SOCKS5
  - Host: 127.0.0.1
  - Port: 9050
-Limitação explícita
  - Falkon não é projetado para privacidade
  - Deve ser tratado como navegador comum atrás de um proxy
### 11.4 Lynx

#### Objetivo
Navegação text-only, baixo footprint e redução de superfície de ataque em nível de aplicação.

#### Instalação
```BASH
apk add lynx
```

#### Configuração para uso com Tor
- Edite o arquivo de configuração:
  ```BASH
    vi /etc/lynx/lynx.cfg  
  ```
- Adicione:
  ```BASH
  SOCKS_PROXY:127.0.0.1:9050
  ```
- Vantagens
  - Sem JavaScript
  - Sem execução de conteúdo ativo
  - Excelente para leitura e consultas simples
- Limitação
  - Usabilidade reduzida
  - Não substitui navegadores gráficos

### 11.5 Considerações de OPSEC (críticas)
- Misturar navegadores aumenta fingerprint comportamental
- Login em contas pessoais anula qualquer ganho técnico
- WebRTC, extensões e hábitos previsíveis são vetores de identificação
- O navegador é o maior ponto de falha do sistema
>> Nenhuma configuração aqui compensa erro humano.
### 11.6 Resumo de uso recomendado
```TABLE
| Navegador   | Uso pretendido                      | Grau de risco |
| ----------- | ----------------------------------- | ------------- |
| Tor Browser | Navegação padronizada via Tor       | Menor         |
| qutebrowser | Uso avançado e estudo de OPSEC      | Médio         |
| Falkon      | Navegação comum atrás de proxy      | Alto          |
| Lynx        | Leitura simples e mínima superfície | Baixo         |
```
---

## 12. Pós-instalação

- Remover pacotes desnecessários
- Revisar serviços ativos
- Auditar tráfego manualmente
- Ler:
  - [`threat-model.md`](threat-model.md)
  - [`decisions-and-limitations.md`](decisions-and-limitations.md)
  - [`network-flow.md`](network-flow.md)
## Nota final


Este guia descreve o que foi feito, não o que deve ser feito em todos os cenários.

A xsilent風 não impõe qual navegador usar.

Ela expõe as consequências de cada escolha.

A responsabilidade final é sempre do usuário.
