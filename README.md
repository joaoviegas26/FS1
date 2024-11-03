# Recruits Task - Week #2
- ## [git para noobs](https://hackmd.io/@PedroRomao/HJ0GJSae1x)
- ## Add this file to your local git repository create a new branch and work on it, when you are done or as you complete the questions merge the branch into the main branch (remote main branch will have your final work, this means main branch on GitHub, please make the repository public for the next weeks).
- ## Perform the following tasks to the best of your hability, sometimes, in the questions, there are multiple answers just tell us what you think, feel free to use the group to ask questions.

### 1
**1.** Check out the [el-sw repository](https://github.com/fs-feup/el-sw/tree/main) code and documentation  and try to generally understand what the software does in each device (there is no need to understand all the little details).
### 2
When we read values from the brake sensor (C1) and the apps (C3) we do not use the most recent reading and use instead a different approach. Explain the approach and why you think it is used.

**Answer:** Ao invés de se usar a leitura atual dos sensores, é feita uma média de várias leituras (atual e anteriores). Deste modo é possível ter uma estimativa mais exata dos parametros que se querem medir, evitando erros muito usuais em sensores analógicos.

No Brake sensor (C1) é feita a média das últimas 20 leituras,armazenadas num buffer, com intervalos entre elas de 20ms, permitindo ter o valor médio do sensor nos últimos 400ms.

Já nos apps (dois sensores), são armaznados em dois buffers de 5 elementos cada, as leituras de cada sensor. Estas leituras são efetuadas em intervalos de 20ms, pelo que as médias, guardadas em v_apps1 e v_apps2 são médias dos últimos 100ms.


### 3
Check out the R2D(Ready To Drive) code on the C3 state machine. In the condition below we use a timer (R2DTimer) to check the brake was engaged instead of just checking the brake pressure received from can, why?
```c++
        if ((r2dButton.fell() and TSOn and R2DTimer < R2D_TIMEOUT) or R2DOverride)
        {
            playR2DSound();
            initBamocarD3();
            request_dataLOG_messages();
            R2DStatus = DRIVING;
            break;
        }
```

**Answer:** Esta parte da statemachine é a condição para o R2DStatus ficar em DRIVING. Só ocorre se o R2Button for premido, o Tractive System estiver On e o R2DTIMER for menor do que R2D_TIMEOUT. O timer é inicializado sempre que o brake value é maior que 165.Desta forma há um intervalo de R2D_TIMEOUT ms para largar o botão desde que o sinal brakevalue ultrapassou os 165.
### 4
What is the ID of the can message sent to the bamocar to request torque?
**Answer:** 0x201
### 5 
The code below is not amazing, tell us some things you would change to improve it, you can write them down in text or correct the code:
```c++
// this is a class for my car
#define HYDRAULIC_PRESSURE_PIN 0
#define TEMPERATURE_PIN 1
#define HUMIDITY_PIN 2
#define LIGHT_PIN 3
#define SOUND_PIN 4
#define DISTANCE_PIN 5
#define ACCELEROMETER_PIN 6
#define GYROSCOPE_PIN 7
//Definir os pinos para ser mais fácil fazer alterações se necessário.

class MyCar {
private:
    std::map<String, int> sensorReadings; 
    //Utilização de um mapa. Desta forma guardamos todas as leituras de forma compacta mas podendo aceder a cada uma através dos parametros medidos.

public:
    MyCar() {
        
        sensorReadings["Hydraulic Pressure"] = 0;
        sensorReadings["Temperature"] = 0;
        sensorReadings["Humidity"] = 0;
        sensorReadings["Light"] = 0;
        sensorReadings["Sound"] = 0;
        sensorReadings["Distance"] = 0;
        sensorReadings["Accelerometer"] = 0;
        sensorReadings["Gyroscope"] = 0;
        //construtor que inicializa todas as leituras a zero.
    }

    
    void updateAndPrint() {
        sensorReadings["Hydraulic Pressure"] = analogRead(HYDRAULIC_PRESSURE_PIN);
        sensorReadings["Temperature"] = analogRead(TEMPERATURE_PIN);
        sensorReadings["Humidity"] = analogRead(HUMIDITY_PIN);
        sensorReadings["Light"] = analogRead(LIGHT_PIN);
        sensorReadings["Sound"] = analogRead(SOUND_PIN);
        sensorReadings["Distance"] = analogRead(DISTANCE_PIN);
        sensorReadings["Accelerometer"] = analogRead(ACCELEROMETER_PIN);
        sensorReadings["Gyroscope"] = analogRead(GYROSCOPE_PIN);
        //atualizar todas as leituras.

        printSensorReadings(); 
    }

    
    void printSensorReadings() {
        for (const auto& reading : sensorReadings) {
            Serial.print(reading.first); 
            Serial.print(" Reading: ");
            Serial.println(reading.second); 
        }
        //através do for conseguimos condensar o código.
    }
};
```

