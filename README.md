#Esp32 in arduíno ide
https://dl.espressif.com/dl/package_esp32_index.json

# Single Channel Gateway Usando Heltec WiFi LoRa 32 (Com OLED display)

- [Arquivos](https://github.com/luisfelip80/ESP32GatewayLoraCanalUnico)

# Traduzido de:
- [Original](https://ldsrc.blogspot.com/2018/01/lorawan-working-example-from-network.html)

# ESP32SingleChannelGateway
Um Fork do popular ESP8266 gateway canal único adaptado para ESP32 na frequência de 915Mhz banda.

# Instruções
- Copie as bibliotecas(libraries) para ~/Documentos/Arduino/libraries
- Para o nó lora, verifique na biblioteca lmic o arquivo config.h e tenha certeza que a banda está em 915Mhz apenas.

# 1) Single Channel Gateway Usando Heltec WiFi LoRa 32 (Com OLED display)
"Apenas leia as coisas que eu fiz para que isso funcionasse (Espero que funciona para os outros)
- [Instale o Arduino IDE](https://www.arduino.cc/en/Main/Software3)
- [Instale ESP8266 core em github](https://github.com/esp8266/Arduino6)
- [Instale ESP32 core em github](https://github.com/espressif/arduino-esp3210)
- Execute o Arduino IDE e vá em “File → Preferences” (Arquivos → Preferências) e faça uma anotação da “Localização do Sketchbook”. Guarde esse “local”  para ser usada depois quando for carregar as pastas. Ele é algo como “C:\Users\...\Documents\arduino”. No Windows, ou “/home/User/.../Arduino” no Linux.
- Extraia o Zip.
- Copie tudo da pasta extraída “libraries” e cole no “local/libraries”.
- Copie as pastas “ESP-sc-gway” e “ESP-sc-node” da pasta Zip e cole em “local/libraries”
- Vá para “local/libraries/ESP-sc-gway.h” e altere o código usando um editor de texto. Você precisa mudar as 3 seguintes partes:


1 Local do gateway
(descrição, Lat (latitude),  long (longitude))

2 ntp
(Mude a zona de tempo NTP_TIMESERVER “uk.pool.ntp.org” para UK, para serves no [Brasil veja](https://ntp.br/)  e mude NTP_TIMEZONES 0

3 Definição de  wpas wpa[]

NO CÓDIGO VOCÊ VAI ENCONTRAR  ISSO:
```
#if 0
wpas wpa[] = {
{ “” , “” }, // Reserved for WiFi Manager
{ “ape”, “apePasswd” }
{ “ape”, “apePasswd” }
};
#else
Place outside version control to avoid the risk of commiting it to github :wink:
#include “d:\arduino\wpa.h”
#endif
```
MUDE PARA ISSO(mude também o WiFi e senha, também tire o if-else-endif):
```
wpas wpa[] = {
{ "" , "" }, // Reserved for WiFi Manager
{ "SEU WIFI SSID", "SUA SENHA DO WIFI SSID" },
{"",""}
};
```
Execute a IDE do Arduino.

Upload o sketch via “File→Exemples→ESP-sc-gway”. Ou vá na pasta “local/libraries/ESP-sc-gway” e abra o arquivo “ESP-sc-gway.ino”. Coloque a porta certa da placa e faça o upload para seu Heltec WiFi LoRa 32 device, (Quando o aparelho fizer o re-boot, você vai ver a informação no display OLED).

Quando o aparelho tiver bootado e logado na sua rede WiFi, ache o endereço IP, ele vai ser usado no seu roteador. Use esse IP no seu navegador Web para ver a webpage dos detalhes do Gateway. Anote os 8 byte “Gateway ID” (você vai precisar dele para o console TTN). Faça o Login no console do TTN e crie um novo gateway – você vai precisar marcas o “I’m using the legacy packet forwarder” checkbox e cole os 8 bytes “Gateway ID” da webpage.

Mude outras coisas como frequência e salve (Aqui usados a 915).
É isso - incrível!!!

# 2) Exemplos de Código TX para HelTec Lora (para usar junto com Gateway)

Essa biblioteca já deve estar na pasta de bibliotecas, mas caso dê algum erro de compilação, tente isso: Baixe a pasta Zip do LMIC https://github.com/matthijskooijman/arduino-lmic8

Extraia e renomeie a pasta “arduino-lmic-master” para “arduino-lmic” (isso pode ser um clash com outras versões da biblioteca “LMIC”)

Coloque a pasta renomeada “arduino-lmic” na “local/libraries”.

(opcional) se você tiver problemas com quando compilar o código, tente remover a  biblioteca chamada “IBM_LMIC_framework” do “local/libraries” (você pode facilmente pegar ela de volta usando o gerenciador de bibliotecas)

Abra a pasta a pasta “ESP-sc-node.ino” em “local/ESP-sc-node”.

As 3 linhas que você precisa mudar estão com o prefixo “$$”. Remova os “$$” e substitua o código usando as informações do TTN – Lembre que no TTN você pode apenas mudar o aparelho(Device) de “OTAA” para “ABP” dpeois de salvar o aparelho (Device) no TTN! Assim que o aparelho for trocado para “ABP”, você vai ver 3 códigos no console que você precisa copiar no código do arduino.

Para DEVADDR/Device Address – apenas copie o hexadecimal e adiciona antes o 0x no código.
Para NWKSKEY e APPSKEY – click no olho (show code) dai o <> (C-style), click em “copy” e use isso para colar no código ( não pressione o botão ⇆ e veja que sua exibição MSB antes de copiar!)."
