// 1. KULLANICI GİRİŞİ
function kullaniciGirisi(kullaniciAdi, sifre):
    if veritabanindaKullaniciVarMi(kullaniciAdi, sifre):
        session.olustur(kullaniciAdi)
        return "Giriş başarılı"
    else:
        return "Hatalı kullanıcı adı veya şifre"

// 2. ÜRÜN SEÇME VE SEPETE EKLEME
function urunEkle(sepet, urunID, adet):
    urun = urunBilgisiGetir(urunID)
    if urun.stok >= adet:
        sepet.urunEkle(urunID, adet)
        return "Ürün sepete eklendi"
    else:
        return "Yetersiz stok"

// 3. SEPET GÖRÜNTÜLEME
function sepetiGoster(sepet):
    for urun in sepet.urunler:
        yazdir(urun.ad, urun.adet, urun.fiyat)
    yazdir("Toplam Tutar:", sepet.toplamFiyatHesapla())

// 4. İNDİRİM KODU UYGULAMA
function indirimKoduUygula(sepet, kod):
    if indirimKoduGecerliMi(kod):
        indirimOrani = kodGetir(kod).oran
        sepet.toplamFiyat *= (1 - indirimOrani)
        return "İndirim uygulandı"
    else:
        return "Geçersiz indirim kodu"

// 5. KARGO HESAPLAMA
function kargoHesapla(sepet, adres):
    if sepet.toplamFiyat > 500:
        kargoUcreti = 0
    else:
        kargoUcreti = kargoUcretiHesapla(adres.il, sepet.agirlik)
    sepet.kargoUcreti = kargoUcreti
    return kargoUcreti

// 6. ÖDEME EKRANI
function odemeYap(sepet, odemeBilgileri):
    toplamTutar = sepet.toplamFiyat + sepet.kargoUcreti
    if odemeIslemiBasarili(odemeBilgileri, toplamTutar):
        stokGuncelle(sepet)
        siparisOlustur(sepet)
        sepet.temizle()
        return "Ödeme başarılı, sipariş alındı"
    else:
        return "Ödeme başarısız, lütfen tekrar deneyin"

// 7. STOK GÜNCELLEME
function stokGuncelle(sepet):
    for urun in sepet.urunler:
        veritabani.urunStokAzalt(urun.id, urun.adet)

// 8. SİPARİŞ OLUŞTURMA
function siparisOlustur(sepet):
    siparis = new Siparis()
    siparis.urunler = sepet.urunler
    siparis.kullanici = session.kullanici
    siparis.toplamTutar = sepet.toplamFiyat + sepet.kargoUcreti
    siparis.tarih = bugun()
    veritabani.siparisKaydet(siparis)
