# Colorivaldo - Sistema de Previsão de Cores em Tempo Real

## 📚 Visão Geral

O **Colorivaldo** é um projeto de machine learning desenvolvido no grupo de extensão [Robótica, Automação, Inteligência Artificial e Tecnologias da Universidade Federal do Ceará (RAITec)](https://www.instagram.com/raitec.ufc/) por mim e Francisco Levi, que implementa um sistema capaz de fazer previsão de cores em tempo real utilizando um sensor LDR conectado a um Arduino. O sistema coleta valores RGB através da biblioteca pyserial e utiliza algoritmos de aprendizado de máquina para classificar cores com alta precisão.

---

## Índice

- [Funcionalidades Principais](#funcionalidades-principais)
- [Metodologia de Desenvolvimento](#metodologia-de-desenvolvimento)
- [Algoritmos Testados](#algoritmos-testados)
- [Dataset e Treinamento](#dataset-e-treinamento)
- [Instalação e Configuração](#instalação-e-configuração)
- [Como Usar](#como-usar)
- [Resultados e Performance](#resultados-e-performance)
- [Integração com Arduino](#integração-com-arduino)

## Funcionalidades Principais

### Classificação de Cores em Tempo Real

- **Detecção Automática**: Sistema capaz de identificar cores através de valores RGB coletados por sensor
- **Alta Precisão**: Modelo treinado com dados reais para máxima acurácia
- **Comunicação Serial**: Integração com Arduino via biblioteca pyserial para coleta de dados em tempo real

### Machine Learning

- **Múltiplos Algoritmos**: Comparação entre Random Forest, Logistic Regression, SVM e K-NN
- **Otimização Automática**: Seleção do melhor algoritmo baseada em métricas de performance
- **Validação Cruzada**: Sistema robusto de validação para garantir generalização do modelo

### Análise de Dados

- **Dataset Sintético**: Geração de dados sintéticos para treinamento inicial
- **Dados Reais**: Utilização de dados coletados do Arduino para validação
- **Visualizações**: Gráficos comparativos de performance dos algoritmos

## Metodologia de Desenvolvimento

### 1. Geração de Dataset Sintético
O projeto inicia com a criação de um dataset sintético contendo valores RGB para diferentes cores:
- **Cores Suportadas**: Vermelho, Verde, Azul, Amarelo, Roxo, Ciano, Branco, Preto, Marrom, Magenta
- **Amostras por Cor**: 150-300 amostras sintéticas por classe
- **Ruído Realístico**: Adição de ruído para simular variações do sensor real

### 2. Comparação de Algoritmos
Teste sistemático de 4 algoritmos de machine learning:
- **Random Forest**
- **Logistic Regression**
- **Support Vector Machine (SVM)**
- **K-Nearest Neighbors (K-NN)**

### 3. Treinamento com Dados Reais
- **Coleta de Dados**: Utilização do arquivo `dadoscores.csv` com dados reais do Arduino
- **Validação**: Teste do modelo com dados coletados durante testagem prática
- **Otimização**: Ajuste fino dos hiperparâmetros

### 4. Integração Arduino
- **Comunicação Serial**: Implementação da comunicação em tempo real
- **Predição**: Sistema capaz de classificar cores instantaneamente
- **Extração da String**: Código otimizado para execução em tempo real

## Algoritmos Testados

### K-NN (Vencedor)
```python
# Melhor performance geral
Score Final: 0.9996
CV Accuracy: 1.0000 (±0.0000)
Overfitting: 0.0000
Tempo treino: 0.003s
```

### Logistic Regression
```python
Score Final: 0.9952
CV Accuracy: 1.0000 (±0.0000)
Overfitting: 0.0000
Tempo treino: 0.033s
```

### SVM
```python
Score Final: 0.9824
CV Accuracy: 1.0000 (±0.0000)
Overfitting: 0.0000
Tempo treino: 0.133s
```

### Random Forest
```python
Score Final: 0.9184
CV Accuracy: 1.0000 (±0.0000)
Overfitting: 0.0042
Tempo treino: 0.955s
```

## Dataset e Treinamento

### Dados Sintéticos
- **Total de Amostras**: 2.400 amostras (300 por classe)
- **Características**: Valores RGB normalizados (0-1023)
- **Balanceamento**: Dataset perfeitamente balanceado entre classes
- **Ruído**: Simulação de variações ambientais e do sensor

### Dados Reais (`dadoscores.csv`)
- **Fonte**: Coleta direta do sensor RGB via Arduino
- **Cores Coletadas**: 7 classes diferentes
- **Formato**: CSV com colunas R, G, B, cor
- **Qualidade**: Dados validados em ambiente controlado

### Métricas de Avaliação
- **Acurácia**
- **Estabilidade**
- **Overfitting**
- **Precisão**


## Instalação e Configuração

### Pré-requisitos

- **Python** 3.7 ou superior
- **Jupyter Notebook** ou **Google Colab**
- **Arduino**

### Bibliotecas Python Necessárias

```bash
- Pandas
- Numpy
- Scikit-Learn
- Matplotlib
- Seaborn
- Joblip
- PySerial
```

### Configuração do Arduino

1. **Conecte o sensor RGB** ao Arduino conforme especificações do fabricante
2. **Configure a comunicação serial** na velocidade adequada (9600 baud)
3. **Teste a coleta de dados** antes de executar o modelo

## Como Usar

### Execução do Notebook

1. **Abra o arquivo** `Colorivaldo.ipynb` no Jupyter Notebook ou Google Colab
2. **Execute as células sequencialmente** para:
   - Gerar dataset sintético
   - Comparar algoritmos
   - Treinar o modelo final
   - Testar com dados reais


## Resultados e Performance

### Comparação de Algoritmos

O **K-NN** foi selecionado como melhor algoritmo baseado em múltiplos critérios:

- **Maior Score Final**: 0.9996/1.0000
- **Zero Overfitting**: Generalização perfeita
- **Treinamento Rápido**: 0.003 segundos
- **Acurácia Perfeita**: 100% em validação cruzada

### Cores Suportadas (Dataset)

O sistema é capaz de identificar as seguintes cores:
- 🔴 **Vermelho**
- 🟢 **Verde** 
- 🔵 **Azul**
- 🟡 **Amarelo**
- 🟣 **Roxo/Magenta**
- ⚫ **Preto**
- 🟤 **Marrom**

## Integração com Arduino

### Configuração da Comunicação

```python
scaler = joblib.load("color_scaler.pkl")
model = joblib.load("modelofinal.pkl")
arduino = serial.Serial('COM3', 9600, timeout=1)

def process_color_data(data):
    """Extrai valores R, G, B da string recebida"""
    parts = data.split(',')
    R = int(parts[0].split(':')[1])
    G = int(parts[1].split(':')[1])
    B = int(parts[2].split(':')[1])
    return R, G, B

try:
    while True:
        if arduino.in_waiting > 0:
            # Lê uma linha completa
            raw_data = arduino.readline().decode('utf-8').strip()

            if raw_data.startswith('R:') and 'G:' in raw_data and 'B:' in raw_data:
                try:
                    # Processa os valores
                    r, g, b = process_color_data(raw_data)
                    print("R: ")
                    print(r)
                    print(" G:")
                    print(g)
                    print(" B:")
                    print(b)
                    dados_lidos = np.array([[R, G, B]])
                    dados_escalonados = scaler.transform(dados_lidos)
                    # Faz a predição com o modelo
                    color = model.predict(dados_escalonados)[0]
                    print(f"\nCor encontrada: {color.lower()}")


                    # Envia resposta de volta para Arduino e será mostrado no painel
                    arduino.write(f"{color}\n".encode())

                except Exception as e:
                    print(f"Erro no processamento: {str(e)}")

except KeyboardInterrupt:
    print("Programa encerrado")
    arduino.close()
```

### Código Arduino

```cpp
int r, g, b;
const int Rpin = 8;
const int Gpin = 5;
const int Bpin = 4;
int k = 0;
// abaixo, valores obtidos em ambientes iluminados
int Ramb = 913;
// padrao: 1016
int Gamb = 1016;
int Bamb = 1016;

#include <Wire.h>
#include <LiquidCrystal_I2C.h>

// Definições do LCD
#define endereco  0x27 // Endereços comuns: 0x27, 0x3F
#define colunas   16
#define linhas    2

LiquidCrystal_I2C lcd(endereco, colunas, linhas);


void setup() {
  Serial.begin(9600);
  // Referente ao display LCD
  lcd.init(); // INICIA A COMUNICAÇÃO COM O DISPLAY
  lcd.backlight(); // LIGA A ILUMINAÇÃO DO DISPLAY
  lcd.clear(); // LIMPA O DISPLAY
  lcd.setCursor(0, 0);
  lcd.print("Ola, Raitecos <3");
  pinMode(Rpin, OUTPUT);   //  Vermelho
  pinMode(Gpin, OUTPUT);  //   Verde
  pinMode(Bpin, OUTPUT);  //   Azul

      digitalWrite(Rpin, HIGH);
      digitalWrite(Gpin, HIGH);
      digitalWrite(Bpin, HIGH);
  
      delay(50);
      digitalWrite(Rpin, HIGH);
      digitalWrite(Gpin, HIGH);
      digitalWrite(Bpin, HIGH);
      delay(100);
  // Logo abaixo, trata-se do modelo com o qual obtivemos os valores médios de Ramb, Gamb, Bamb para ambientes bem iluminados
  //while (k<2){
     // delay(500);
     // digitalWrite(Rpin, LOW);
     // digitalWrite(Gpin, HIGH);
     // digitalWrite(Bpin, HIGH);
     // delay(500);
     // Ramb = analogRead(A0);

     // Após isso, veremos com o Verde (G)
     // digitalWrite(Rpin, HIGH);
     // digitalWrite(Gpin, LOW);
     // digitalWrite(Bpin, HIGH);
     // delay(500);
     // Gamb = analogRead(A0);

     // Agora com o Azul (B)
     // digitalWrite(Rpin, HIGH);
     // digitalWrite(Gpin, HIGH);
     // digitalWrite(Bpin, LOW);
     // delay(500);
     // Bamb = analogRead(A0);
     // Serial.print("R(ambiente): ");
     // Serial.print(Ramb);
     // Serial.print("   G(ambiente): ");
     // Serial.print(Gamb);
     // Serial.print("   B(ambiente): ");
     // Serial.println(Bamb);
     // k+=1;
  //}
}


void loop() {

      delay(100);
      digitalWrite(Rpin, LOW);
      digitalWrite(Gpin, HIGH);
      digitalWrite(Bpin, HIGH);
      delay(500);
      r = analogRead(A0);

      // Após isso, veremos com o Verde (G)
      digitalWrite(Rpin, HIGH);
      digitalWrite(Gpin, LOW);
      digitalWrite(Bpin, HIGH);
      delay(500);
      g = analogRead(A0);

      // Agora com o Azul (B)
      digitalWrite(Rpin, HIGH);
      digitalWrite(Gpin, HIGH);
      digitalWrite(Bpin, LOW);
      delay(500);
      b = analogRead(A0);
      // resultado final
      Serial.print("R:");
      Serial.print(abs((-r+Ramb)/2));
      Serial.print(",G:");
      Serial.print(-g+Gamb);
      Serial.print(",B:");
      Serial.println(-b+Bamb);
    
      delay(100);
      //digitalWrite(Rpin, HIGH);
      //digitalWrite(Gpin, HIGH);
      //digitalWrite(Bpin, HIGH);
      //delay(100);

// Envia o que foi lido para o monitor serial (Arduino), cujos dados serão lidos e interpretados no script python
    
  // Aguarda resposta do Python
   
      if(Serial.available()>0){
      String resposta = Serial.readStringUntil('\n');
      lcd.clear();
      lcd.setCursor(0,0);
      lcd.print("Cor observada");
      lcd.setCursor(0,1);
      lcd.print(resposta);
      delay(50);
      }
}
```

---

## Características Técnicas 
**Linguagem Principal**: Python  
**Plataforma**: Jupyter Notebook / Google Colab  
**Hardware**: Arduino + Sensor de Luminosidade LDR
**Dataset**: Arquivo 'dadoscores.csv'
**Algoritmo Vencedor**: K-Nearest Neighbors  

---
