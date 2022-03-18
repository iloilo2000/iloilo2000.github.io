# Intró

Powershell van mögötte
Nem jó tömeges szerver módosításra (arra SCCM kell)
Csak Chrome alapú böngésző jó, (Firefox sem!!)

Még nem váltja ki a szerver managert és az mmc konzolokat, rdp-re sem az igazi.

Jelenleg csak egy alternatíva amivel kezdeni kell megismerkedni

# Telepítés

- WinRM over HTTPS biztonságosabb, de bonyolult lehet későbbb

# Szekuriti

- Gateway adminoknak és GW usereknek AD csoportot létrehozni
- Nekünk a "No internet" kell
- (Per server administration: JEA - Just Enough Admin, DSC - Desired State Config)

# Managing servers

- Ha mindenki számára elérhető szervert akarunk felvenni, akkor a "Shared Connections" kell

![image](https://user-images.githubusercontent.com/57397545/159094021-4a970253-50e1-4d4a-afe3-04f75d35e0ab.png)

- Contrained Delegation kell az SSO-hoz (utánanézni!!!)
- LAPS-al is használható???
- Szerver role-oknak megfelelő extension-öket hozzá kell adni, nem adjs hozzá automatikusan
- Disk monitoring nem működik amíg be nem kapcsoljuk a WAC-ból a szerveren (enable disk metrics --> too much resources to run)
- "dynamic disk" már nem támogatott

# Storage replica / migration

- DR esetekre jó, de Backup is kell mellé
- Csak 2019 Datacenter Edition-ön
- Management csak Powershell vagy WAC-al

- Kell egy SMS (storage migration service) Orchestrator

# System insights

- Erőforrás előrejelzés
- Telepíthető server role-ként

