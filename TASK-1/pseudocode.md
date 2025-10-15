BAŞLA

    pinHakkı ← 3
    doğruPIN ← 1234
    bakiye ← 2500
    günlükLimit ← 1500
    günlükÇekilen ← 0

    // PIN DOĞRULAMA
    DÖNGÜ pinHakkı > 0 İKEN
        YAZ "Lütfen 4 haneli PIN kodunuzu giriniz:"
        OKU girilenPIN

        EĞER girilenPIN = doğruPIN İSE
            ÇIKIŞ DÖNGÜDEN
        DEĞİLSE
            pinHakkı ← pinHakkı - 1
            YAZ "Hatalı PIN. Kalan hakkınız: ", pinHakkı

    EĞER pinHakkı = 0 İSE
        YAZ "Kartınız bloke olmuştur. Lütfen banka ile iletişime geçiniz."
