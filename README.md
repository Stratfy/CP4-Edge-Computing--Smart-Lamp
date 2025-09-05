# CP4 — Edge Computing — Smart Lamp (ESP32)  
**Stratfy × Vinheria Agnello**

Firmware (`.ino`) para uma **lâmpada inteligente** baseada em **ESP32**.  
Lê **luminosidade** (LDR) e envia periodicamente para o backend (ex.: FIWARE).  
Permite **ligar/desligar** a lâmpada via saída digital (LED/relé).

> **Equipe Stratfy**  
> Luigi Mendes Cabrini — RA 563552  
> Bruno Koeke — RA 561309  
> Rogério Cruz Arroyo — RA 563517  
> Anthony Sforzin — RA 562096

---

## Funcionalidades
- Conexão Wi‑Fi (SSID/senha configuráveis no código).
- Leitura analógica do LDR (conversão para percentual ajustável).
- Envio periódico de telemetria (HTTP POST com pequeno payload JSON).
- Controle local da lâmpada (GPIO → LED/relé).
- (Opcional) Botão para alternar estado localmente.

---

## Hardware mínimo
- ESP32 DevKit (ou similar)  
- LDR + resistor (divisor de tensão, p.ex. 10 kΩ)  
- LED **ou** módulo relé (para carga externa)  
- Jumpers e protoboard

### Ligações (sugerido)
- `PIN_LDR = GPIO34` (entrada ADC, somente leitura)  
- `PIN_RELAY_OR_LED = GPIO23` (saída digital)  
- Divisor do LDR entre **3V3** e **GND**, ponto médio no **ADC**.

> Os pinos podem ser alterados nas `#define` do `.ino`.

---

## Configuração no código
Edite no topo do arquivo:
```cpp
// Wi‑Fi
#define WIFI_SSID  "SUA_REDE"
#define WIFI_PASS  "SUA_SENHA"

// Endpoint de telemetria (HTTP POST)
#define SERVER_URL "http://SEU_SERVIDOR/telemetry"

// Pinos
#define PIN_LDR           34
#define PIN_RELAY_OR_LED  23

// Intervalo de envio (ms)
#define TELEMETRY_INTERVAL_MS 5000
```
> O payload enviado é um JSON simples contendo ID do dispositivo, leitura do LDR e estado on/off.

---

## Como compilar e gravar
1. **Arduino IDE 2.x** (ou **PlatformIO**).  
2. Placa: **ESP32 Dev Module**.  
3. Selecione a **porta** correta.  
4. Faça o **Upload**.  
5. Abra o **Serial Monitor** (115200) para logs de Wi‑Fi, leitura e envio.

---

## Operação
- A cada `TELEMETRY_INTERVAL_MS` o firmware lê o ADC do LDR, normaliza e envia ao servidor.  
- O estado da lâmpada é controlado pela variável interna e refletido no GPIO (`PIN_RELAY_OR_LED`).  
- Cubra/exponha o LDR para observar variações no Serial.

---

## Notas de segurança
- Para **cargas em 127/220 V**, use **relé/módulo adequado com isolamento** e siga normas elétricas.  
- Ajuste a **conversão ADC→%** conforme o seu divisor e iluminação do ambiente.

---

## Licença
Defina a licença do projeto (ex.: MIT) conforme necessidades do grupo.
