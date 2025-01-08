// Definición de sensores
const int sensorIzquierdo = A0;
const int sensorCentral = A2;
const int sensorDerecho = A1;

// L293D #1 - Motores Delanteros
const int enableA1 = 11;  // Enable motor delantero derecho
const int in1 = 10;       // Input 1
const int in2 = 9;        // Input 2
const int enableB1 = 6;   // Enable motor delantero izquierdo
const int in3 = 8;        // Input 3
const int in4 = 7;        // Input 4

// L293D #2 - Motores Traseros
const int enableA2 = 5;   // Enable motor trasero derecho
const int in5 = 4;        // Input 1
const int in6 = 3;        // Input 2
const int enableB2 = 2;   // Enable motor trasero izquierdo
const int in7 = A3;       // Input 3
const int in8 = A4;       // Input 4

void setup() {
    Serial.begin(9600);
    
    // Configuración L293D #1
    pinMode(enableA1, OUTPUT);
    pinMode(enableB1, OUTPUT);
    pinMode(in1, OUTPUT);
    pinMode(in2, OUTPUT);
    pinMode(in3, OUTPUT);
    pinMode(in4, OUTPUT);
    
    // Configuración L293D #2
    pinMode(enableA2, OUTPUT);
    pinMode(enableB2, OUTPUT);
    pinMode(in5, OUTPUT);
    pinMode(in6, OUTPUT);
    pinMode(in7, OUTPUT);
    pinMode(in8, OUTPUT);
    
    // Configuración de sensores
    pinMode(sensorIzquierdo, INPUT);
    pinMode(sensorCentral, INPUT);
    pinMode(sensorDerecho, INPUT);
}

void loop() {
    int lecturaSensorIzquierdo = analogRead(sensorIzquierdo);
    int lecturaSensorCentral = analogRead(sensorCentral);
    int lecturaSensorDerecho = analogRead(sensorDerecho);
    
    Serial.print("Sensor Izquierdo: ");
    Serial.print(lecturaSensorIzquierdo);
    Serial.print(" - Sensor Central: ");
    Serial.print(lecturaSensorCentral);
    Serial.print(" - Sensor Derecho: ");
    Serial.println(lecturaSensorDerecho);
    
    if (lecturaSensorCentral > 340) {
        avanza();
    }
    else if (lecturaSensorIzquierdo > 340) {
        giraIzquierda();
    }
    else if (lecturaSensorDerecho > 340) {
        giraDerecha();
    }
    else if (lecturaSensorIzquierdo < 340 && lecturaSensorCentral < 340 && lecturaSensorDerecho < 340) {
        reversa();
    }
}

void avanza() {
    // Motor delantero derecho
    digitalWrite(in1, HIGH);
    digitalWrite(in2, LOW);
    analogWrite(enableA1, 200);
    
    // Motor delantero izquierdo
    digitalWrite(in3, HIGH);
    digitalWrite(in4, LOW);
    analogWrite(enableB1, 200);
    
    // Motor trasero derecho
    digitalWrite(in5, HIGH);
    digitalWrite(in6, LOW);
    analogWrite(enableA2, 200);
    
    // Motor trasero izquierdo
    digitalWrite(in7, HIGH);
    digitalWrite(in8, LOW);
    analogWrite(enableB2, 200);
}

void giraIzquierda() {
    // Motores derechos adelante
    digitalWrite(in1, HIGH);
    digitalWrite(in2, LOW);
    digitalWrite(in5, HIGH);
    digitalWrite(in6, LOW);
    
    // Motores izquierdos atrás
    digitalWrite(in3, LOW);
    digitalWrite(in4, HIGH);
    digitalWrite(in7, LOW);
    digitalWrite(in8, HIGH);
    
    // Velocidades
    analogWrite(enableA1, 200);
    analogWrite(enableB1, 150);
    analogWrite(enableA2, 200);
    analogWrite(enableB2, 150);
}

void giraDerecha() {
    // Motores derechos atrás
    digitalWrite(in1, LOW);
    digitalWrite(in2, HIGH);
    digitalWrite(in5, LOW);
    digitalWrite(in6, HIGH);
    
    // Motores izquierdos adelante
    digitalWrite(in3, HIGH);
    digitalWrite(in4, LOW);
    digitalWrite(in7, HIGH);
    digitalWrite(in8, LOW);
    
    // Velocidades
    analogWrite(enableA1, 150);
    analogWrite(enableB1, 200);
    analogWrite(enableA2, 150);
    analogWrite(enableB2, 200);
    delay(250);
}

void reversa() {
    // Todos los motores hacia atrás
    digitalWrite(in1, LOW);
    digitalWrite(in2, HIGH);
    digitalWrite(in3, LOW);
    digitalWrite(in4, HIGH);
    digitalWrite(in5, LOW);
    digitalWrite(in6, HIGH);
    digitalWrite(in7, LOW);
    digitalWrite(in8, HIGH);
    
    // Velocidades
    analogWrite(enableA1, 200);
    analogWrite(enableB1, 200);
    analogWrite(enableA2, 200);
    analogWrite(enableB2, 200);
}
