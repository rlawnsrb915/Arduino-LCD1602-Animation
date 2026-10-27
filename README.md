# Arduino-LCD1602-Animation
Arduino LCD1602 Custom Character Animation

<회로도>
<img width="826" height="361" alt="image" src="https://github.com/user-attachments/assets/d6c7ffa3-dca8-4a00-aa71-fd8e770e565d" />

<코드 스케치>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27, 16, 2);

// 0. 초기 알
byte c0[] = {
  B00000,
  B00100,
  B01110,
  B11111,
  B11111,
  B11111,
  B01110,
  B00000
};

// 1. 작은 금
byte c1[] = {
  B00000,
  B00100,
  B01010,
  B11111,
  B11111,
  B11111,
  B01110,
  B00000
};

// 2. 금이 퍼짐
byte c2[] = {
  B00000,
  B00100,
  B01010,
  B11011,
  B10111,
  B11111,
  B01110,
  B00000
};

// 3. 깨지기 직전
byte c3[] = {
  B00000,
  B00100,
  B01010,
  B10001,
  B10101,
  B11011,
  B01110,
  B00000
};

// 4. 껍질이 깨짐
byte c4[] = {
  B10101,
  B01010,
  B10001,
  B00100,
  B10001,
  B01010,
  B10101,
  B00000
};

// 5. 껍질 파편
byte c5[] = {
  B10001,
  B00000,
  B00100,
  B01110,
  B00100,
  B00000,
  B10001,
  B00000
};

// 6. 계란 후라이
byte c6[] = {
  B00000,
  B00000,
  B01110,
  B11011,
  B10101,
  B11011,
  B01110,
  B00000
};

// 애니메이션 순서 (c6 두 번 출력)
byte animation[] = {0, 1, 2, 3, 4, 5, 6, 6};

byte frame = 0;
unsigned long previousMillis = 0;

// 모든 프레임 동일한 속도
const unsigned long interval = 200;

void setup() {
  lcd.init();
  lcd.backlight();
  lcd.clear();

  // Custom Character 등록
  lcd.createChar(0, c0);
  lcd.createChar(1, c1);
  lcd.createChar(2, c2);
  lcd.createChar(3, c3);
  lcd.createChar(4, c4);
  lcd.createChar(5, c5);
  lcd.createChar(6, c6);

  // 첫째 줄 첫 번째 칸에 출력
  lcd.setCursor(0, 0);
  lcd.write((byte)0);
}

void loop() {
  unsigned long currentMillis = millis();

  if (currentMillis - previousMillis >= interval) {
    previousMillis = currentMillis;

    frame = (frame + 1) % 8;

    lcd.setCursor(0, 0);
    lcd.write(animation[frame]);
  }
}
