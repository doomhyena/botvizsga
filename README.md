# TechNova Zrt. - Hálózati infrastruktúra és szerverrendszer

## A projektről

A projekt célja a TechNova Zrt. informatikai hálózati infrastruktúrájának megtervezése, kialakítása és dokumentálása volt. A megoldás egy strukturált, biztonságos és bővíthető vállalati hálózatot mutat be, amely több telephelyet, VLAN-okkal elkülönített hálózatokat, vezeték nélküli kapcsolatot és különböző szerveroldali szolgáltatásokat foglal magában.

A hálózati topológiát Cisco Packet Tracerben modelleztük, a Windows- és Linux-szervereket pedig Oracle VirtualBoxban telepítettük és konfiguráltuk.

## A projekt céljai

- Több telephelyet összekapcsoló hálózati infrastruktúra kialakítása.
- IPv4-címzési terv és alhálózatok megtervezése.
- VLAN-ok létrehozása a hálózati forgalom logikai elkülönítéséhez.
- Dinamikus és statikus forgalomirányítás konfigurálása.
- NAT/PAT, ACL és DHCP beállítása.
- Biztonságos távoli elérés megvalósítása SSH-val.
- Vezeték nélküli hálózat és IoT-eszközök konfigurálása.
- Windows Server- és Ubuntu Server-alapú szolgáltatások kialakítása.
- DNS, webkiszolgálás, adatbázis-kezelés, fájlmegosztás és mentési feladatok konfigurálása.

## Hálózati infrastruktúra

A megtervezett hálózat három telephelyet, valamint egy internetkapcsolatot modellező hálózati elemet tartalmaz.

| Hálózati elem | Feladat |
|---|---|
| Routerek | Alhálózatok összekapcsolása és forgalomirányítás |
| Switchek | Helyi hálózati eszközök összekötése |
| VLAN-ok | A hálózati forgalom logikai elkülönítése |
| WLAN | Vezeték nélküli eszközök csatlakoztatása |
| DHCP | IP-címek dinamikus kiosztása |
| NAT/PAT | Címfordítás a hálózatok közötti kommunikációhoz |
| ACL | A hálózati forgalom szabályozása |
| OSPF | Dinamikus útvonalválasztás |

### Alkalmazott technológiák

- **IPv4:** privát IP-címzés és alhálózatokra bontás.
- **VLAN:** Management, vállalati management, dolgozói és VoIP-hálózatok elkülönítése.
- **802.1Q trunk:** VLAN-forgalom továbbítása a megfelelő hálózati kapcsolatokon.
- **OSPF:** dinamikus forgalomirányítás.
- **NAT/PAT:** belső címek címfordítása.
- **SSH:** titkosított távoli adminisztráció.
- **Port security:** a switchportok biztonságának növelése.
- **WLAN:** vezeték nélküli hálózati hozzáférés.
- **DHCP:** automatikus hálózati konfiguráció a kliensek számára.

### IP-címzési terv

A belső hálózat alapja a `172.16.0.0/16` privát címtartomány. Ezt több kisebb alhálózatra bontottuk a telephelyek és a különböző hálózati szerepkörök igényeinek megfelelően.

| Hálózati szegmens | Hálózati cím |
|---|---|
| A épület – LAN1 | `172.16.0.0/26` |
| B épület – LAN2 | `172.16.0.64/27` |
| C épület – LAN3 | `172.16.0.160/29` |
| Management VLAN | `172.16.0.168/29` |
| VoIP VLAN | `172.16.0.176/29` |
| Vállalati management VLAN | `172.16.0.96/27` |
| Dolgozói VLAN | `172.16.0.128/27` |
| Vezeték nélküli hálózat | `192.168.10.0/24` |

## Virtualizáció és szerverek

A szerverkörnyezetet Oracle VirtualBox segítségével, két külön virtuális gépen alakítottuk ki. A virtuális gépek belső hálózati adaptere az `Internal Network` módot használja, `TechNova` hálózatnévvel.

### Windows Server 2022

**Gépnév:** `TECHNOVA-SRV`  
**IP-cím:** `172.16.0.165/29`  
**Alapértelmezett átjáró:** `172.16.0.161`  
**Tartomány:** `technova.local`

Megvalósított szolgáltatások:

- Active Directory Domain Services (AD DS)
- DNS-kiszolgáló
- DHCP-kiszolgáló
- IIS webkiszolgáló
- SMB-alapú fájlmegosztások
- Szervezeti egységek, csoportok és felhasználói fiókok
- Csoportházirendek (GPO)
- Windows tűzfalszabályok
- Ütemezett biztonsági mentés

Az Active Directory segítségével központilag kezelhetők a tartomány felhasználói, csoportjai és a hozzáférési jogosultságok.

### Ubuntu Server 24.04

**Gépnév:** `linux-srv`  
**IP-cím:** `172.16.0.166/29`  
**Alapértelmezett átjáró:** `172.16.0.161`

Megvalósított szolgáltatások:

- Apache2 webkiszolgáló
- MariaDB adatbázis-kiszolgáló
- BIND9 DNS-kiszolgáló
- UFW tűzfal
- Netplan hálózati konfiguráció
- Webmin adminisztrációs felület
- Bash-alapú automatizálás
- Ütemezett biztonsági mentés
- Szolgáltatásokat ellenőrző segédszkript

A Linux-szerveren egy belső weboldalt és egy `technova_db` nevű adatbázist alakítottunk ki. A DNS-szolgáltatás a Windows Server DNS-rendszerét kiegészítő névfeloldást biztosít.

## Biztonság és automatizálás

A hálózati és szerveroldali biztonság kialakításakor több megoldást alkalmaztunk:

- SSH-alapú, titkosított távoli adminisztráció.
- VLAN-okkal történő hálózati elkülönítés.
- ACL-szabályok és NAT/PAT konfiguráció.
- Windows tűzfalszabályok a szükséges szolgáltatásokhoz.
- UFW-tűzfal a Linux-szerveren.
- Active Directory-csoportok és jogosultságok.
- Csoportházirendek a központi beállításokhoz.
- Ütemezett szervermentések.
- Automatikus szolgáltatás-ellenőrzés Linuxon.

## Felhasznált technológiák és eszközök

| Technológia | Felhasználási terület |
|---|---|
| Cisco Packet Tracer | Hálózati topológia és konfiguráció szimulációja |
| Oracle VirtualBox | Virtuális szerverek futtatása |
| Windows Server 2022 | Tartománykezelés és vállalati szolgáltatások |
| Ubuntu Server 24.04 | Linux-alapú szerveroldali szolgáltatások |
| PowerShell | Windows Server konfigurálása és automatizálása |
| Bash | Linuxos adminisztrációs és mentési szkriptek |
| Active Directory | Központi felhasználó- és jogosultságkezelés |
| IIS és Apache2 | Webkiszolgálás |
| MariaDB | Relációs adatbázis-kezelés |
| BIND9 és DNS | Névfeloldás |
| DHCP | Dinamikus IP-címkiosztás |
| UFW és Windows Firewall | Hálózati forgalom szabályozása |

## Tesztelés és ellenőrzés

A projekt során a hálózati konfigurációkat és a szerveroldali szolgáltatásokat a megfelelő eszközökkel ellenőriztük.

A dokumentációban szereplő ellenőrzési területek:

- IP-címzés és hálózati kapcsolatok.
- DHCP-címkiosztás és bérlések.
- Dinamikus útvonalválasztás.
- DNS-névfeloldás.
- Weboldalak elérhetősége.
- Active Directory-tartomány működése.
- Fájlmegosztási jogosultságok.
- Tűzfalszabályok és szerveroldali szolgáltatások.
- Automatizált mentések és szolgáltatás-ellenőrzés.

## Dokumentáció

A részletes vizsgadokumentáció tartalmazza a hálózati topológia, az IPv4-címzési terv, a Packet Tracer-konfigurációk, a fizikai megvalósítás és a Windows- és Linux-szerverek beállításainak leírását.

## Készítők

A projektet három tanuló készítette az **Informatikai rendszer- és alkalmazás-üzemeltető technikus** képzés keretében:

- Csontos Kincső Anasztázia
- Faragó Tamás Gyula
- Szabó Dávid

## A projekt eredménye

A projekt során egy több telephelyet modellező vállalati hálózati infrastruktúrát és egy kapcsolódó szerverkörnyezetet terveztünk meg és dokumentáltunk. A megoldás a hálózatépítési, címzési, forgalomirányítási, virtualizációs, szerverüzemeltetési és informatikai biztonsági ismeretek gyakorlati alkalmazását mutatja be.

**Projekt típusa:** Vizsgaremek / hálózati infrastruktúra és szerverüzemeltetés  
**Fókusz:** Hálózatépítés, virtualizáció, Windows Server, Linux Server és IT-biztonság
