// Declares the constants and variables.
const int redLed = 3;
const int yellowLed = 10;
const int blueLed = 2;
const int redBut = 8;
const int yellowBut = 12;
const int blueBut = 7;
const int buzzer = 5;
int Size = 1, score = 0, i;
char seq[10], answer[10], option;

// Raffles the sequence of LEDs that will blink.
void giveAway(){
  int n;
  for(i = 0; i < Size; i++){
    n = random(3);
    if(n == 0) seq[i] = 'r';
    else if(n == 1) seq[i] = 'y';
    else if(n == 2) seq[i] = 'b';
  }
}

// Reproduces a sound representing the red LED.
void redSound(){
  if(option == 'y') tone(buzzer, 1800, 900);
}

// Reproduces a sound representing the yellow LED.
void yellowSound(){
  if(option == 'y') tone(buzzer, 1900, 900);
}

// Reproduces a sound representing the blue LED.
void blueSound(){
  if(option == 'y') tone(buzzer, 2000, 900);
}

// Lets the user choose if the buzzer is going to be activated or is not.
void turnBuzzer(){
  Serial.print(" Activate the sound? (y) or (n). ");
  while(true)
    if(Serial.available() > 0){
      option = Serial.read();
      break;
    }
   Serial.println(option);
   Serial.println();
}

// Turns the LEDs ON and plays each respective sound.
void ledsOn(){
  for(i = 0; i < Size; i++){
    if(seq[i] == 'r'){
      digitalWrite(redLed,HIGH);
      redSound();
    }
    else if(seq[i] == 'y'){
      digitalWrite(yellowLed,HIGH);
      yellowSound();
    }
    else if(seq[i] == 'b'){
      digitalWrite(blueLed,HIGH);
      blueSound();
    }
    delay(900);
    if(seq[i] == 'r') digitalWrite(redLed,LOW);
    else if(seq[i] == 'y') digitalWrite(yellowLed,LOW);
    else if(seq[i] == 'b') digitalWrite(blueLed,LOW);
    delay(800);
  }
}

// Gets the user's answer by the keyboard ot the buttons...
void takeAnswer(){
  Serial.print(" What's the sequence?");
  for(i = 0; i < Size;){
    if(Serial.available() > 0) butInput();
    else if(digitalRead(redBut) == LOW || digitalRead(yellowBut) == LOW || digitalRead(blueBut) == LOW) keyboardInput();
  }
}

// If the answer comes by the keyboard.
void keyboardInput(){
  answer[i] = Serial.read();
  if(answer[i] == 'r') redSound();
  else if(answer[i] == 'y') yellowSound();
  else if(answer[i] == 'b') blueSound();
  printAnswer();
}

// If the answer comes by a button.
void butInput(){
  if(digitalRead(redBut) == LOW){
    redSound();
    answer[i] = 'r';
  }
  else if(digitalRead(yellowBut) == LOW){
    yellowSound();
    answer[i] = 'y';
  }
  else if(digitalRead(blueBut) == LOW){
    blueSound();
    answer[i] = 'b';
  }
  printAnswer();
}

// Shows the answer.
void printAnswer(){
  Serial.print(" ");
  Serial.print(answer[i]);
  delay(900);
  i++;
}

// Verifies if the user won or lost the round...
void result(){
  int res = 0;
  for(i = 0; i < Size; i++)
    if(seq[i] != answer[i]) res = 1;
  Serial.println();
  if(res == 0) right();
  else wrong();
  delay(150);
  noTone(buzzer);
}

// In the case of a correct sequence.
void right(){
  Serial.println(" Right!");
  score++;
  if(option == 'y'){
    tone(buzzer, 2100);
    delay(150);
    tone(buzzer, 1950);
  }
  if(Size == 10){
    Serial.println(" You win!");
    Size = 0;
  }
  digitalWrite(redLed,HIGH);
  digitalWrite(yellowLed,HIGH);
  digitalWrite(blueLed,HIGH);
  delay(100);
  digitalWrite(redLed,LOW);
  digitalWrite(yellowLed,LOW);
  digitalWrite(blueLed,LOW);
  Size++;
}

// In the case of an incorrect sequence.
void wrong(){
  Serial.println(" Wrong!");
  score = 0;
  Size = 1;
  if(option == 'y'){
    tone(buzzer, 1550);
    delay(150);
    tone(buzzer, 1350);
  }
}

// Here, the PINs are configurated, the user can choose for the activation of the buzzer and the game's difficulty.
// Also, the give away's starting point is shuffled.
void setup(){
  pinMode(yellowLed,OUTPUT);
  pinMode(blueLed,OUTPUT);
  pinMode(redLed,OUTPUT);
  pinMode(yellowBut,INPUT_PULLUP);
  pinMode(blueBut,INPUT_PULLUP);
  pinMode(redBut,INPUT_PULLUP);
  Serial.begin(9600);
  turnBuzzer();
  randomSeed(analogRead(0));
}

// The loop consists in shuffling the sequence of LEDs, turning on them, taking the answer, showing the results and
// then starting a new round.
void loop(){
  giveAway();
  ledsOn();
  takeAnswer();
  delay(800);
  result();
  Serial.print(" Score: ");
  Serial.println(score);
  Serial.println();
  delay(2000);
}
