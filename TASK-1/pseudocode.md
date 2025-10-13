BAŞLA

  GÜNLÜK_LIMIT ← 5000
  PIN_HAK ← 3
  HESAP_BAKIYE ← 10000
  GÜNLÜK_CEKILEN ← 0
  DOGRU_PIN ← "1234"

  // KART OKUMA
  YAZ "Kartınızı takınız..."
  OKU KART

  // === PIN DOĞRULAMA ===
  PIN_DOGRU_MU ← FALSE

  PIN_DENEME ← 0
  PIN_GIRIS ← ""

  DÖNGÜ PIN_DENEME < PIN_HAK VE PIN_DOGRU_MU = FALSE
    YAZ "Lütfen PIN kodunuzu giriniz:"
    OKU PIN_GIRIS

    EĞER PIN_GIRIS = DOGRU_PIN İSE
      PIN_DOGRU_MU ← TRUE
    DEĞİLSE
      PIN_DENEME ← PIN_DENEME + 1
      KALAN ← PIN_HAK - PIN_DENEME
      EĞER KALAN > 0 İSE
        YAZ "Yanlış PIN. Kalan deneme: ", KALAN
      DEĞİLSE
        YAZ "3 kez yanlış PIN girildi. Kart bloke edildi."
        ÇIK
      BİTİŞ
    BİTİŞ
  BİTİŞ_DÖNGÜ

  EĞER PIN_DOGRU_MU = FALSE İSE
    ÇIK
  BİTİŞ

  // === ANA MENÜ ===
  ISLEM_TEKRAR ← "E"

  DÖNGÜ ISLEM_TEKRAR = "E"

    YAZ "--------- ANA MENÜ ---------"
    YAZ "1 - Para Çekme"
    YAZ "2 - Bakiye Görüntüle"
    YAZ "3 - Çıkış"
    YAZ "Bir seçenek giriniz (1-3):"
    OKU SECIM

    EĞER SECIM = 1 İSE

      // === PARA ÇEKME ===
      YAZ "Mevcut bakiye: ", HESAP_BAKIYE, " TL"
      YAZ "Bugün çekilen: ", GÜNLÜK_CEKILEN, " TL"
      YAZ "Günlük limit: ", GÜNLÜK_LIMIT, " TL"

      YAZ "Çekmek istediğiniz tutarı girin (20 TL ve katı):"
      OKU TUTAR

      // === TUTAR KONTROLÜ ===
      EĞER TUTAR <= 0 İSE
        YAZ "Geçersiz tutar."
      DEĞİLSE EĞER TUTAR MOD 20 ≠ 0 İSE
        YAZ "Lütfen 20 TL ve katı bir tutar giriniz."
      DEĞİLSE EĞER TUTAR > HESAP_BAKIYE İSE
        YAZ "Yetersiz bakiye."
      DEĞİLSE EĞER GÜNLÜK_CEKILEN + TUTAR > GÜNLÜK_LIMIT İSE
        YAZ "Günlük limitinizi aşıyorsunuz."
      DEĞİLSE
        HESAP_BAKIYE ← HESAP_BAKIYE - TUTAR
        GÜNLÜK_CEKILEN ← GÜNLÜK_CEKILEN + TUTAR
        YAZ TUTAR, " TL başarıyla çekildi."
        YAZ "Yeni bakiye: ", HESAP_BAKIYE, " TL"

        // FİŞ SORUSU

