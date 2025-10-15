function akilliEvGuvenlikSistemi():
    sistemAktif = true

    while sistemAktif:
        sensorVerisi = sensorOku()
        
        if tehditAlgilandi(sensorVerisi):
            alarmCal()
            bildirimGonder()
        else:
            bekle(1 saniye)
