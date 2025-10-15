function kimlikDogrula(tcKimlikNo, sifre):
    if kullaniciVarMi(tcKimlikNo, sifre):
        session.olustur(tcKimlikNo)
        return true
    else:
        return false

function tahlilVarMi(tcKimlikNo):
    return veritabanindaTahlilVarMi(tcKimlikNo)

function sonucHazirMi(tcKimlikNo):
    return tahlilSonucuHazir(tcKimlikNo)

function tahlilGoruntule(tcKimlikNo):
    sonuc = tahlilSonucGetir(tcKimlikNo)
    yazdir("Tahlil Sonucu:", sonuc)

function pdfIndir(tcKimlikNo):
    pdf = tahlilPDFHazirla(tcKimlikNo)
    indir(pdf)

function tahlilSistemiCalistir():
    tc = kullaniciGirdisi("TC Kimlik No:")
    sifre = kullaniciGirdisi("Şifre:")

    if not kimlikDogrula(tc, sifre):
        yazdir("Kimlik doğrulama başarısız.")
        return

    if not tahlilVarMi(tc):
        yazdir("Kayıtlı tahlil bulunamadı.")
        return

    if sonucHazirMi(tc):
        tahlilGoruntule(tc)
        pdfIndir(tc)
    else:
        yazdir("Tahlil sonucu henüz hazır değil. Lütfen daha sonra tekrar deneyin.")
