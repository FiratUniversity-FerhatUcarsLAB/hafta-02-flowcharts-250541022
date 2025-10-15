function dersKayitSistemi():
    ogrenci = girisYap()
    if not ogrenci:
        yazdir("Giriş başarısız.")
        return

    dersListesi = sistemDersListesiniGetir()
    secilenDersler = []

    while True:
        ders = dersSec(dersListesi)
        if ders == "bitir":
            break
        secilenDersler.append(ders)

    toplamKredi = 0

    for ders in secilenDersler:
        if ders.kontenjan <= 0:
            yazdir(ders.ad + ": Kontenjan dolu.")
            continue

        if not onKosulSaglandiMi(ogrenci, ders):
            yazdir(ders.ad + ": Ön koşul sağlanmıyor.")
            continue

        if zamanCakismasiVarMi(secilenDersler, ders):
            yazdir(ders.ad + ": Zaman çakışması var.")
            continue

        toplamKredi += ders.kredi

    if toplamKredi > ogrenci.krediLimiti:
        yazdir("Kredi limiti aşıldı: " + toplamKredi)
        return

    if danismanOnayiGerekiyor(secilenDersler):
        if not danismanOnayiAlindi(ogrenci):
            yazdir("Danışman onayı gerekli.")
            return

    dersKaydiniOnayla(ogrenci, secilenDersler)
    yazdir("Ders kaydı başarıyla tamamlandı.")
