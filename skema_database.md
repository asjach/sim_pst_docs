Project SIM_Pesantren_DBML {
  database_type: "MySQL"
  Note: "Skema utama SIM Pesantren dalam satu database"
}

Table users {
  id bigint [pk, increment]
  name varchar [not null]
  email varchar [not null, unique]
  email_verified_at timestamp
  password varchar [not null]
  is_active boolean [not null, default: true]
  remember_token varchar
}

Table roles {
  id bigint [pk, increment]
  name varchar [not null]
  slug varchar [not null, unique]
  description varchar
}

Table role_user {
  id bigint [pk, increment]
  user_id bigint [not null]
  role_id bigint [not null]
}

Table password_reset_tokens {
  email varchar [pk]
  token varchar [not null]
  created_at timestamp
}

Table sessions {
  id varchar [pk]
  user_id bigint
  ip_address varchar(45)
  user_agent text
  payload longtext [not null]
  last_activity int [not null]
}

// ===========
// MASTER DATA
// ===========


// MASTER LEMBAGA
Table lembaga {
  id varchar [pk, note: 'PK string mengikuti skema existing']
  nama varchar [not null]
  nsm varchar
  npsn varchar
  jenjang_id bigint
}


// SANTRI
Table santri {
  id bigint [pk, increment]
  nama_lengkap varchar [not null]
  nama_singkat varcchar
  nik varchar
  nisn varchar
  tmp_lahir varchar
  tgl_lahir date
  jk varchar

  anak_ke varchar
  j_saudara varchar
  agama varchar
  cita_cita varchar
  hobi varchar
  yang_membiayai varchar
  kebutuhan_khusus varchar [default:'tidak ada']
  nomor_kip varchar

  ayah_nama varchar
  ayah_status bigint
  ayah_nik varchar
  ayah_tmp_lahir varchar
  ayah_tgl_lahir date
  ayah_pekerjaan varchar
  ayah_pendidikan varchar
  ayah_penghasilan varchar
  ayah_telp varchar

  ibu_nama varchar
  ibu_status varchar
  ibu_nik varchar
  ibu_tmp_lahir varchar
  ibu_tgl_lahir date
  ibu_pekerjaan varchar
  ibu_pendidikan varchar
  ibu_penghasilan varchar
  ibu_telp varchar

  wali_status varchar
  wali_nama varchar
  wali_nik varchar
  wali_tmp_lahir varchar
  wali_tgl_lahir varchar
  wali_pekerjaan varchar
  wali_pendidikan varchar
  wali_penghasilan varchar
  wali_telp varchar

  provinsi varchar
  kab_kota varchar
  kecamatan varchar
  kel_desa varchar
  rw varchar
  rt varchar
  kodepos varchar
  alamat varchar
  alamat_full text
  jarak_id bigint
  waktu_id bigint
  transport_id bigint
  aktif boolean [not null, default: true]
}


// MASTER PEGAWAI
Table pegawai {
  id bigint [pk, increment]
  user_id bigint [unique, note: 'Nullable jika pegawai punya akun login']
  kode_pegawai varchar [unique]
  nama_lengkap varchar [not null]
  nama_panggilan varchar
  nik varchar
  nip varchar
  nuptk varchar
  npwp varchar
  jk varchar
  tmp_lahir varchar
  tgl_lahir date
  agama varchar
  status_pernikahan varchar
  telp varchar
  email_pribadi varchar
  foto varchar
  alamat text
  npa varchar
  niat varchar
  nrg varchar
  status_aktif boolean [not null, default: true]
  tgl_masuk_pertama date
  tgl_keluar_terakhir date
  catatan text
}







// ===========
// TABEL LOOKUP
// ===========

Table jarak {
  id bigint [pk, increment]
  kode varchar
  nama varchar [not null]
}

Table waktu {
  id bigint [pk, increment]
  kode varchar
  nama varchar [not null]
}

Table transport {
  id bigint [pk, increment]
  kode varchar
  nama varchar [not null]
}

Table kategori_alamat {
  id bigint [pk, increment]
  kode varchar
  nama varchar [not null]
}

Table pendidikan {
  id bigint [pk, increment]
  kode varchar
  nama varchar [not null]
  urutan int [not null, default: 0]
}

Table pekerjaan {
  id bigint [pk, increment]
  kode varchar
  nama varchar [not null]
  urutan int [not null, default: 0]
}


// lookup akademik

Table jenjang {
  id bigint [pk, increment]
  kode varchar
  nama varchar [not null]
}

Table tahun_ajaran {
  id bigint [pk, increment]
  nama varchar [not null]
  status int [not null, default: 0]
}

Table semester {
  id bigint [pk, increment]
  kode varchar
  nama varchar [not null]
  status int [not null, default: 0]
}

Table tingkat {
  id bigint [pk, increment]
  nama varchar [not null]
  urutan int [not null, default: 0]
  jenjang_id bigint
}

Table status_kelas_santri {
  id bigint [pk, increment]
  kode varchar
  nama varchar [not null]
}

Table jenis_mutasi {
  id bigint [pk, increment]
  kode varchar
  nama varchar [not null]
}

Table status_kelulusan {
  id bigint [pk, increment]
  kode varchar
  nama varchar [not null]
}


// lookup dokumen
Table jenis_dokumen_santri {
  id bigint [pk, increment]
  nama varchar [not null]
  keterangan varchar
  urutan int [not null, default: 0]
}

Table status_dokumen {
  id bigint [pk, increment]
  kode varchar
  nama varchar [not null]
}

// LOOKUP PSB

Table jalur_psb {
  id bigint [pk, increment]
  kode varchar
  nama varchar [not null]
}

Table status_pendaftaran {
  id bigint [pk, increment]
  kode varchar
  nama varchar [not null]
}

Table predikat {
  kode varchar [pk]
  keterangan varchar
}











// Table unit_kerja {
//   id bigint [pk, increment]
//   lembaga_id varchar [not null]
//   kode varchar
//   nama varchar [not null]
//   parent_unit_id bigint
//   penanggung_jawab_pegawai_id bigint
//   urutan int [not null, default: 0]
//   status_aktif boolean [not null, default: true]
//   created_at datetime
//   updated_at datetime

//   Indexes {
//     lembaga_id
//     parent_unit_id
//     penanggung_jawab_pegawai_id
//     (lembaga_id, nama)
//   }
// }

// Table jabatan {
//   id bigint [pk, increment]
//   lembaga_id varchar [note: 'Nullable jika jabatan berlaku global lintas lembaga']
//   kode varchar
//   nama varchar [not null]
//   kategori varchar [note: 'Contoh: struktural, fungsional, pengajar, staf']
//   level_jabatan varchar
//   status_aktif boolean [not null, default: true]
//   created_at datetime
//   updated_at datetime

//   Indexes {
//     lembaga_id
//     (lembaga_id, nama)
//   }
// }

// Table penugasan_pegawai {
//   id bigint [pk, increment]
//   pegawai_id bigint [not null]
//   lembaga_id varchar [not null]
//   unit_kerja_id bigint
//   jabatan_id bigint
//   status_pegawai varchar [not null, note: 'Contoh: tetap, kontrak, honorer, magang']
//   jenis_ptk varchar [note: 'Contoh: guru, tendik, pengasuh, admin']
//   nomor_sk varchar
//   tgl_mulai date [not null]
//   tgl_selesai date
//   is_homebase boolean [not null, default: false]
//   is_aktif boolean [not null, default: true]
//   catatan text
//   created_at datetime
//   updated_at datetime

//   Indexes {
//     pegawai_id
//     lembaga_id
//     unit_kerja_id
//     jabatan_id
//     is_aktif
//     (pegawai_id, lembaga_id, tgl_mulai)
//   }
// }

// Table kontrak_pegawai {
//   id bigint [pk, increment]
//   pegawai_id bigint [not null]
//   nomor_kontrak varchar
//   jenis_kontrak varchar [not null, note: 'Contoh: PKWT, PKWTT, tugas_tambahan']
//   tgl_mulai date [not null]
//   tgl_selesai date
//   gaji_pokok decimal(14,2)
//   status_kontrak varchar [not null, default: 'draft']
//   file_kontrak varchar
//   catatan text
//   created_at datetime
//   updated_at datetime

//   Indexes {
//     pegawai_id
//     status_kontrak
//     (pegawai_id, tgl_mulai)
//   }
// }

Table riwayat_pendidikan_pegawai {
  id bigint [pk, increment]
  pegawai_id bigint [not null]
  jenjang_pendidikan varchar [not null]
  nama_institusi varchar [not null]
  jurusan varchar
  tahun_masuk int
  tahun_lulus int
  no_ijazah varchar
  is_pendidikan_terakhir boolean [not null, default: false]
  keterangan text
}

Table keluarga_pegawai {
  id bigint [pk, increment]
  pegawai_id bigint [not null]
  nama varchar [not null]
  hubungan_keluarga varchar [not null, note: 'Contoh: pasangan, anak, ayah, ibu']
  jk varchar
  tgl_lahir date
  tanggungan boolean [not null, default: false]
  telp varchar
  keterangan text
}

Table rekening_pegawai {
  id bigint [pk, increment]
  pegawai_id bigint [not null]
  nama_bank varchar [not null]
  nama_pemilik_rekening varchar [not null]
  nomor_rekening varchar [not null]
  is_default boolean [not null, default: false]
  status_aktif boolean [not null, default: true]
}

Table dokumen_pegawai {
  id bigint [pk, increment]
  pegawai_id bigint [not null]
  jenis_dokumen varchar [not null, note: 'Contoh: ktp, kk, ijazah, sertifikat, kontrak, sk']
  nomor_dokumen varchar
  nama_file varchar
  file_path varchar
  tgl_terbit date
  tgl_berakhir date
  status_dokumen varchar [not null, default: 'valid']
  keterangan text
}

// Table mutasi_pegawai {
//   id bigint [pk, increment]
//   penugasan_pegawai_id bigint [not null]
//   jenis_mutasi varchar [not null, note: 'Contoh: antar_lembaga, antar_unit, perubahan_jabatan']
//   tgl_mutasi date [not null]
//   lembaga_tujuan_id varchar
//   unit_kerja_tujuan_id bigint
//   jabatan_tujuan_id bigint
//   nomor_sk varchar
//   alasan text
//   keterangan text
//   created_at datetime
//   updated_at datetime

//   Indexes {
//     penugasan_pegawai_id
//     tgl_mutasi
//     lembaga_tujuan_id
//     unit_kerja_tujuan_id
//     jabatan_tujuan_id
//   }
// }

// Table terminasi_pegawai {
//   id bigint [pk, increment]
//   pegawai_id bigint [not null]
//   jenis_terminasi varchar [not null, note: 'Contoh: resign, pensiun, wafat, diberhentikan']
//   tgl_terminasi date [not null]
//   nomor_sk varchar
//   alasan text
//   catatan text
//   created_at datetime
//   updated_at datetime

//   Indexes {
//     pegawai_id
//     tgl_terminasi
//     jenis_terminasi
//   }
// }

// Table presensi_pegawai {
//   id bigint [pk, increment]
//   penugasan_pegawai_id bigint [not null]
//   tanggal date [not null]
//   jam_masuk datetime
//   jam_pulang datetime
//   status_presensi varchar [not null, note: 'Contoh: hadir, izin, sakit, alpha, dinas_luar']
//   sumber_presensi varchar [note: 'Contoh: manual, fingerprint, mobile']
//   catatan text
//   created_at datetime
//   updated_at datetime

//   Indexes {
//     penugasan_pegawai_id
//     tanggal
//     status_presensi
//     (penugasan_pegawai_id, tanggal) [unique]
//   }
// }






// ===========
// AKADEMIK
// ===========

Table kelas {
  id bigint [pk, increment]
  nama varchar [not null]
  alias varchar
  urutan varchar
  lembaga_id varchar [not null]
  tahun_ajaran_id bigint
  tingkat_id bigint
}

// Table wali_kelas {
//   id bigint [pk, increment]
//   kelas_id bigint [not null]
//   semester_id bigint
//   pegawai_id bigint
// }





Table proses_psb {
  id bigint [pk, increment]
  nama varchar [not null]
  is_active boolean [not null, default: false]
  lembaga_id varchar [not null]
  tahun_ajaran_id bigint
  jalur_id bigint
}

Table gelombang_psb {
  id bigint [pk, increment]
  nama varchar [not null]
  proses_psb_id bigint [not null]
  tgl_mulai date
  tgl_akhir date
}



Table pendaftaran {
  id bigint [pk, increment]
  no_pendaftaran varchar
  lembaga_id varchar [not null]
  gelombang_id bigint
  santri_id bigint [not null]
  tgl_daftar date
  nama_sekolah_asal varchar
  npsn_sekolah_asal varchar
  nsm_sekolah_asal varchar
  alamat_sekolah_asal text
  status_pendaftaran_id bigint
}

Table log_status_pendaftaran {
  id bigint [pk, increment]
  pendaftaran_id bigint [not null]
  status_pendaftaran_id bigint
  tgl_perubahan date
  catatan varchar
}

Table jenis_tes {
  id bigint [pk, increment]
  kode varchar
  nama varchar [not null]
}

Table tes_psb {
  id bigint [pk, increment]
  proses_psb_id bigint [not null]
  jenis_tes_id bigint
  nama varchar [not null]
}


Table nilai_tes {
  id bigint [pk, increment]
  pendaftaran_id bigint [not null]
  tes_psb_id bigint [not null]
  nilai decimal(8,2)
  predikat varchar
  catatan varchar
}

Table status_lembaga_santri {
  id bigint [pk, increment]
  kode varchar
  nama varchar [not null]
}

Table log_lembaga_santri {
  id bigint [pk, increment]
  lembaga_id varchar [not null]
  santri_id bigint [not null]
  pendaftaran_id bigint [not null]
  nis_lokal varchar
  nis_kemenag varchar
  tgl_mulai date
  tgl_keluar date
  catatan varchar
  status_lembaga_santri_id bigint
}


Table kelas_santri {
  id bigint [pk, increment]
  kelas_id bigint [not null]
  santri_id bigint [not null]
  status_awal_kelas bigint
  status_akhir_kelas bigint
  tgl_mulai date
  tgl_selesai date
}



Table mutasi_keluar_santri {
  id bigint [pk, increment]
  santri_id bigint [not null]
  kelas_id bigint
  lembaga_id varchar [not null]
  tahun_ajaran_id bigint
  jenis_mutasi_id bigint
  tgl_mutasi date
  sekolah_tujuan varchar
  npsn_sekolah_tujuan varchar
  nsm_sekolah_tujuan varchar
  alamat_sekolah_tujuan text
  alasan_id bigint
}

Table kelulusan_santri {
  id bigint [pk, increment]
  santri_id bigint [not null]
  lembaga_id varchar [not null]
  tahun_ajaran_id bigint
  nomor_ijazah varchar
  nomor_peserta varchar
  tgl_kelulusan date
  status_kelulusan_id bigint
  catatan text
}

Table dokumen_santri {
  id bigint [pk, increment]
  santri_id bigint [not null]
  nama_file varchar
  jenis_dokumen_id bigint
  status_dokumen_id bigint
  keterangan varchar
  status boolean [not null, default: true]
}




// ===========
// KEUANGAN
// ===========

Table kas {
  id bigint [pk, increment]
  lembaga_id varchar [not null]
  nama varchar [not null]
  status boolean [not null, default: true]
  keterangan varchar
}

Table metode_pembayaran {
  id bigint [pk, increment]
  nama varchar [not null]
  kas_id bigint
  keterangan varchar
  status boolean [not null, default: true]
}

Table jenis_pos {
  id bigint [pk, increment]
  kode varchar
  nama varchar [not null]
}

Table pos_keuangan {
  id bigint [pk, increment]
  kode varchar
  jenis_pos_id bigint
  nama varchar [not null]
  keterangan varchar
  status boolean [not null, default: true]
}

Table biaya {
  id bigint [pk, increment]
  lembaga_id varchar [not null]
  tahun_ajaran_id bigint
  pos_keuangan_id bigint
  nama varchar [not null]
  is_bulanan boolean [not null, default: false]
  is_tunggakan boolean [not null, default: false]
  nominal_standar int [not null, default: 0]
  urutan int [not null, default: 0]
  aktif boolean [not null, default: true]
  created_at datetime
  updated_at datetime
}

Table status_tagihan {
  id bigint [pk, increment]
  kode varchar
  nama varchar [not null]
}

// Table relasi_santri_pegawai {
//   id bigint [pk, increment]
//   santri_id bigint [not null]
//   pegawai_id bigint [not null]
//   hubungan varchar [not null, note: 'Contoh: anak_kandung, anak_tiri']
//   status_aktif boolean [not null, default: true]
//   tgl_mulai date
//   tgl_selesai date
//   keterangan text
//   created_at datetime
//   updated_at datetime

//   Indexes {
//     santri_id
//     pegawai_id
//     (santri_id, pegawai_id, hubungan) [unique]
//   }
// }

Table aturan_dispensasi {
  id bigint [pk, increment]
  lembaga_id varchar [not null]
  kode varchar [not null]
  nama varchar [not null]
  target_tipe varchar [not null, note: 'Contoh: anak_pegawai']
  tipe_nilai varchar [not null, note: 'Contoh: persen / nominal']
  nilai decimal(12,2) [not null, default: 0]
  is_active boolean [not null, default: true]
  // prioritas int [not null, default: 0]
  tgl_mulai date
  tgl_selesai date
  keterangan text
}

Table aturan_dispensasi_biaya {
  id bigint [pk, increment]
  aturan_dispensasi_id bigint [not null]
  biaya_id bigint [not null]
}

Table santri_dispensasi {
  id bigint [pk, increment]
  santri_id bigint [not null]
  aturan_dispensasi_id bigint [not null]
  status_aktif boolean [not null, default: true]
  catatan text
}

Table tagihan {
  id bigint [pk, increment]
  lembaga_id varchar [not null]
  santri_id bigint [not null]
  biaya_id bigint [not null]
  tahun_ajaran_id bigint
  status_tagihan_id bigint
  nama_tagihan varchar
  periode_tagihan varchar
  nominal_awal int [not null, default: 0]
  nominal_potongan int [not null, default: 0]
  nominal_final int [not null, default: 0]
  nominal_terbayar int [not null, default: 0]
  tgl_tagihan date
  tgl_jatuh_tempo date
  keterangan text
}


Table pembayaran {
  id bigint [pk, increment]
  lembaga_id varchar [not null]
  santri_id bigint [not null]
  metode_pembayaran_id bigint
  nomor_bukti varchar
  tgl_bayar date [not null]
  total_bayar int [not null, default: 0]
  keterangan text
  created_at datetime
  updated_at datetime

  Indexes {
    lembaga_id
    santri_id
    metode_pembayaran_id
    tgl_bayar
  }
}

Table detail_pembayaran {
  id bigint [pk, increment]
  pembayaran_id bigint [not null]
  tagihan_id bigint [not null]
  jumlah_bayar int [not null, default: 0]
  keterangan text
  created_at datetime
  updated_at datetime

  Indexes {
    pembayaran_id
    tagihan_id
    (pembayaran_id, tagihan_id) [unique]
  }
}



// Table tagihan_potongan {
//   id bigint [pk, increment]
//   tagihan_id bigint [not null]
//   dispensasi_santri_id bigint
//   nama_potongan_snapshot varchar [not null]
//   tipe_nilai_snapshot varchar [not null]
//   nilai_snapshot decimal(12,2) [not null, default: 0]
//   dasar_perhitungan int [not null, default: 0]
//   nominal_potongan int [not null, default: 0]
//   urutan int [not null, default: 1]
//   keterangan text
//   created_at datetime
//   updated_at datetime

//   Indexes {
//     tagihan_id
//     dispensasi_santri_id
//     (tagihan_id, urutan)
//   }
// }





// ===========
// RELASI
// ===========

Ref: role_user.user_id > users.id [delete: cascade, update: cascade]
Ref: role_user.role_id > roles.id [delete: cascade, update: cascade]

Ref: sessions.user_id > users.id [delete: set null, update: cascade]

// Ref: alamat.santri_id > santri.id [delete: cascade, update: cascade]

Ref: kelas.lembaga_id > lembaga.id [delete: cascade, update: cascade]
// Ref: wali_kelas.kelas_id > kelas.id [delete: cascade, update: cascade]
// Ref: wali_kelas.pegawai_id > pegawai.id [delete: set null, update: cascade]

Ref: proses_psb.lembaga_id > lembaga.id [delete: cascade, update: cascade]
Ref: gelombang_psb.proses_psb_id > proses_psb.id [delete: cascade, update: cascade]
Ref: pendaftaran.lembaga_id > lembaga.id [delete: cascade, update: cascade]
Ref: pendaftaran.gelombang_id > gelombang_psb.id [delete: set null, update: cascade]
Ref: pendaftaran.santri_id > santri.id [delete: cascade, update: cascade]
Ref: log_status_pendaftaran.pendaftaran_id > pendaftaran.id [delete: cascade, update: cascade]
Ref: tes_psb.proses_psb_id > proses_psb.id [delete: cascade, update: cascade]
Ref: nilai_tes.pendaftaran_id > pendaftaran.id [delete: cascade, update: cascade]
Ref: nilai_tes.tes_psb_id > tes_psb.id [delete: cascade, update: cascade]

Ref: log_lembaga_santri.lembaga_id > lembaga.id [delete: cascade, update: cascade]
Ref: log_lembaga_santri.santri_id > santri.id [delete: cascade, update: cascade]
Ref: log_lembaga_santri.pendaftaran_id > pendaftaran.id [delete: set null, update: cascade]
Ref: kelas_santri.kelas_id > kelas.id [delete: cascade, update: cascade]
Ref: kelas_santri.santri_id > santri.id [delete: cascade, update: cascade]
Ref: mutasi_keluar_santri.santri_id > santri.id [delete: cascade, update: cascade]
Ref: mutasi_keluar_santri.kelas_id > kelas.id [delete: set null, update: cascade]
Ref: mutasi_keluar_santri.lembaga_id > lembaga.id [delete: cascade, update: cascade]
Ref: kelulusan_santri.santri_id > santri.id [delete: cascade, update: cascade]
Ref: kelulusan_santri.lembaga_id > lembaga.id [delete: cascade, update: cascade]

Ref: dokumen_santri.santri_id > santri.id [delete: cascade, update: cascade]

Ref: kas.lembaga_id > lembaga.id [delete: cascade, update: cascade]
Ref: metode_pembayaran.kas_id > kas.id [delete: set null, update: cascade]
Ref: biaya.lembaga_id > lembaga.id [delete: cascade, update: cascade]
Ref: biaya.pos_keuangan_id > pos_keuangan.id [delete: set null, update: cascade]

Ref: pegawai.user_id > users.id [delete: set null, update: cascade]

Ref: riwayat_pendidikan_pegawai.pegawai_id > pegawai.id [delete: cascade, update: cascade]
Ref: keluarga_pegawai.pegawai_id > pegawai.id [delete: cascade, update: cascade]
Ref: rekening_pegawai.pegawai_id > pegawai.id [delete: cascade, update: cascade]
Ref: dokumen_pegawai.pegawai_id > pegawai.id [delete: cascade, update: cascade]

// Ref: relasi_santri_pegawai.santri_id > santri.id [delete: cascade, update: cascade]
// Ref: relasi_santri_pegawai.pegawai_id > pegawai.id [delete: restrict, update: cascade]
Ref: aturan_dispensasi.lembaga_id > lembaga.id [delete: cascade, update: cascade]
Ref: aturan_dispensasi_biaya.aturan_dispensasi_id > aturan_dispensasi.id [delete: cascade, update: cascade]
Ref: aturan_dispensasi_biaya.biaya_id > biaya.id [delete: cascade, update: cascade]
// Ref: santri_dispensasi.relasi_santri_pegawai_id > relasi_santri_pegawai.id [delete: restrict, update: cascade]
Ref: santri_dispensasi.aturan_dispensasi_id > aturan_dispensasi.id [delete: restrict, update: cascade]

Ref: tagihan.lembaga_id > lembaga.id [delete: restrict, update: cascade]
Ref: tagihan.santri_id > santri.id [delete: restrict, update: cascade]
Ref: tagihan.biaya_id > biaya.id [delete: restrict, update: cascade]
// Ref: tagihan_potongan.tagihan_id > tagihan.id [delete: cascade, update: cascade]
// Ref: tagihan_potongan.dispensasi_santri_id > dispensasi_santri.id [delete: set null, update: cascade]
Ref: pembayaran.lembaga_id > lembaga.id [delete: restrict, update: cascade]
Ref: pembayaran.santri_id > santri.id [delete: restrict, update: cascade]
Ref: pembayaran.metode_pembayaran_id > metode_pembayaran.id [delete: set null, update: cascade]
Ref: detail_pembayaran.pembayaran_id > pembayaran.id [delete: cascade, update: cascade]
Ref: detail_pembayaran.tagihan_id > tagihan.id [delete: restrict, update: cascade]

// Ref: "pegawai"."id" < "terminasi_pegawai"."pegawai_id"

// Ref: "pegawai"."id" < "kontrak_pegawai"."pegawai_id"