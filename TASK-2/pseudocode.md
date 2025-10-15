digraph ECommerceFlow {
    rankdir=TB;
    node [shape=box style=rounded fontsize=10];

    Start [label="Başla"];
    Login [label="Kullanıcı Girişi"];
    LoginCheck [label="Giriş Başarılı mı?", shape=diamond];

    AddToCart [label="Ürün Sepete Ekle"];
    StockCheck [label="Stok Yeterli mi?", shape=diamond];
    ShowCart [label="Sepeti Göster"];
    ApplyDiscount [label="İndirim Kodu Uygula"];
    CalculateShipping [label="Kargo Hesapla"];
    Payment [label="Ödeme Yap"];
    PaymentSuccess [label="Ödeme Başarılı mı?", shape=diamond];

    UpdateStock [label="Stok Güncelle"];
    CreateOrder [label="Sipariş Oluştur"];
    End [label="Bitti"];

    // Akışlar
    Start -> Login;
    Login -> LoginCheck;
    LoginCheck -> AddToCart [label="Evet"];
    LoginCheck -> End [label="Hayır"];

    AddToCart -> StockCheck;
    StockCheck -> ShowCart [label="Evet"];
    StockCheck -> End [label="Hayır"];

    ShowCart -> ApplyDiscount;
    ApplyDiscount -> CalculateShipping;
    CalculateShipping -> Payment;
    Payment -> PaymentSuccess;

    PaymentSuccess -> UpdateStock [label="Evet"];
    PaymentSuccess -> End [label="Hayır"];

    UpdateStock -> CreateOrder;
    CreateOrder -> End;
}

