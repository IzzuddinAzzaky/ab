#!/bin/bash

# Tentukan folder utama tujuan untuk menyimpan file ZIP berdasarkan tanggal hari ini
tanggal_waktu=$(date +\%Y-\%m-\%d)
tujuan_utama="/E/backup/$tanggal_waktu"

# Pastikan folder tujuan utama ada, atau buat jika belum ada
if [ ! -d "$tujuan_utama" ]; then
    mkdir -p "$tujuan_utama"
fi

# Loop untuk setiap folder dalam struktur direktori /C/laragon/www
find /C/laragon/www -type d | while read -r sumber_folder; do
    # Periksa apakah folder ini mengandung subfolder "views"
    if [ -d "$sumber_folder/views" ]; then
        # Mendapatkan dua nama path terakhir dari path folder sumber
        folder1=$(basename "$sumber_folder")
        folder2="views"
        
        # Tentukan nama file ZIP dengan format nama sesuai dengan yang Anda inginkan dan menyimpannya di folder tujuan yang sesuai dengan tanggal hari ini di /E/backup
        jam_waktu=$(date +\%H\%M\%S)
        nama_zip="$tujuan_utama/$folder1\_$folder2\_$jam_waktu.zip"
        
        # Buat file ZIP dari folder sumber
        zip -r "$nama_zip" "$sumber_folder/views"
    fi
done
