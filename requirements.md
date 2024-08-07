## Crie os diretórios
1. Abra o prompt de comando e rode os códigos abaixo para criar os diretórios `C:\ardu-sim` e `C:\ardu-sim\parameters`:
```shell
mkdir C:\ardu-sim
mkdir C:\ardu-sim\parameters
```


## Instale os programas requeridos
1. Instale [Git](https://github.com/git-for-windows/git/releases/download/v2.45.1.windows.1/Git-2.45.1-64-bit.exe).
2. Instale [Python 3.12.5](https://www.python.org/ftp/python/3.12.5/python-3.12.5-amd64.exe).
3. Instale [MAVProxy](https://firmware.ardupilot.org/Tools/MAVProxy/MAVProxySetup-latest.exe).
4. Instale [Mission Planner](https://firmware.ardupilot.org/Tools/MissionPlanner/MissionPlanner-latest.msi).

## Baixe os binários pré buildados
1. Baixe [ArduCopter.elf](https://firmware.ardupilot.org/Tools/MissionPlanner/sitl/CopterStable/ArduCopter.elf) e salve na pasta `C:\ardu-sim` e renomeie o arquivo para `arducopter.exe`.
2. Baixe [cygwin1.dll](https://firmware.ardupilot.org/Tools/MissionPlanner/sitl/CopterStable/cygwin1.dll) e salve na pasta `C:\ardu-sim`
3. Baixe [cygstdc++-6.dll](https://firmware.ardupilot.org/Tools/MissionPlanner/sitl/CopterStable/cygstdc++-6.dll) e salve na pasta `C:\ardu-sim`.
4. Baixe [cyggcc_s-seh-1.dll](https://firmware.ardupilot.org/Tools/MissionPlanner/sitl/CopterStable/cyggcc_s-seh-1.dll) e salve na pasta `C:\ardu-sim`.

## Baixe o arquivo de parâmetros
1. Baixe [copter.parm](https://raw.githubusercontent.com/ArduPilot/ardupilot/master/Tools/autotest/default_params/copter.parm) entrado no link e apertando ctrl + s e salve na pasta `C:\ardu-sim\parameters`.

## Instale os módulos de python requeridos
1. Abra o prompt de comando na pasta `C:\ardu-sim` e rode o códiogo abaixo:
```shell
python -m pip install pymavlink==2.4.41 dronekit==2.9.2 geopy==2.3.0 get-key==1.60.0
```

## Rode a simulação
1. Abra o prompt de comando na pasta `C:\ardu-sim` e rode o código abaixo:
```shell
arducopter -w -S --model + --speedup 1 --defaults parameters/copter.parm -I0
```
A seguinte mensagem deve ser exibida, indicando que o sistema está pronto para estabelecer a conexão com o drone:
```shell
Setting SIM_SPEEDUP=1.000000
Suggested EK3_DRAG_BCOEF_* = 16.288, EK3_DRAG_MCOEF = 0.209
Starting sketch 'ArduCopter'
Starting SITL input
Using Irlock at port : 9005
bind port 5760 for SERIAL0
SERIAL0 on TCP port 5760
Waiting for connection ....
```

2. Abra outro prompt de comando na pasta `C:\ardu-sim` e rode o código abaixo:
```shell
mavproxy --master tcp:127.0.0.1:5760 --out 127.0.0.1:14550 --out 127.0.0.1:14560
```
Se a conexão for corretamente estabelecida, uma nova janela vai se abrir onde mostra os parâmetros do drone simulado em tempo real, no terminal abrirá um espaço para digitação com a seguinte mensagem:
```shell
Connect tcp:127.0.0.1:5760 source_system=255
Loaded module console
Running script (C:\Users\thiago\AppData\Local\.mavproxy\mavinit.scr)
Loaded module help
Unknown command 'graph timespan 30'
Log Directory:
Telemetry log: mav.tlog
Waiting for heartbeat from tcp:127.0.0.1:5760
Detected vehicle 1:1 on link 0
Received 1390 parameters (ftp)
Saved 1390 parameters to mav.parm
STABILIZE>
```
O cursor será posicionado no `STABILEZE>`. Esse "STABILIZE" indica o modo de operação do drone. 

3. Para visualizar os movimentos que o drone vai realizar podemos renderizar um mini mapa. Rode (no `STABILIZE>`) o comando:
```shell
module load map
```
Uma nova janela se abrirá exibindo o mapa.

4. Para rodarmos nossos próprios scripts em python o drone devera está no modo "GUIDED". Para mudar para o modo guided, rode (no `STABILIZE>`) o comando:
```shell
mode guided
```
Após isso o `STABILEZE>` mudará para `GUIDED>`. 

5. O motor deve está armado para o drone voar. Para isso espere aparecer na nova janela que se abriu (a que mostra os parâmetros do drone), a mensagem "pre arm good". Pode demorar um pouco. Após aparecer essa mensagem, rode (no `GUIDED>`) o comando:
```shell
arm throttle
```
e será exibido "ARMED" na janela do drone. Após isso, está tudo pronto para você testar seu script

6. Abra outro prompt de comando e rode seu script em python.

7. Observações: 
- Se o seu script em python já se encarrega de armar os motores, o passo 5 é dispensável. 
- Certifique-se de conectar o drone no script no mesmo ip que o primeiro `--out` do passo 2.