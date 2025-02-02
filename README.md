# Homelab

## 📌 Opis projektu
Homelab to zestaw narzędzi i usług zarządzanych za pomocą Dockera i Kubernetes, umożliwiających wygodne uruchamianie i konfigurację aplikacji na własnym serwerze.

## 🏗 Struktura projektu
Projekt składa się z:
- **Dockerfile** – Pliki definiujące obrazy kontenerów.
- **Docker Compose** – Konfiguracja usług w kontenerach, przygotowana do używania jako stack w Portainerze.
- **Manifesty Kubernetes** – Konfiguracje dla środowisk Kubernetes.

## 🛠 Wymagania
Aby uruchomić projekt, wymagane są:
- **Docker** >= 20.10
- **Docker Compose** >= 2.0
- **Portainer** *(opcjonalnie, do zarządzania kontenerami)*

## 🚀 Instalacja i uruchomienie
1. **Sklonuj repozytorium:**
   ```sh
   git clone https://github.com/Sebastian-Koziatek/homelab.git
   cd homelab
   ```
2. **Uruchom usługi za pomocą Docker Compose:**
   ```sh
   docker-compose up -d
   ```
   Alternatywnie, jeśli korzystasz z Portainera, możesz dodać plik `docker-compose.yml` jako stack.

## 📁 Struktura katalogów
```
/homelab
├── Dockerfile
│   └── Zabbix
│       ├── Dockerfile
│       ├── zabbix_agentd.conf
│       └── zabbix_server.conf
├── README.md
├── docker-compose
│   ├── InfluxDB
│   │   └── docker-compose.yml
│   ├── Wekan
│   │   └── docker-compose.yml
│   ├── heimdall
│   │   └── docker-compose.yml
│   ├── hoeygain
│   │   └── docker-compose.yml
│   └── nginx-proxy-manager
│       └── docker-compose.yml
└── kubernetes
```

## 🔧 Konfiguracja
### Docker Compose
W katalogu `docker-compose/` znajdują się pliki `docker-compose.yml` dla poszczególnych usług, przygotowane do używania jako stack w Portainerze. Można w nich dostosować:
- Porty
- Ścieżki wolumenów
- Zmienne środowiskowe

### Kubernetes
Manifesty Kubernetes znajdują się w katalogu `kubernetes/` i mogą być używane do wdrażania usług w klastrze Kubernetes.

## 🛡 Bezpieczeństwo
- **.env** – Używaj pliku `.env` do przechowywania poufnych danych.
- **Zarządzanie dostępem** – Ogranicz dostęp do serwera i usług.

## 📜 Licencja
Projekt jest dostępny na licencji MIT. Możesz go dowolnie modyfikować i używać.

## 📞 Kontakt
Jeśli masz pytania lub sugestie, skontaktuj się poprzez **Issues** lub **Pull Requests** na GitHubie.

