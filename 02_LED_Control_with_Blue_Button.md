# 1. `STM32CubeMX'e Geri Dönüş ve Yanılgı`
`.ioc` uzantılı dosyayı CLion üzerinden açtım, bu sayede CubeMX arayüzüne geri döndüm.
1. **PC13** pinini **GPIO_Input** olarak tanımlamam gerekiyordu ancak kafam karıştığı için **PA13** pinine baktım. Bu pin kart üzerinde tanımlı olduğu için herhangi bir şey yapmadan sağ üstten **"Generate Code"** butonuna basarak geri döndüm.

# 2. `"main.c" Kodunu Güncelleme`
`main.c` içindeki `while(1)` döngüsünü aşağıdaki gibi güncelledim:
```
/* USER CODE BEGIN WHILE */
while (1)
{
    // PC13 pinindeki (Mavi Buton) durumu oku
    // NOT: Nucleo kartlarında buton "Active Low" çalışır. 
    // Yani butona BASILDIĞINDA pin "GPIO_PIN_RESET" (0) olur.
    if (HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_13) == GPIO_PIN_RESET)
    {
        // Butona basılıyorsa LED'i YAK (Pin'e high ver)
        HAL_GPIO_WritePin(GPIOA, GPIO_PIN_5, GPIO_PIN_SET);
    }
    else
    {
        // Buton bırakıldıysa LED'i SÖNDÜR (Pin'e low ver)
        HAL_GPIO_WritePin(GPIOA, GPIO_PIN_5, GPIO_PIN_RESET);
    }

    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */
}
/* USER CODE END 3 */
```

# 3. `Çalıştırdım, Ama...`
Çalıştırdığımda fark ettiğim şey şu oldu: STM kartım üzerindeki LD2 kısmındaki yeşil LED sürekli yanıyordu ve mavi butona basınca hiçbir şey değişmiyordu. Sonra siyah butona bastım ve siyah butona basınca LD2 kısmındaki yeşil LED'in söndüğünü fark ettim. Yani mavi butonla LED'i kontrol etmeyi umuyorken, siyah butonla kontrol eder oldum.

Yaptığım araştırma sonucunda, siyah butonun **"Reset (NRST)"** butonu olduğunu öğrendim. Yani yazdığım kod sebebiyle siyah bu tona basınca LED sönmüyor; siyah butona basınca **işlemciyi donanımsal resete soktuğum ve tüm GPIO pinleri başlangıç (reset) durumuna döndüğü için** doğal olarak o LD2 LED'i sönüyordu.

Bu durumun asıl sebebinin ise, kod üzerinde girdi (Input) olarak okumaya çalıştığım PC13 pininin CubeMX üzerinde `GPIO_Input` olarak ayarlanmayışı olduğunu anladım. CubeMX üzerine geri döndüğümde PC13 pinini doğru şekilde tanımladım ve sistem olması gerektiği gibi çalıştı.

# 4. `Bu Sefer Çalıştı!`
Tekrar runladığımda bu sefer mavi butona bastığımda LED'i yaktığımı ve bıraktığımda söndürdüğümü (tıpkı kodda yazdığım gibi) gördüm.
