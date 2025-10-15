digraph HospitalSystem {
    rankdir=TB;
    node [shape=box, style=rounded, fontsize=10];

    // Ana Menü
    Start [label="Başla"];
    Menu [label="Ana Menü\n(1) Randevu Al\n(2) Tahlil Görüntüle"];
    Decision [label="Seçim?", shape=diamond];
    End [label="Bitti"];

    Start -> Menu -> Decision;

    // ======= RANDEVU MODÜLÜ =======
    subgraph cluster_randevu {
        label="Modül 1: Randevu Alma";
        style=dashed;
        
        Auth1 [label="Kimlik Doğrulama"];
        AuthCheck1 [label="Giriş Başarılı mı?", shape=diamond];
        Poliklinik [label="Poliklinik Seç"];
        Doktor [label="Doktor Seç"];
        Saat [label="Uygun Saat Seç"];
        SaatKontrol [label="Saat Müsait mi?", shape=diamond];
        RandevuKaydet [label="Randevuyu Kaydet"];
        SmsGonder [label="SMS Gönder"];

        // Akış
        Decision -> Auth1 [label="1"];
        Auth1 -> AuthCheck1;
        AuthCheck1 -> Poliklinik [label="Evet"];
        AuthCheck1 -> End [label="Hayır"];
        Poliklinik -> Doktor -> Saat -> SaatKontrol;
        SaatKontrol -> RandevuKaydet [label="Evet"];
        SaatKontrol -> End [label="Hayır"];
        RandevuKaydet -> SmsGonder -> End;
    }

    // ======= TAHLİL MODÜLÜ =======
    subgraph cluster_tahlil {
        label="Modül 2: Tahlil Sonuçları";
        style=dashed;

        Auth2 [label="Kimlik Doğrulama"];
        AuthCheck2 [label="Giriş Başarılı mı?", shape=diamond];
        TahlilVar [label="Tahlil Var mı?", shape=diamond];
        SonucHazir [label="Sonuç Hazır mı?", shape=diamond];
        Goruntule [label="Tahlil Sonucunu Göster"];
        PdfIndir [label="PDF İndir"];
        Bekleme [label="Bekleme Mesajı Göster"];

        // Akış
        Decision -> Auth2 [label="2"];
        Auth2 -> AuthCheck2;
        AuthCheck2 -> TahlilVar [label="Evet"];
        AuthCheck2 -> End [label="Hayır"];
        TahlilVar -> SonucHazir [label="Evet"];
        TahlilVar -> End [label="Hayır"];
        SonucHazir -> Goruntule [label="Evet"];
        SonucHazir -> Bekleme [label="Hayır"];
        Goruntule -> PdfIndir -> End;
        Bekleme -> End;
    }
}

