# 🤖 Robô Seguidor de Linha

> Firmware para robô autônomo seguidor de linha com controle PID

[![C](https://img.shields.io/badge/C-A8B9CC?style=flat-square&logo=c&logoColor=white)]()
[![Arduino](https://img.shields.io/badge/Arduino-00979D?style=flat-square&logo=arduino&logoColor=white)]()
[![Status](https://img.shields.io/badge/Status-Concluído-22c55e?style=flat-square)]()

---

## Sobre o Projeto

Repositório com os códigos desenvolvidos para um **robô autônomo seguidor de linha**, capaz de detectar e seguir uma trilha no chão usando sensores infravermelhos e algoritmo de controle PID.

---

## ✨ Funcionalidades

- 📶 Leitura de sensores infravermelhos para detecção de linha
- 🎯 Algoritmo **PID** para navegação suave e precisa
- ⚡ Controle de motores DC via PWM
- 🔄 Calibração automática dos sensores
- 🚧 Tratamento de cruzamentos e curvas acentuadas

---

## 🛠️ Hardware Utilizado

| Componente | Descrição |
|---|---|
| Microcontrolador | Arduino Uno / Nano |
| Sensores | Array de sensores IR |
| Atuadores | Motores DC com encoder |
| Driver | Ponte H (L298N / L293D) |
| Alimentação | Bateria Li-Po 7.4V |

---

## 🚀 Como Usar

```bash
# Clone o repositório
git clone https://github.com/MarcosCF1/Rob-Seguidor-de-Linha.git

# Abra o projeto no Arduino IDE
# Selecione a placa correta (Arduino Uno/Nano)
# Faça o upload do firmware
```

**Ajuste as constantes PID** no arquivo principal conforme seu hardware:
```c
#define KP 1.2
#define KI 0.01
#define KD 0.8
```

---

## 👤 Autor

**Marcos Felipe** — [github.com/MarcosCF1](https://github.com/MarcosCF1)
