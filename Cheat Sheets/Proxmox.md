
-----

# Proxmox VE - Cheat Sheet 🚀

## Zarządzanie Maszynami Wirtualnymi (VM) - `qm`

| Polecenie | Opis |
| :--- | :--- |
| `qm list` | Wyświetla listę wszystkich maszyn wirtualnych i ich status. |
| `qm start <VMID>` | Uruchamia maszynę wirtualną o podanym ID. |
| `qm stop <VMID>` | Zatrzymuje maszynę wirtualną (twarde wyłączenie). |
| `qm shutdown <VMID>` | Bezpiecznie zamyka system operacyjny w maszynie wirtualnej. |
| `qm reset <VMID>` | Resetuje maszynę wirtualną. |
| `qm destroy <VMID>` | Usuwa maszynę wirtualną. **Uwaga: operacja nieodwracalna\!** |
| `qm suspend <VMID>` | Zawiesza maszynę wirtualną, zapisując jej stan w pamięci RAM. |
| `qm resume <VMID>` | Wznawia działanie zawieszonej maszyny wirtualnej. |
| `qm clone <VMID> <NOWE_VMID> --name <NAZWA>` | Klonuje maszynę wirtualną. |
| `qm config <VMID>` | Wyświetla konfigurację maszyny wirtualnej. |
| `qm set <VMID> [opcje]` | Modyfikuje konfigurację maszyny wirtualnej (np. `qm set 100 --memory 2048`). |
| `qm resize <VMID> <DYSK> <ROZMIAR>` | Zmienia rozmiar dysku (np. `qm resize 100 scsi0 +10G`). |
| `qm snapshot <VMID> <NAZWA_MIGAWKI>` | Tworzy migawkę (snapshot) maszyny wirtualnej. |
| `qm rollback <VMID> <NAZWA_MIGAWKI>` | Przywraca maszynę wirtualną do stanu z migawki. |
| `qm terminal <VMID>` | Otwiera konsolę szeregową do maszyny wirtualnej. |

-----

## Zarządzanie Kontenerami (LXC) - `pct`

| Polecenie | Opis |
| :--- | :--- |
| `pct list` | Wyświetla listę wszystkich kontenerów. |
| `pct start <CTID>` | Uruchamia kontener o podanym ID. |
| `pct stop <CTID>` | Zatrzymuje kontener (twarde wyłączenie). |
| `pct shutdown <CTID>` | Bezpiecznie zamyka kontener. |
| `pct destroy <CTID>` | Usuwa kontener. **Uwaga: operacja nieodwracalna\!** |
| `pct enter <CTID>` | Otwiera powłokę (shell) wewnątrz działającego kontenera. |
| `pct mount <CTID>` | Montuje system plików kontenera. |
| `pct unmount <CTID>` | Odmontowuje system plików kontenera. |
| `pct config <CTID>` | Wyświetla konfigurację kontenera. |
| `pct set <CTID> [opcje]` | Modyfikuje konfigurację kontenera (np. `pct set 101 --memory 1024`). |
| `pct resize <CTID> <DYSK> <ROZMIAR>` | Zmienia rozmiar dysku kontenera (np. `pct resize 101 rootfs +5G`). |
| `pct snapshot <CTID> <NAZWA_MIGAWKI>` | Tworzy migawkę kontenera. |
| `pct rollback <CTID> <NAZWA_MIGAWKI>` | Przywraca kontener do stanu z migawki. |

-----

## Zarządzanie Klastrem - `pvecm`

| Polecenie | Opis |
| :--- | :--- |
| `pvecm status` | Wyświetla status klastra i jego członków. |
| `pvecm nodes` | Wyświetla listę węzłów w klastrze. |
| `pvecm create <NAZWA_KLASTRA>` | Tworzy nowy klaster na aktualnym węźle. |
| `pvecm add <ADRES_IP_CZŁONKA_KLASTRA>` | Dodaje bieżący węzeł do istniejącego klastra. |
| `pvecm delnode <NAZWA_WĘZŁA>` | Usuwa węzeł z klastra. |
| `pvecm expected 1` | Ustawia oczekiwaną liczbę głosów na 1 (przydatne przy awarii węzłów). |

-----

## Zarządzanie Pamięcią Masową - `pvesm`

| Polecenie | Opis |
| :--- | :--- |
| `pvesm status` | Wyświetla status wszystkich skonfigurowanych pamięci masowych. |
| `pvesm list <STORAGE_ID>` | Wyświetla zawartość danej pamięci masowej. |
| `pvesm add <TYP> <STORAGE_ID> [opcje]` | Dodaje nową pamięć masową (np. `pvesm add dir local-backup --path /mnt/backup`). |
| `pvesm remove <STORAGE_ID>` | Usuwa konfigurację pamięci masowej. |
| `pvesm alloc <STORAGE_ID> <VMID> <NAZWA_PLIKU> <ROZMIAR>` | Alokuje nowy wolumin na pamięci masowej. |
| `pvesm free <WOLUMIN_ID>` | Usuwa wolumin (np. `pvesm free local-lvm:vm-100-disk-0`). |

-----

## Kopie Zapasowe i Przywracanie - `vzdump` / `qmrestore`

| Polecenie | Opis |
| :--- | :--- |
| `vzdump <VMID>` | Tworzy kopię zapasową maszyny wirtualnej lub kontenera. |
| `vzdump <VMID> --mode snapshot` | Tworzy kopię zapasową na żywo (snapshot). |
| `vzdump <VMID> --compress zstd` | Tworzy kopię zapasową z kompresją Zstandard. |
| `vzdump --all --mailtome` | Tworzy kopię zapasową wszystkich VM i kontenerów z powiadomieniem email. |
| `qmrestore <PLIK_KOPII> <NOWE_VMID>` | Przywraca maszynę wirtualną z kopii zapasowej. |
| `pct restore <PLIK_KOPII> <NOWE_CTID>` | Przywraca kontener z kopii zapasowej. |

-----

## Zarządzanie Siecią 🌐

Konfiguracja sieci w Proxmoxie odbywa się głównie poprzez edycję pliku `/etc/network/interfaces`. Po zmianach należy przeładować konfigurację sieci.

  * **Edycja pliku:** `nano /etc/network/interfaces`
  * **Zastosowanie zmian:** `ifreload -a`

**Przykładowa konfiguracja mostka (bridge):**

```bash
auto vmbr0
iface vmbr0 inet static
    address 192.168.1.10/24
    gateway 192.168.1.1
    bridge-ports eno1
    bridge-stp off
    bridge-fd 0
```

-----

## Ogólne Polecenia Systemowe

| Polecenie | Opis |
| :--- | :--- |
| `pveversion -v` | Wyświetla szczegółowe informacje o wersji Proxmox VE i pakietów. |
| `pvereport` | Generuje szczegółowy raport o stanie systemu i konfiguracji. |
| `pve-firewall status` | Wyświetla status wbudowanego firewalla Proxmox. |
| `pveum` | Zarządza użytkownikami i uprawnieniami (np. `pveum useradd`). |
| `ha-manager status` | Wyświetla status menedżera wysokiej dostępności (High Availability). |
| `apt update && apt dist-upgrade` | Aktualizuje system i pakiety Proxmox. |