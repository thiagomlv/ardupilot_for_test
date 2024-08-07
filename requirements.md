## Crie os diretórios
1. Abra o prompt de comando e rode os códigos abaixo:
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

## Rode o simulador
1. Abra o prompt de comando na pasta `C:\ardu-sim` e rode o código abaixo:
```shell
arducopter -w -S --model + --speedup 1 --defaults parameters/copter.parm -I0
```
2. Abra outro prompt de comando na pasta `C:\ardu-sim` e rode o código abaixo:
```shell
mavproxy --master tcp:127.0.0.1:5760 --out 127.0.0.1:14550 --out 127.0.0.1:14560 --daemon --no-console --non-interactive
```