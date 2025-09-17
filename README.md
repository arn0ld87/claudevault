# Here are your Instructions
# CloudVault - Private Cloud Storage

Eine sichere, selbst gehostete Cloud-Speicher-Lösung als Alternative zu Google Drive oder Dropbox.

![CloudVault Logo](https://via.placeholder.com/200x80/2563eb/ffffff?text=CloudVault)

## 🌟 Features

### 🔐 Sichere Authentifizierung
- Benutzerregistrierung mit E-Mail-Validierung
- JWT-basierte Anmeldung mit bcrypt-Passwort-Hashing
- Admin-Genehmigungsworkflow für neue Benutzer
- Sichere Session-Verwaltung

### 📁 Vollständige Dateiverwaltung
- Datei-Upload bis zu 5GB pro Datei
- Download mit sicherer Zugriffskontrolle
- Ordner erstellen, navigieren und löschen
- Benutzer-Isolation - Benutzer sehen nur ihre eigenen Dateien
- Breadcrumb-Navigation und intuitive Dateiverwaltung

### 💾 Speichermanagement
- 50GB Speicherplatz pro Benutzer
- Echtzeit-Speichernutzung-Tracking
- Fortschrittsanzeigen für Speicherplatz-Auslastung

### ⚙️ Admin-Panel
- Umfassendes Benutzermanagement-Dashboard
- Benutzerkonten aktivieren/deaktivieren
- Benutzerstatistiken anzeigen
- Speichernutzung aller Benutzer überwachen

## 🚀 Installation & Setup

### Voraussetzungen
- Ubuntu Server
- Docker oder Python 3.11+
- Node.js 18+
- MongoDB

### Schnellstart

1. **Repository klonen:**
```bash
git clone <repository-url>
cd cloudvault
```

2. **Backend Setup:**
```bash
cd backend
pip install -r requirements.txt
```

3. **Frontend Setup:**
```bash
cd frontend
yarn install
```

4. **Umgebungsvariablen konfigurieren:**
```bash
# Backend/.env
MONGO_URL="mongodb://localhost:27017"
DB_NAME="cloudvault_db"
SECRET_KEY="your-secret-key-here"
CORS_ORIGINS="*"

# Frontend/.env
REACT_APP_BACKEND_URL=http://localhost:8001
```

5. **Speicherverzeichnis erstellen:**
```bash
sudo mkdir -p /home/admin/cloud
sudo chmod 755 /home/admin/cloud
```

6. **Services starten:**
```bash
# Mit Supervisor (empfohlen)
sudo supervisorctl restart all

# Oder manuell:
# Backend
cd backend && uvicorn server:app --host 0.0.0.0 --port 8001

# Frontend
cd frontend && yarn start
```

## 📋 Standardzugangsdaten

### Administrator
- **Benutzername:** `admin`
- **Passwort:** `admin123`

*Bitte ändern Sie diese Zugangsdaten nach dem ersten Login!*

## 🔧 Konfiguration

### Speicher-Limits anpassen
```python
# In backend/server.py
MAX_FILE_SIZE = 5 * 1024 * 1024 * 1024  # 5GB pro Datei
USER_QUOTA = 50 * 1024 * 1024 * 1024    # 50GB pro Benutzer
```

### Speicherort ändern
```python
# In backend/server.py
CLOUD_STORAGE_PATH = Path("/home/admin/cloud")  # Pfad anpassen
```

## 📊 API-Dokumentation

### Authentifizierung
```bash
# Benutzer registrieren
POST /api/auth/register
{
  "username": "benutzer",
  "email": "benutzer@example.com", 
  "password": "passwort",
  "full_name": "Vollständiger Name"
}

# Anmelden
POST /api/auth/login
{
  "username": "benutzer",
  "password": "passwort"
}
```

### Dateiverwaltung
```bash
# Dateien auflisten
GET /api/files?path=ordner/pfad

# Datei hochladen
POST /api/files/upload
Content-Type: multipart/form-data

# Datei herunterladen
GET /api/files/download?file_path=pfad/zur/datei

# Ordner erstellen
POST /api/files/create-folder
{
  "folder_name": "neuer_ordner",
  "current_path": "aktueller/pfad"
}

# Datei/Ordner löschen
DELETE /api/files/delete?file_path=pfad/zur/datei
```

### Admin-Funktionen
```bash
# Alle Benutzer auflisten
GET /api/admin/users

# Benutzer aktivieren
POST /api/admin/users/{username}/activate

# Benutzer deaktivieren
POST /api/admin/users/{username}/deactivate
```

## 🛡️ Sicherheitsfeatures

- **JWT-Token-Authentifizierung** mit automatischem Ablauf
- **bcrypt-Passwort-Hashing** für sichere Speicherung
- **Pfad-Traversal-Schutz** gegen Angriffe
- **Benutzer-Isolation** - komplette Datentrennung
- **Sichere Dateizugriffskontrolle**
- **Admin-Genehmigungsworkflow** für neue Benutzer

## 🔄 Backup & Wartung

### Daten sichern
```bash
# Benutzerdateien
tar -czf backup_files.tar.gz /home/admin/cloud/

# MongoDB Datenbank
mongodump --db cloudvault_db --out /backup/mongodb/
```

### Logs überwachen
```bash
# Backend-Logs
tail -f /var/log/supervisor/backend.*.log

# Frontend-Logs
tail -f /var/log/supervisor/frontend.*.log
```

## 🐛 Troubleshooting

### Häufige Probleme

**Problem:** Backend startet nicht
```bash
# Logs prüfen
tail -n 50 /var/log/supervisor/backend.err.log

# Dependencies installieren
cd backend && pip install -r requirements.txt
```

**Problem:** Dateien können nicht hochgeladen werden
- Prüfen Sie die Speicherberechtigungen: `chmod 755 /home/admin/cloud`
- Prüfen Sie die Speicherplatz-Limits in der Konfiguration

**Problem:** Frontend kann Backend nicht erreichen
- Prüfen Sie die REACT_APP_BACKEND_URL in frontend/.env
- Stellen Sie sicher, dass CORS richtig konfiguriert ist

## 📁 Projektstruktur

```
cloudvault/
├── backend/
│   ├── server.py           # FastAPI-Anwendung
│   ├── requirements.txt    # Python-Dependencies
│   └── .env               # Backend-Konfiguration
├── frontend/
│   ├── src/
│   │   ├── components/    # React-Komponenten
│   │   ├── App.js        # Haupt-App-Komponente
│   │   └── App.css       # Styling
│   ├── package.json      # Node.js-Dependencies
│   └── .env             # Frontend-Konfiguration
└── README.md            # Diese Datei
```

## 🤝 Beitragen

1. Fork des Repositories erstellen
2. Feature-Branch erstellen (`git checkout -b feature/neue-funktion`)
3. Änderungen committen (`git commit -am 'Neue Funktion hinzufügen'`)
4. Branch pushen (`git push origin feature/neue-funktion`)
5. Pull Request erstellen

## 📝 Lizenz

Dieses Projekt steht unter der MIT-Lizenz. Siehe [LICENSE](LICENSE) für Details.

## 🙏 Danksagung

- [FastAPI](https://fastapi.tiangolo.com/) für das exzellente Backend-Framework
- [React](https://reactjs.org/) für die Frontend-Entwicklung
- [Shadcn/ui](https://ui.shadcn.com/) für die UI-Komponenten
- [MongoDB](https://www.mongodb.com/) für die Datenbankunterstützung

## 📞 Support

Bei Fragen oder Problemen:
- Erstellen Sie ein Issue im Repository
- Kontaktieren Sie das Entwicklungsteam
- Prüfen Sie die Dokumentation und Troubleshooting-Sektion

---

**CloudVault** - Ihre sichere, private Cloud-Speicher-Lösung 🔒☁️
