#include <Keypad.h>
#include <LiquidCrystal_I2C.h>
LiquidCrystal_I2C lcd(32,16,2); // Configura endereço I2C e display com 16 caracteres e 2 linhas
int peso = 0; // Variável para armazenar o valor do sensor de peso
int first = 0; // Variável para armazenar o valor de tempo inserido
int timer = 0; // Variável de contagem regressiva
int bal = A0; // Atribui o pino A0 na variável bal
int led = 13; // Atribui p pino 13 na variável led
char customKey; // Variável para armazenar a tecla pressionada
unsigned long previousMillis = 0; // Variável para armazenar o último tempo em milissegundos
const long interval = 1000; // Intervalo de 1 segundo

const byte ROWS = 4; // Define o número de linhas do teclado
const byte COLS = 4; // Define o número de colunas do teclado

char keys[ROWS][COLS] = {// Mapeia as teclas do teclado
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'+','0','=','D'}
};

byte rowPins[ROWS] = {12, 11, 10, 9}; // Conecta às saídas das linhas do teclado
byte colPins[COLS] = {8, 7, 6, 5}; // Conecta às saídas das colunas do teclado

Keypad customKeypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);// Inicializa o teclado

void setup() {
  pinMode(bal, INPUT); // Define a variável bal como entrada para o sensor de peso
  pinMode(led, OUTPUT); // Define a variável led como saída
  lcd.init(); // Inicializa o LCD
  lcd.backlight(); // Ativa o backlight do LCD
  lcd.print("TAMAGOTCHI"); // Imprime a mensagem
  delay(2000); // Espera 2 segundos
  lcd.clear(); // Limpa o display LCD
  lcd.setCursor(0, 0); // Move o cursor para a primeira coluna da primeira linha
  lcd.print("Defina o tempo:"); // Imprime a mensagem
  Serial.begin(9600); // Inicializa a comunicação serial a 9600 bps
}

void loop() {
  peso = analogRead(bal); // Atribui a variável bal na variável peso
  if (peso < 40 || first == 0) { // Verifica se o peso é menor que 40 ou se o tempo não foi definido
    digitalWrite(led, HIGH); // Escreve na variável led HIGH
  } else {
    digitalWrite(led, LOW); //Escreve na variável led LOW
  }
  customKey = customKeypad.getKey();// Lê a tecla pressionada no teclado
  if (customKey) {
    switch (customKey) {
      case '0' ... '9': // Captura valores até que um operador seja pressionado
        lcd.setCursor(0, 1);// Move o cursor para a primeira coluna da segunda linha
        first = first * 10 + (customKey - '0');// Atualiza o valor de tempo inserido
        lcd.print(first);// Imprime a variável first
        break;
      case 'A': // Inicia a contagem regressiva
        timer = first; // Define o timer com o valor inserido
        lcd.clear(); // Limpa o display LCD
        lcd.setCursor(0, 0); // Move o cursor para a primeira coluna da primeira linha
        lcd.print("Tempo restante:"); // Imprime a mensagem "Tempo restante:" no LCD
        break;
      case 'B': // Defina uma ação específica para a tecla 'B'
        first = 1; // Define 'first' para 1
        break;
      case 'C': // Reseta o valor de 'first'
        first = 0; // Reseta o valor de 'first'
        timer = 0; // Reseta o valor do timer
        lcd.clear(); // Limpa o display LCD
        lcd.setCursor(0, 0); // Move o cursor para a primeira coluna da primeira linha
        lcd.print("Defina o tempo:"); // Imprime a mensagem
        break;
    }
  }

  if (timer > 0) {// Se o timer é maior que 0
    unsigned long currentMillis = millis(); // Captura o tempo atual em milissegundos
    if (currentMillis - previousMillis >= interval) { // Verifica se um segundo se passou
      previousMillis = currentMillis; // Atualiza o último tempo
      timer--; // Diminui o timer em 1
      lcd.setCursor(0, 1); // Move o cursor para a primeira coluna da segunda linha
      lcd.print("                "); // Limpa a linha
      lcd.setCursor(0, 1); // Move o cursor para a primeira coluna da segunda linha
      lcd.print(timer); // Imprime o valor do timer no LCD
      lcd.print(" seg"); // Imprime a mensagem
    }
    if (timer == 0) { // Se o timer é igual a 0
      lcd.clear(); // Limpa o display LCD
      lcd.setCursor(0, 0); // Move o cursor para a primeira coluna da primeira linha
      lcd.print("Defina o tempo:"); // Imprime a mensagem
      first = 0; // Reseta o valor de 'first'
    }
  }
}
