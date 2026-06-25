# Bluestamp Color Sensing Memory Game
The color sensing memory game is a game in which the game will flash colors at the human player and the player has to match them in order to move on to the next difficulty. The game will keep on adding colors to the sequence until the player makes a mistake.

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Sashvi M | Cerritos High School | Electrical Engineering | Incoming Junior


**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**


![Headstone Image](logo.svg)
  
# Final Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>


For my final milestone, I added an LCD screen that displays the high score of the time that the game is on that resets each time you unplug, and that also displays your current score under that. I also added a D F Mini Player and a speaker to state when a new round is starting, celebration when you get a new high score, and a sad music for when the game is over. I had a lot of challenges in adding my modifications, mainly my speaker code. It took me the most time to figure this out as my speaker was origionaly buzzing the whole time and I had to work hard with many people until I discovered what was wrong. I am very happy with how my project has turned out with all of my modifications and I hope to keep on finding things that I can use to make my project better and better.


# Second Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/y3VAmNlER5Y" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>


For my second milestone, I have worked on the software side of my project. I used C++ to code the logic behind the game into it. I added the code behind the game flashing the lights first, and then the buttons flashing them following them. I also added the game to stop and the LEDs to flash when each round ends. I then added the code for the display to add the score at the end. Some challenges that I faced including figuring out how to get the display to flash the numbers and get the LEDs to turn off after they flash at the end of a round. Next I am going to be working on my modifications including adding an LCD screen to show your current score and the high score and a d f mini player with a speaker to play sounds and music throughout it.

# First Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/heWFLB8U8-4?si=0P1acKjgqoYlQun6" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


For my first milestone, I finished the hardware for my game. In my game there are 4 LED bulbs, 4 buttons, a 7-segment dispay, and a decoder that are all used in the game. The objective of the game is to copy the lights that the game gives off in order. So far I have connected all of the wires, lights, buttons, resistors, the dispay, and the decoder. Some challenges that I have faced throughout this process are that I origionally started my project too far to the right and then had to move everything to the left to make it work. Another challenge was that I mixed up the positive and negitive sides of the LEDs and it took me a while to figure that out. My plan is to finish my initial coding for my second milestone and to have my third milestone be modifications including adding a high score board, sound, and a way to automatacly move from one game to the next.

# Schematics 
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 

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

Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs.

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |

# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

