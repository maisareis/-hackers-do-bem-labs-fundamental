# Hackers do Bem – Labs Fundamental

Documentação das atividades práticas do curso **Fundamental do Hackers do Bem** (RNP/ESR).

---

## Objetivo
Este repositório documenta o aprendizado prático obtido nas atividades de laboratório do curso, abordando conceitos de segurança da informação, administração de sistemas Linux e Windows Server.

---

## Módulo 1 – Fundamentos de Segurança e OSINT

| Atividade | Tema |
|-----------|------|
| [1.1 – Integridade com diff](modulo-01/atividade-1.1-integridade-diff.md) | Comparação de arquivos de texto |
| [1.2 – Integridade com cmp](modulo-01/atividade-1.2-integridade-cmp.md) | Comparação de arquivos binários |
| [1.3 – Hash MD5](modulo-01/atividade-1.3-hash-md5.md) | Verificação de integridade com hash |
| [1.4 – ROT13 e Cifra de César](modulo-01/atividade-1.4-cifragem-rot13-cesar.md) | Criptografia clássica |
| [1.5 – Taskwarrior](modulo-01/atividade-1.5-taskwarrior.md) | Gerenciamento de tarefas via CLI |
| [1.6 – Phishing com ShellPhish](modulo-01/atividade-1.6-phishing-shellphish.md) | Simulação de ataque de phishing |
| [1.7 – WHOIS](modulo-01/atividade-1.7-whois.md) | Reconhecimento de domínios |
| [1.8 – Maltego](modulo-01/atividade-1.8-maltego.md) | Configuração da ferramenta OSINT |
| [1.9 – OSINT com Maltego](modulo-01/atividade-1.9-osint-maltego.md) | Reconhecimento passivo |
| [1.10 – Adversarial Machine Learning](modulo-01/atividade-1.10-adversarial-ml.md) | Ataques contra IA |

---

## Módulo 2 – Malware, Criptografia e Anonimato

| Atividade | Tema |
|-----------|------|
| [2.1 – Trojan com Social-Engineer Toolkit](modulo-02/atividade-2.1-trojan-set.md) | Criação de RAT com SET e Metasploit |
| [2.2 – Keylogger XSPY](modulo-02/atividade-2.2-keylogger-xspy.md) | Captura de teclas no Kali Linux |
| [2.3 – Ransomware WannaCry](modulo-02/atividade-2.3-ransomware.md) | Exploração educacional de ransomwares |
| [2.4 – Payloads com Brutal](modulo-02/atividade-2.4-brutal-payloads.md) | Múltiplos payloads maliciosos |
| [2.5 – Windows Defender vs Keylogger](modulo-02/atividade-2.5-windows-defender-keylogger.md) | Detecção de malware no Windows |
| [2.6 – Criptografia com Ccrypt](modulo-02/atividade-2.6-ccrypt.md) | Cifração e decifração de arquivos |
| [2.7 – Análise de Logs com Logcheck](modulo-02/atividade-2.7-logcheck.md) | Controle detectivo via logs |
| [2.8 – Navegador Tor](modulo-02/atividade-2.8-tor-browser.md) | Anonimato e mascaramento de IP |
| [2.9 – DeepWeb com Tor](modulo-02/atividade-2.9-deepweb-tor.md) | Navegação em sites .onion |
| [2.10 – Políticas de Segurança Windows](modulo-02/atividade-2.10-politicas-seguranca-windows.md) | secpol e controles gerenciais |

---

## Módulo 3 – Ferramentas de Segurança e Reconhecimento de Rede

| Atividade | Tema |
|-----------|------|
| [3.1 – Exploit Database](modulo-03/atividade-3.1-exploitdb.md) | Pesquisa de exploits com searchsploit |
| [3.2 – Varreduras com Nmap](modulo-03/atividade-3.2-nmap.md) | Descoberta de hosts e portas |
| [3.3 – Burp Suite](modulo-03/atividade-3.3-burpsuite.md) | Interceptação de tráfego HTTP/HTTPS |
| [3.4 – Netcat: Escuta de Requisições](modulo-03/atividade-3.4-netcat-escuta.md) | Captura de cabeçalhos HTTP por porta |
| [3.5 – Netcat: Redirecionamento de Portas](modulo-03/atividade-3.5-netcat-redirect.md) | Port forwarding com Ncat |
| [3.6 – Diagnóstico de Rede](modulo-03/atividade-3.6-diagnostico-rede.md) | ifconfig, ping, traceroute e netstat |
| [3.7 – Honeypot com PentBox](modulo-03/atividade-3.7-honeypot-pentbox.md) | Detecção de intrusão simulada |
| [3.8 – Enumeração DNS com host](modulo-03/atividade-3.8-dns-host.md) | Registros A, AAAA, MX e NS |
| [3.9 – Enumeração DNS com nslookup](modulo-03/atividade-3.9-dns-nslookup.md) | Registros NS, MX e TXT |
| [3.10 – Enumeração DNS com dig](modulo-03/atividade-3.10-dns-dig.md) | Análise completa de respostas DNS |

---

## Módulo 4 – Controle de Acesso, Autenticação e Gerenciamento de Credenciais

| Atividade | Tema | Ferramenta |
|-----------|------|------------|
| [4.1 – Controle de Autenticação](modulo-04/atividade-4.1-controle-autenticacao.md) | passwd e arquivos de usuário | passwd / /etc/passwd |
| [4.2 – SELinux](modulo-04/atividade-4.2-selinux.md) | Controle de acesso obrigatório (MAC) | selinux-activate / sestatus |
| [4.3 – KeePassXC](modulo-04/atividade-4.3-keepassxc.md) | Cofre de senhas criptografado | KeePassXC |
| [4.4 – John the Ripper](modulo-04/atividade-4.4-john-the-ripper.md) | Ataque de dicionário off-line | John the Ripper |
| [4.5 – Credential Manager](modulo-04/atividade-4.5-credential-manager-windows.md) | Gerenciamento de credenciais Windows | Credential Manager |
| [4.6 – FreeRADIUS](modulo-04/atividade-4.6-freeradius.md) | Servidor de autenticação RADIUS | FreeRADIUS |
| [4.7 – Google Authenticator](modulo-04/atividade-4.7-google-authenticator.md) | TOTP com smartphone | Google Authenticator |
| [4.8 – Authenticator Firefox](modulo-04/atividade-4.8-authenticator-firefox.md) | TOTP no navegador | Plugin Authenticator |
| [4.9 – TPM e USB](modulo-04/atividade-4.9-tpm-usb.md) | Verificação de hardware de segurança | journalctl / usbview |
| [4.10 – OTPClient](modulo-04/atividade-4.10-otpclient.md) | Gerenciamento de tokens TOTP | OTPClient |

---

## Módulo 5 – Active Directory, Controle de Acesso e Políticas de Senha

| Atividade | Tema |
|-----------|------|
| [5.1 – Active Directory no Windows Server 2022](modulo-05/atividade-5.1-active-directory.md) | Instalação do AD DS e DNS Server |
| [5.2 – Domain Controller do Active Directory](modulo-05/atividade-5.2-domain-controller.md) | Promoção a DC e configuração DNS |
| [5.3 – Usuário no Domínio AD](modulo-05/atividade-5.3-usuario-dominio-ad.md) | Criação de usuário, OU e GPO de acesso RDP |
| [5.4 – Cliente no Domínio AD](modulo-05/atividade-5.4-cliente-dominio-ad.md) | Ingresso de cliente Windows no domínio |
| [5.5 – Política de Senhas no GPO](modulo-05/atividade-5.5-gpo-politica-senhas.md) | Complexidade, tamanho mínimo e histórico |
| [5.6 – DAC no Windows Server 2022](modulo-05/atividade-5.6-dac-windows.md) | Permissões NTFS com Allow e Deny |
| [5.7 – Organizational Unit no AD](modulo-05/atividade-5.7-organizational-unit.md) | Criação de OU e usuário com Common Name |
| [5.8 – DAC no Kali Linux](modulo-05/atividade-5.8-dac-kali.md) | Permissões POSIX com chmod |
| [5.9 – RBAC no Kali Linux](modulo-05/atividade-5.9-rbac-kali.md) | Grupos e controle de acesso por função |
| [5.10 – Rotação de Senha no Kali Linux](modulo-05/atividade-5.10-rotacao-senha.md) | Políticas de validade com /etc/login.defs |

---

## Módulo 6 – Programação e Segurança Web

| Atividade | Tema |
|-----------|------|
| [6.1 – Reuso de Código com Python](modulo-06/atividade-6.1-reuso-codigo-python.md) | Redimensionamento de imagens com Pillow |
| [6.2 – JDK e IDE NetBeans](modulo-06/atividade-6.2-jdk-netbeans.md) | Instalação do Java e NetBeans |
| [6.3 – Automação com Python](modulo-06/atividade-6.3-automacao-python.md) | Coleta de informações do sistema |
| [6.4 – ClamAV e Assinaturas de Malware](modulo-06/atividade-6.4-clamav.md) | Antivírus baseado em assinatura |
| [6.5 – Automação com PowerShell](modulo-06/atividade-6.5-automacao-powershell.md) | Scripts no Windows Server |
| [6.6 – HTTP e Codificação Percentual](modulo-06/atividade-6.6-http-codificacao-percentual.md) | curl e percent encoding |
| [6.7 – Encurtador de Links e QR Code](modulo-06/atividade-6.7-encurtador-qrcode.md) | TinyURL API e qrencode |
| [6.8 – Ataque Clickjacking](modulo-06/atividade-6.8-clickjacking.md) | Demonstração com iframe |
| [6.9 – Teste de Clickjacking](modulo-06/atividade-6.9-teste-clickjacking.md) | Verificação de vulnerabilidade |
| [6.10 – SQL Injection com SQLmap](modulo-06/atividade-6.10-sql-injection.md) | Exploração de banco de dados |

---

## Módulo 7 – Armazenamento, Backup e Sanitização

| Atividade | Tema |
|-----------|------|
| [7.1 – RAID 0](modulo-07/atividade-7.1-raid0.md) | Striping com mdadm |
| [7.2 – RAID 1](modulo-07/atividade-7.2-raid1.md) | Mirroring com mdadm |
| [7.3 – Rsync Manual](modulo-07/atividade-7.3-rsync-manual.md) | Replicação manual de arquivos |
| [7.4 – Rsync Automático](modulo-07/atividade-7.4-rsync-automatico.md) | Replicação automática com crontab |
| [7.5 – Backup OneDrive](modulo-07/atividade-7.5-onedrive-backup.md) | Replicação síncrona em nuvem |
| [7.6 – Backup Completo com Duplicity](modulo-07/atividade-7.6-backup-completo-duplicity.md) | Backup completo local |
| [7.7 – Backup Diferencial com Duplicity](modulo-07/atividade-7.7-backup-diferencial-duplicity.md) | Backup diferencial local |
| [7.8 – Formatação Completa](modulo-07/atividade-7.8-formatacao-completa.md) | wipefs e zeragem de superblocks |
| [7.9 – Sobrescrita Simples](modulo-07/atividade-7.9-sobrescrita-simples.md) | Sobrescrita com dd e urandom |
| [7.10 – Sobrescrita DoD 5220.22-M](modulo-07/atividade-7.10-sobrescrita-dod.md) | Padrão militar de sanitização |

---

## Aviso Legal
> As atividades de segurança documentadas aqui foram realizadas exclusivamente em ambientes controlados e autorizados para fins educacionais. O uso de qualquer técnica descrita em sistemas sem autorização prévia é ilegal.
