# Ares
Um pequeno robô com um chassi impresso em 3D com duas rodas, 3 sensores de distância ultrassónicos HC-SR04 , um sensor de linha refletor infravermelho TCRT5000, uma placa Arduino Uno R3, uma ponte H L298N, uma bateria de 9 volts, duas baterias conectadas de 3.7 volts, um cabo USB 2.0 A/B e um adaptador de bateria 9V com plug P4.
# Código 
int TRIG_F = 7;
int ECHO_F = 4;

int TRIG_E = 6;
int ECHO_E = 5;

int TRIG_D = A0;
int ECHO_D = A1;

int IR_E = 3;
int IR_D = 2;

int IN1 = 8;
int IN2 = 9;
int IN3 = 10;
int IN4 = 11;

long distancia(int trig, int echo) {
  digitalWrite(trig, 0);
  delayMicroseconds(2);
  digitalWrite(trig, 1);
  delayMicroseconds(10);
  digitalWrite(trig, 0);

  long duracao = pulseIn(echo, 1, 20000);
  long dist = duracao * 0.034 / 2;
  return dist;
}

void frente() {
  digitalWrite(IN1, 1);
  digitalWrite(IN2, 0);
  digitalWrite(IN3, 1);
  digitalWrite(IN4, 0);
}

void tras() {
  digitalWrite(IN1, 0);
  digitalWrite(IN2, 1);
  digitalWrite(IN3, 0);
  digitalWrite(IN4, 1);
}

void esquerda() {
  digitalWrite(IN1, 0);
  digitalWrite(IN2, 1);
  digitalWrite(IN3, 1);
  digitalWrite(IN4, 0);
}

void direita() {
  digitalWrite(IN1, 1);
  digitalWrite(IN2, 0);
  digitalWrite(IN3, 0);
  digitalWrite(IN4, 1);
}

void setup() {
  pinMode(TRIG_F, OUTPUT);
  pinMode(ECHO_F, INPUT);
  pinMode(TRIG_E, OUTPUT);
  pinMode(ECHO_E, INPUT);
  pinMode(TRIG_D, OUTPUT);
  pinMode(ECHO_D, INPUT);

  pinMode(IR_E, INPUT);
  pinMode(IR_D, INPUT);

  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);
}

void loop() {
  int irE = digitalRead(IR_E);
  int irD = digitalRead(IR_D);

  long dF = distancia(TRIG_F, ECHO_F);
  long dE = distancia(TRIG_E, ECHO_E);
  long dD = distancia(TRIG_D, ECHO_D);

  if (irE == 0 || irD == 0) {
    tras();
    delay(300);
    direita();
    delay(300);
  }
  else if (dF > 0 && dF < 40) {
    frente();
  }
  else if (dE > 0 && dE < 40) {
    esquerda();
  }
  else if (dD > 0 && dD < 40) {
    direita();
  }
  else {
    direita();
  }
}

