# Thermal Zone Killed

*by Hinomori Project*

Module Magisk / KernelSU / KernelSU Next (KSUN) untuk menonaktifkan service
thermal HAL dan mem-spoof nilai suhu di `/sys/class/thermal/thermal_zoneX/temp`
lewat bind-mount.

## Kompatibilitas

- Magisk v20.4+
- KernelSU
- KernelSU Next (KSUN)

Deteksi root manager otomatis lewat variabel `$KSU` / `$APATCH` yang
disediakan saat proses install (`customize.sh`).

## Struktur repo

```
.
├── module.prop              # metadata module
├── customize.sh             # dijalankan sekali saat install
├── post-fs-data.sh          # early boot: stop service + set prop
├── service.sh                # late boot: bind-mount spoof suhu + loop stop
├── uninstall.sh              # cleanup saat module dihapus
├── common/
│   └── config.prop           # template config default
└── META-INF/
    └── com/google/android/
        ├── update-binary      # wrapper flash standar Magisk
        └── updater-script
```

## Instalasi

1. Zip seluruh isi folder ini (bukan foldernya, isinya) jadi satu file `.zip`.
2. Flash lewat Magisk Manager / KernelSU Manager / KSUN Manager → *Install from
   storage*.
3. Reboot.
4. Edit `/data/adb/thermal_spoof/config.prop` sesuai kebutuhan device, lalu
   reboot lagi.

## Konfigurasi (`/data/adb/thermal_spoof/config.prop`)

| Key | Default | Keterangan |
|---|---|---|
| `ENABLE_SPOOF` | `1` | Aktifkan bind-mount suhu palsu |
| `ENABLE_SERVICE_STOP` | `1` | Aktifkan stop service thermal HAL |
| `SPOOF_TEMP` | `38000` | Suhu palsu, satuan millidegree Celsius (38000 = 38.0°C) |
| `THERMAL_SERVICES` | lihat file | Daftar nama init service yang dicoba di-stop, pisah koma |
| `EXTRA_PROPS` | lihat file | Prop tambahan `key=value` yang dipaksa set, pisah koma |

## Catatan penting

- Nama service thermal HAL **berbeda-beda tiap vendor/SoC**. Untuk device
  MediaTek (mis. Helio G99), cek `vendor/etc/init/*.rc` di source device tree
  kamu, atau jalankan `ps -A | grep thermal` di device, untuk memastikan nama
  service yang benar sebelum diedit ke `THERMAL_SERVICES`.
- Suhu yang di-spoof bersifat **statis** (tidak berfluktuasi). App yang
  mengharapkan pembacaan suhu naik-turun wajar bisa mencurigai nilai ini.
- Spoof ini menutupi pembacaan lewat sysfs; kalau ada mekanisme throttling
  yang membaca sumber lain (langsung dari driver kernel), itu tidak
  terpengaruh oleh module ini.

## Lisensi

MIT — lihat [LICENSE](LICENSE).
# Thermal-Zone-Spoof-to--1-
