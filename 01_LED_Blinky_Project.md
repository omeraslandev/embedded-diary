Aşağıdaki kodu yazdım:
```
    /* USER CODE BEGIN 3 */

    // PA5 pininin durumunu tersine cevir (Aciksa kapat, kapaliysa ac).
    HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_5);

    // 500 milisaniye (0.5 saniye bekle).
    HAL_Delay(500);
   }
  /* USER CODE END 3 */
```
Ardından sırasıyla yeşil çekiç (build) butonuna, ve ardından yeşil oynat (run) butonuna bastım.

Kod mikrodenetleyici kartıma aktarılınca, altında "LD2" yazan yeşil LED'in 0.5 saniyede bir yanıp söndüğünü gördüm.

Arka planda ne olduğunu şöyle açıklayabilirim:
```
[HAL_GPIO_TogglePin] ---> Registers (Yazmaçlar) ---> ODR (Output Data Register) bitini tersle
                                                                 │
[HAL_Delay(500)]    <--- İşlemciyi Güvenli Döngüde Tut <--- PA5 Pinine Akım Git/Kes (3.3V / 0V)
```