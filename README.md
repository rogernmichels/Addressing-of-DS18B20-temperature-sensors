# Addressing-of-DS18B20-temperature-sensors
Code for addressing DS18B20 temperature sensors for automated collection via Arduino platform.
// Programa : Scan DS18B20
// Alterações : Arduino & Cia
// Este programa procura pelos sensores no circuito e mostra o valor 
// do endereço físico de cada sensor no Serial Monitor

#include <OneWire.h>

OneWire  ds(10);  // Conecte o pino central dos sensores ao pino 10 do Arduino

void setup(void) {
  Serial.begin(9600); // inicia a comunicação serial
  discoverOneWireDevices(); // cria a função discoverOneWireDevices
}

void discoverOneWireDevices(void) { //define a função discoverOneWireDevices
  // cria variáveis do tipo byte para armazenar os valores qe serão encontrados durante o programa
  byte i;
  byte present = 0;
  byte data[12];
  byte addr[8];
  
  Serial.print("Procurando dispositivos DS18B20...\n\r"); 
  while(ds.search(addr)) {  //define um loop de que será executado enquanto a função search encontrar novos endereços
    Serial.print("\n\rEncontrado dispositivo \'DS18B20\' com endereco:\n\r");
    for( i = 0; i < 8; i++) { //loop que escreve o endereço do sensor da forma que ele deve ser usado
      Serial.print("0x"); //escreve "0x" que deve existir no começo de cada trechoo do endereço
      if (addr[i] < 16) { //caso o valor encontrado seja menor que 16 escreve um 0 (importante para manter a integridade do endereço, pois este usa o padrão hexadecimal)
        Serial.print('0');
      }
      Serial.print(addr[i], HEX);
      if (i < 7) { //escreve "," nas sete primeiras passagens do loop, já que o endereço é composto de 8 partes separadas por ","
        Serial.print(", ");
      }
    }
    if ( OneWire::crc8( addr, 7) != addr[7]) { //chama a função crc8 que retorna um erro caso o endereço encontrado encontrado seja inválido
        Serial.print("CRC nao e valido!\n");
        return;
    }
  }
  Serial.print("\n\r\n\rFinal da verificacao.\r\n");
  ds.reset_search(); //reinicia a função search
  return;
}

void loop(void) {
  // Loop Vazio
}
