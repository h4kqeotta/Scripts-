# Security Scripts 🛠️

Scripts y herramientas propias para automatizar tareas de seguridad ofensiva.
Desarrollados durante la práctica en HTB, TryHackMe y DockerLabs.

> ⚠️ **Aviso legal**: Estas herramientas son exclusivamente para uso educativo
> y en entornos autorizados. Nunca usar en sistemas sin permiso explícito.

---

## 📁 Estructura

```
security-scripts/
├── README.md
├── recon/              ← Reconocimiento
│   └── port_scanner.py
├── enum/               ← Enumeración
│   └── dir_brute.sh
├── exploitation/       ← Explotación
│   └── (próximamente)
└── utils/              ← Utilidades generales
    └── hash_identifier.py
```

---

## 🔧 Scripts disponibles

### 🔍 Reconocimiento
| Script | Descripción | Lenguaje |
|--------|-------------|----------|
| `port_scanner.py` | Scanner de puertos con detección de servicios | Python |

### 📂 Enumeración
| Script | Descripción | Lenguaje |
|--------|-------------|----------|
| `dir_brute.sh` | Fuerza bruta de directorios web | Bash |

### 🔑 Utilidades
| Script | Descripción | Lenguaje |
|--------|-------------|----------|
| `hash_identifier.py` | Identifica el tipo de hash según su formato | Python |

---

## ⚙️ Requisitos

```bash
# Python
pip install requests colorama

# Herramientas del sistema
sudo apt install nmap gobuster
```

---

## 📌 Cómo usar

Cada script tiene su propio bloque de uso en el encabezado del archivo.
Ejemplo general:

```bash
python3 port_scanner.py -t 192.168.1.1 -p 1-1000
bash dir_brute.sh http://target.com /usr/share/wordlists/dirb/common.txt
```
