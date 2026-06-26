# Bluestamp Color Sensing Memory Game
The color sensing memory game is a game in which the game will flash colors at the human player and the player has to match them in order to move on to the next difficulty. The game will keep on adding colors to the sequence until the player makes a mistake.

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Sashvi M | Cerritos High School | Electrical Engineering | Incoming Junior


**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**


![Headstone Image](logo.svg)
  
# Final Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/mRS1ZB0bbeU?si=VcDlFa-7eDzorLIz" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

For my final milestone, I added an LCD screen that displays the high score of the time that the game is on that resets each time you unplug, and that also displays your current score under that. I also added a D F Mini Player and a speaker to state when a new round is starting, celebration when you get a new high score, and a sad music for when the game is over. I had a lot of challenges in adding my modifications, mainly my speaker code. It took me the most time to figure this out as my speaker was origionaly buzzing the whole time and I had to work hard with many people until I discovered what was wrong. I am very happy with how my project has turned out with all of my modifications and I hope to keep on finding things that I can use to make my project better and better.


# Second Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/PIJasSJQAQE?si=c5N-fjxDLv9g6Ikq" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

For my second milestone, I have worked on the software side of my project. I used C++ to code the logic behind the game into it. I added the code behind the game flashing the lights first, and then the buttons flashing them following them. I also added the game to stop and the LEDs to flash when each round ends. I then added the code for the display to add the score at the end. Some challenges that I faced including figuring out how to get the display to flash the numbers and get the LEDs to turn off after they flash at the end of a round. Next I am going to be working on my modifications including adding an LCD screen to show your current score and the high score and a d f mini player with a speaker to play sounds and music throughout it.

# First Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/heWFLB8U8-4?si=0P1acKjgqoYlQun6" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


For my first milestone, I finished the hardware for my game. In my game there are 4 LED bulbs, 4 buttons, a 7-segment dispay, and a decoder that are all used in the game. The objective of the game is to copy the lights that the game gives off in order. So far I have connected all of the wires, lights, buttons, resistors, the dispay, and the decoder. Some challenges that I have faced throughout this process are that I origionally started my project too far to the right and then had to move everything to the left to make it work. Another challenge was that I mixed up the positive and negitive sides of the LEDs and it took me a while to figure that out. My plan is to finish my initial coding for my second milestone and to have my third milestone be modifications including adding a high score board, sound, and a way to automatacly move from one game to the next.

# Schematics 
<img width="609" height="629" alt="Screenshot 2026-06-26 at 9 48 41 AM" src="https://github.com/user-attachments/assets/7ee69d3a-cd77-4a51-bfdb-afc9ed3646b8" />

# Code

Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs.


```c++
#include <Wire.h> 
#include <LiquidCrystal_I2C.h>
#include <SoftwareSerial.h>
#include <DFRobotDFPlayerMini.h>

// Set the LCD address to 0x27 for a 16 chars and 2 line display
LiquidCrystal_I2C lcd(0x27, 16, 2);

// Setup software serial pins using working test configuration
SoftwareSerial mySoftwareSerial(10, 11); // D10 = Arduino RX (to DFPlayer TX), D11 = Arduino TX (to DFPlayer RX)
DFRobotDFPlayerMini myDFPlayer;

namespace Delays {
    enum : uint16_t {
        LONG = 1000,
        MEDIUM_LONG = 750,
        MEDIUM = 500,
        SHORT = 250,
    };
};

namespace Colors {
    enum ColorType : uint8_t {
        BLUE,
        GREEN,
        YELLOW,
        RED
    };

    constexpr Colors::ColorType colors[] = {
        Colors::BLUE,
        Colors::GREEN,
        Colors::YELLOW,
        Colors::RED
    };
    constexpr uint8_t num_colors = sizeof(colors) / sizeof(colors[0]);
};

namespace Leds {
    enum PinType : uint8_t {
        BLUE = 6,
        GREEN = 7,
        YELLOW = 8,
        RED = 9
    };

    void reset() {
        digitalWrite(Leds::BLUE, LOW);
        digitalWrite(Leds::GREEN, LOW);
        digitalWrite(Leds::YELLOW, LOW);
        digitalWrite(Leds::RED, LOW);
    }

    void all_on() {
        digitalWrite(Leds::BLUE, HIGH);
        digitalWrite(Leds::GREEN, HIGH);
        digitalWrite(Leds::YELLOW, HIGH);
        digitalWrite(Leds::RED, HIGH);
    }
}

namespace Buttons {
    enum PinType : uint8_t {
        BLUE = 2,
        GREEN = 3,
        YELLOW = 4,
        RED = 5,
    };

    constexpr Buttons::PinType buttons[] = {
        Buttons::BLUE,
        Buttons::GREEN,
        Buttons::YELLOW,
        Buttons::RED
    };
    constexpr uint8_t num_buttons = sizeof(buttons) / sizeof(buttons[0]);
}

Leds::PinType led_pin_of(Colors::ColorType color) {
    switch (color) {
        case Colors::BLUE: return Leds::BLUE;
        case Colors::GREEN: return Leds::GREEN;
        case Colors::YELLOW: return Leds::YELLOW;
        case Colors::RED: return Leds::RED;
        default: return (Leds::PinType) 0;
    }
}

Leds::PinType button_to_led(Buttons::PinType pin) {
    switch (pin) {
        case Buttons::BLUE: return Leds::BLUE;  
        case Buttons::GREEN: return Leds::GREEN;  
        case Buttons::YELLOW: return Leds::YELLOW;  
        case Buttons::RED: return Leds::RED;
        default: return (Leds::PinType) 0;  
    }
}

Colors::ColorType button_to_color(Buttons::PinType pin) {
    switch (pin) {
        case Buttons::BLUE: return Colors::BLUE;
        case Buttons::GREEN: return Colors::GREEN;
        case Buttons::YELLOW: return Colors::YELLOW;
        case Buttons::RED: return Colors::RED;
        default: return Colors::BLUE;
    }
}

bool button_is_pressed(Buttons::PinType pin) {
    return digitalRead(pin) == LOW;
}

Colors::ColorType get_button_input() {
    Buttons::PinType button_pin = (Buttons::PinType) 0;
    Leds::PinType led_pin = (Leds::PinType) 0;
    
    while (button_pin == 0) {
        for (uint8_t i = 0; i < Buttons::num_buttons; i++) {
            if (button_is_pressed(Buttons::buttons[i])) {
                button_pin = Buttons::buttons[i];
                led_pin = button_to_led(button_pin);
                digitalWrite(led_pin, HIGH);
                break;
            }
        }
    }
    
    while (button_is_pressed(button_pin)) {
        delay(10); // Faster, snappier button debounce polling
    }
    
    digitalWrite(led_pin, LOW);
    delay(50); // Small debounce window between inputs
    return button_to_color(button_pin);
}

struct GameState {
    Colors::ColorType sequence[UINT8_MAX + 1];
    uint8_t round;
    bool has_lost;
    uint8_t high_score; 
};

struct GameState game_state;

void initalize_game_state() {
    game_state.round = 0;
    game_state.has_lost = false;
}

void generate_next_color() {
    uint8_t index = random(Colors::BLUE, Colors::RED + 1);
    Colors::ColorType color = Colors::colors[index];
    game_state.sequence[game_state.round] = color;
    game_state.round++;
}

void display_sequence() {
    for (uint8_t i = 0; i < game_state.round; i++) {
        Leds::PinType pin = led_pin_of(game_state.sequence[i]);
        digitalWrite(pin, HIGH);
        delay(Delays::MEDIUM_LONG);
        digitalWrite(pin, LOW);
        delay(Delays::SHORT);
    }
}

// Separate function to update LCD to completely eliminate screen flickering
void update_game_lcd() {
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("High Score: ");
    lcd.print(game_state.high_score);

    lcd.setCursor(0, 1);
    lcd.print("Score: ");
    lcd.print(game_state.round);
}

void start_round() {
    generate_next_color();
    update_game_lcd(); 
    display_sequence();
    
    for (uint8_t i = 0; i < game_state.round; i++) {
        Colors::ColorType color = get_button_input();
        if (color != game_state.sequence[i]) {
            game_state.has_lost = true;
            break;
        }
    }
}

void display_start() {
    // FIXED: Uses working test structure (/mp3/0001.mp3)
    myDFPlayer.playMp3Folder(1); 

    digitalWrite(Leds::BLUE, HIGH);
    delay(Delays::MEDIUM);
    digitalWrite(Leds::GREEN, HIGH);
    delay(Delays::MEDIUM);
    digitalWrite(Leds::YELLOW, HIGH);
    delay(Delays::MEDIUM);
    digitalWrite(Leds::RED, HIGH);
    delay(Delays::MEDIUM);
    
    Leds::reset();
    digitalWrite(Leds::BLUE, HIGH);
    digitalWrite(Leds::GREEN, HIGH);
    delay(Delays::MEDIUM);
    Leds::reset();
    digitalWrite(Leds::YELLOW, HIGH);
    digitalWrite(Leds::RED, HIGH);
    delay(Delays::MEDIUM);
}

void display_loss() {
    // FIXED: Plays /mp3/0002.mp3
    myDFPlayer.playMp3Folder(2); 

    Leds::all_on();
    delay(Delays::LONG);
    Leds::reset();
}

void display_high_score_celebration() {
    // FIXED: Plays /mp3/0003.mp3
    myDFPlayer.playMp3Folder(3); 

    for (int i = 0; i < 4; i++) {
        Leds::all_on();
        delay(Delays::SHORT);
        Leds::reset();
        delay(Delays::SHORT);
    }
}

void setup() {
    randomSeed(analogRead(A0));
    initalize_game_state();
    game_state.high_score = 0;
    
    pinMode(Buttons::BLUE, INPUT_PULLUP);
    pinMode(Buttons::GREEN, INPUT_PULLUP);
    pinMode(Buttons::YELLOW, INPUT_PULLUP);
    pinMode(Buttons::RED, INPUT_PULLUP);
    
    pinMode(Leds::BLUE, OUTPUT);
    pinMode(Leds::GREEN, OUTPUT);
    pinMode(Leds::YELLOW, OUTPUT);
    pinMode(Leds::RED, OUTPUT);

    lcd.init();
    lcd.backlight();

    mySoftwareSerial.begin(9600);
    
    if (myDFPlayer.begin(mySoftwareSerial)) {
        myDFPlayer.volume(20); 
    } else {
        lcd.setCursor(0, 0);
        lcd.print("Audio Error!");
        while(1); // Stop execution if connection failed
    }

    lcd.setCursor(0, 0);
    lcd.print("Welcome Player!");
    delay(Delays::LONG * 2);
    lcd.clear();

    lcd.setCursor(0, 0);
    lcd.print("Watch Closely and");
    lcd.setCursor(0, 1);
    lcd.print("Copy the Colors!");
    delay(Delays::LONG * 2);
    lcd.clear();
    
    display_start();
    Leds::reset();
    delay(Delays::LONG);
}

void loop() {
    if (!game_state.has_lost)  {
        start_round();
        delay(Delays::MEDIUM_LONG); 
    } else {
        uint8_t final_score = game_state.round - 1;

        if (final_score > game_state.high_score) {
            game_state.high_score = final_score;

            lcd.clear();
            lcd.setCursor(0, 0);
            lcd.print("NEW HIGH SCORE!!!");
            lcd.setCursor(0, 1);
            lcd.print("Score: ");
            lcd.print(final_score);
            display_high_score_celebration();
            delay(Delays::LONG * 2);
        } else {
            lcd.clear();
            lcd.setCursor(0, 0);
            lcd.print("GAME OVER");
            lcd.setCursor(0, 1);
            lcd.print("Final Score: ");
            lcd.print(final_score);
            display_loss();
        }
        delay(Delays::LONG);
        initalize_game_state();
        display_start();
        Leds::reset();
    }
}
```
# Bill of Materials

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Arduino Starter 1 | Base of the project | $44.88 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/ELEGOO-Project-Tutorial-Controller-Projects/dp/B01D8KOZF4/ref=sr_1_1?dib=eyJ2IjoiMSJ9._L3JiWgIo_Asrnpq9JBCAvlFJKU-cwUzPEOX6Xf2L2ocJ5VwjOWbJ7InSxeX25zyxpBZeI01sPn8IXm6km2PRBq2fQPugHT_ehDjSpnudGaxnVApmlAN4PU9YooLBBsLg7XqS0qh296_sHdxy7YBotvHfNEnD_3EBUamknDg-kBNTWfYhXJPBygMBY4I29b_IWZBqoqM9pIXMIkWrTluA5y6SZlIzqvk_8UaoxsYUU0.WVJCYuLtjZfC_Pxjs5ltQ35CxXinbD40L-RgbPLhU-w&dib_tag=se&keywords=arduino%2Bstarter%2Bkit&qid=1779928287&sr=8-1&th=1)"> Link </a> |
| DMM | Saftey | $9.99 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/dp/B0CXM242J1?ref=fed_asin_title&th=1)"> Link </a> |
| 12C LCD 1602 Display | High and Current Scores | $9.99 | <a href="(https://www.amazon.com/SunFounder-Serial-Module-Display-Arduino/dp/B019K5X53O?th=1)"> Link </a> |
| 8ohm Speaker | give out noise | $9.99| <a href="[link](https://www.amazon.com/DWEII-Loundspeaker-Compatible-Motherboard-Electronic/dp/B0CX1JC6NM/ref=sr_1_1_pp?crid=GV8U1RO8ACMR&dib=eyJ2IjoiMSJ9.xLQ3cVWcOi3Yc2kulD9z1Ff2mu094D4FCYaz1SI8xkbosrTWdxR4zx8hADLPdATW3jXaOvSv11xMVeVfjV2lg-QgTrePe8CxRqV3eqMAvx0fnqM0U4Qa6xzrZn2f7MgYXSghqKVCqavJdGirrSe1gANyOWqll6RAyR2Y-EBhKQXXwe5scUM0Y-KWmCAI_0Vf_srXhKVYiPXURxPxRtAdyQ3tOETK0jrNPehKql_DFBpQzBRDOlWEr9ZKdCmCbWYlQFwNj9QZxJoskinFclEP923xc_8w97w8CiD-dQpLYHI.G2-g8XDTSSMlv_rmIPEjl-rUDjkzvJQ08qrhJRHfn54&dib_tag=se&keywords=speaker%2Barduino&qid=1781213803&s=electronics&sprefix=speaker%2Barduino%2Celectronics%2C214&sr=1-1&th=1)"> Link </a> |
| SD Card | store different sounds | $4.98| <a href="[link](https://www.amazon.com/Class10-Memory-Extreme-Compatibility-Meomory/dp/B0GKPVR4RH/ref=sr_1_12?dib=eyJ2IjoiMSJ9.WpvOswVDewFqiI3veGUzsCymqT6rPhaS5vyfN3W4iImKPISiq73v_Whty0RBZOYLQzcT8t4o8MgG6LZdzc0W9DOIwoBzuyStqt0JiP94G1FmqLRupi7xnAJ4g_AMLyvThfCWFcJodole7awWuTGJxGKyhvrDP8JzgDs_RWMj0zwBEkwpSbvEqpeb8-N74NdZ8B-YdmSp1yLPaohe5baFuQa7V5RSlp4e1R6A0cUJ_K8.603luMMc6GoqCpn7KrAkpRU30Il_TBWAN7q2Jkgcxaw&dib_tag=se&keywords=micro+sd+card&qid=1781205599&sr=8-12)"> Link </a> |
| DFP Player | transfer SD card noise to speaker | $9.90| <a href="[link](https://www.amazon.com/DFPlayer-A-Mini-MP3-Player/dp/B089D5NLW1/ref=sr_1_2?crid=2TFMSNV3V6X4N&dib=eyJ2IjoiMSJ9.YrXsgIIjUaSsAEXykz_XhedAiNLuph_OWmxQmAavcckeBOFIILf0piK1lQqo5NxzajN6ZStSOjzJboPlpjoUf_kIRnJjQde-WrCF5xGtuzN_h6Vk3qwf0gPz50b3ClmYutnkj3rVJj84S0BNAllkj77MPYEoenGCggxFdwCZynAEKI6Xv-G6-VPkppnA7AVo_TFeSb4Brkx4LgydYz-Bx8gVlbt7OkCHi7QBQMqfzFk.8VD_uGGjesENqjp_unTxGKDrtmXUpVXbU7zOdh2OHbo&dib_tag=se&keywords=df+player+with+arduino&qid=1781205043&sprefix=df+player+with+arduino%2Caps%2C169&sr=8-2)"> Link </a> |
| Converter SD to USBC | download sound on SD card | $8.98| <a href="[link](https://www.amazon.com/Anker-Reader-Memory-RS-MMC-Micro/dp/B07NW8RPYN/ref=sr_1_3?crid=1SH32TWW75I8Z&dib=eyJ2IjoiMSJ9.1KsRUD7kzbYoYbE95yB_XkPKwqCZWOzBCvf-R03qoQzXzOrVd73sK7Y2f5FFc_zgsrGP6jtJyPyEctpFxCd0NvdwJA0iUhm7Wze3Rvq3hy-mNmG4k68tykGtuw6jlo07As3BOZTWtah3FHr_UxKK5WiPysOYDjU7YO7btTQ0RRGQu2AT9tJejK-rZEzUOJgZMeQJf-sbmQ8d2jftk2SpCPuCD1V7tO0JDQHeLJnT6NwTmDTyDws3vwXYTgAZjv75cTKhVJSYccmUBzgjpCHh1b68Fd7nYOr4x5JeIi9J-pM.VVFVwJvrGLgH7asEKfH2N7aigdTkuFXKtwmjllfd2yA&dib_tag=se&keywords=1%2BMarceloant%2BMemory%2BCard%2BClass%2B10%2BTF%2BCard%2Busbc%2Badapter&nsdOptOutParam=true&qid=1781209751&s=electronics&sprefix=1%2Bmarceloant%2Bmemory%2Bcard%2Bclass%2B10%2Btf%2Bcard%2Busbc%2Badapte%2Celectronics%2C145&sr=1-3&th=1)"> Link </a> |
| Converter USB to USBC | connect project to computer | $6.59| <a href="[link](http://amazon.com/dp/B07CVX3516?ref=nb_sb_ss_w_as-reorder_k3_1_3&amp=&crid=1IXI3GEBWAGFU&amp=&sprefix=usb)"> Link </a> |
