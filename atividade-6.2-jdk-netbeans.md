# Atividade 6.2 – Conhecendo o SDK do Java (JDK) e o NetBeans

## Objetivo
Instalar e explorar o Java Development Kit (JDK) e o IDE Apache NetBeans no Kali Linux para desenvolvimento de aplicações Java.

## Conceitos

### JDK (Java Development Kit)
O **JDK** é o kit de desenvolvimento Java que inclui:
- **JRE** (Java Runtime Environment) → para executar programas Java
- **Compilador** (`javac`) → para compilar código Java
- **Ferramentas de desenvolvimento** → depurador, documentador, etc.

### IDE (Integrated Development Environment)
Um **IDE** é um ambiente de desenvolvimento integrado que reúne em uma única ferramenta:
- Editor de código com realce de sintaxe
- Compilador integrado
- Depurador
- Gerenciador de projetos

---

## Pré-requisitos
- Kali Linux com Terminal
- Arquivo de instalação do Apache NetBeans disponível

---

## Passo a Passo

### 1. Acessar como superusuário
```bash
sudo -i
```

### 2. Verificar a versão do JDK instalado
```bash
java --version
```

### 3. Navegar até a pasta do NetBeans
```bash
cd /curso/netbeans
```

### 4. Verificar o arquivo de instalação
```bash
ls
# apache-netbeans_19-1_all.deb
```

### 5. Instalar o NetBeans
```bash
dpkg -i apache-netbeans_19-1_all.deb
```

### 6. Abrir o Apache NetBeans
Acessar via menu: **Aplicativos → Desenvolvimento → Apache NetBeans**

---

## Resultado Esperado
O Apache NetBeans IDE aberto e pronto para desenvolvimento de aplicações Java.

## Aprendizado
- O JDK é essencial para desenvolver aplicações Java
- IDEs como NetBeans aumentam a produtividade do desenvolvedor
- O comando `dpkg -i` instala pacotes `.deb` no Debian/Kali Linux
